---
layout: post
title: Working around the performance problem of STUnion
date: 2014-02-07 00:00:00
category: performance
tags: SqlGeography algorithm
---
<p class="jumbotron">
	When life gives you an <a href="http://en.wikipedia.org/wiki/Big_O_notation"><em>O(n<sup>k</sup>)</em></a> algorithm, it's time to get creative
	and respond with your own logarithmic algorithm.
</p>

At [DevResults](http://devresults.com) we do a lot of work with maps. One such activity involves <abbr title="Keyhold Markup Language">kml</abbr>
shape files of things like countries, states / provinces, cities, etc. While learning how to set up a new site,
we grabbed the shape files for the United States off of [GADM](http://gadm.org/) and started the import process.

An hour later, the process still hadn't finished the first (and largest) shape file.
Something was definitely wrong and so I dug in and found out where the code was stuck at and found that
the code was iterating through all the points and combining them together into one big [SqlGeography](http://technet.microsoft.com/en-us/library/microsoft.sqlserver.types.sqlgeography.aspx) instance
via the [STUnion method](http://technet.microsoft.com/en-us/library/microsoft.sqlserver.types.sqlgeography.stunion.aspx). 
Below is an example of the code that was doing this

```csharp
SqlGeography shape = polygons[0];
for (int i = 1; i < polygons.Length; i++) {
	shape = shape.STUnion(polygons[i]);
}

return shape;
```

When I started searching around for some alternatives to STUnion, I found this graph:

<img src="http://i.imgur.com/JXdHk0H.png">

Classic <code>O(n<sup>k</sup>)</code> problem from the look of it.

### Taming the beast

From the graph, we know that we can combine small shapes very quickly, and large shapes take a very long time.
This means for our algorithm we want to <strong>minimize the number of operations we do on large shapes</strong>.
With this optimization goal in mind, we can make a divide and conquer algorithm that will combine shapes 
into increasingly larger shapes where finally at the end we only have two shapes to merge.

First, lets get the implementation out of the way.

```csharp
public static SqlGeography CombinePolygons(List<SqlGeography> polygons, int start, int end)
{
    if (start > end) return null;
    if (start == end) return polygons[start];

    int midpoint = (start + end) / 2;

    SqlGeography left = CombinePolygons(polygons, start, midpoint);
    SqlGeography right = CombinePolygons(polygons, midpoint + 1, end);

    // if both values have shapes, combine
    if (left != null && right != null)
        return left.STUnion(right);

    // if only one has a shape, pick whichever has a value
    return left ?? right;
}
```

So, the way this works is it recursively splits the list of polygons until it gets down to two single polygons,
unions them together, then returns that shape, which then gets unioned with another, until in the end we have one shape.
This means that most of our operations are done on small objects, thus minimizing the large objects we have to merge.

For example, consider 512 polygons.

<table class="table table-striped">
	<thead>
		<tr><th>Operation Count</th><th>Left Size</th><th>Right Size</th></tr>
	</thead>
	<tbody>
		<tr><td>256</td><td>1</td><td>1</td></tr>
		<tr><td>128</td><td>2</td><td>2</td></tr>
		<tr><td>64</td><td>4</td><td>4</td></tr>
		<tr><td>32</td><td>8</td><td>8</td></tr>
		<tr><td>16</td><td>16</td><td>16</td></tr>
		<tr><td>8</td><td>32</td><td>32</td></tr>
		<tr><td>4</td><td>64</td><td>64</td></tr>
		<tr><td>2</td><td>128</td><td>128</td></tr>
		<tr><td>1</td><td>256</td><td>256</td></tr>
	</tbody>
</table>

We can easily see that we are now minimizing the number of operations that we are doing on large shapes.

### Reviewing the solution

So how well does this work out? Well the first time I ran this, it never finished the first shape 
file in <strong>over and hour and a half</strong> while this algorithm lets me process this shape file 
in <strong>just under 4 minutes</strong>. A great result from a simple change in how we process the
polygon shapes.