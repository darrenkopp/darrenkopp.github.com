---
layout: post
title: "Faster Lambda Http Performance when connecting to AWS Application Load Balancer"
date: 2018-11-15 00:00:00
categories: software
tags: aws lambda alb
comments: true
---

My current project at [Homesnap](https://homesnap.com) involves breaking a monolith application into multiple microservices and part of that is moving a large amount of data to that new microservice by way of feeding the data through our new API.

While I'm able to get reasonable throughput by firehosing the API with data from my machine, it simply doesn't have enough throughput to move the volume of data necessary in any reasonable amount of time, so I decided to queue the data using Amazon's Simple Queue Service (SQS) with a Lambda function being triggered from data being written to the Queue. In theory, this allows me to scale out the data ingestion using Amazon's capacity rather than me finding more and more machines to run my import utility on.

What I found, however, was that Lambda, with 80 concurrent executions sending 10 requests per batch, could barely outperform my single machine. My setup was pretty basic: a stateless, HTTP-based API behind an AWS Application Load Balancer (ALB); I would expect Lambda would achieve linear growth until the database's resources were exhausted. So I began tinkering until I got the performance I was expecting.

## 1. Sticky Sessions

While I wouldn't expect sticky sessions to be necessary (since my application is completely stateless), I wasn't able to get reasonable performance from my machine or Lambda to my API through ALB without sticky sessions enabled. While this improved performance significantly on my machine, Lambda still suffered, so I kept digging.

## 2. Get node http to use sticky sessions

My Lambda function was just a simple node function that was using the node http functionality to send the requests to the API. However, unlike client-side javascript using the `fetch` API, the http library doesn't automatically store / send cookies for the request. Thus, while sticky sessions were enabled, the Lambda function was never benefitting from those sticky sessions! Fixing this fact is fairly trivial, once you know what needs to be done.

<div class="alert alert-info">
    When Lambda loads your node function, it executes your script once, but your exported handler function repeatedly until the process shuts down. We'll exploit this fact in the code snippet below.
</div>

``` javascript
const http = require('http');
// initialize our cookie to an empty array
let cookie = [];

function sendRequest(data) {
    return new Promise((resolve, reject) => {
        const request = http.request({
            host: 'example.com',
            path: '/api',
            method: 'POST',
            headers: {
                // Pass our cookies to the request
                "Cookie": cookie
            }
        }, (response) => {
            // when we receive a response, store the cookies returned from the server into
            // our cookie variable. note that a cookie is a semi-colon delimited string of properties
            // but the only part we want to send up to the server is the first part.
            let cookie = (response.headers["set-cookie"] || []).map(v => v.split(';')[0]);
        });

        request.end(data);
    });
}

exports.handler = async (event, context, callback) => {
    await sendRequest('{ "hello": "world" }');
};
```

The result? My function went from ~2,000 invocations per minute to ~9,500 invocations minute and the maximum duration of my function dropped from ~22 - 28 seconds to ~6 seconds. Additionally, I was able to reduce the number of docker images running in the cluster from 20 to 6 while sustaining the same throughput. All in all, everything is running faster with lower cost, which makes me happy.

Hopefully this can help you if you are in a similar situation.