---
layout: post
title: The most amazing keyboard command for Sql Server Management Studio
date: 2014-03-13 19:00:00
category: tips
tags: sql-server-management-studio
---
<p class="jumbotron">
	I learned the most amazing shortcut today that likely would have saved me years of time in
	the past. Full credit goes to my co-worker 
	<a href="http://devresults.com/en/p/about">Brent Keller</a> for this.
</p>

While doing some pair-programming with my co-worker Brent today I noticed something peculiar happen
on his screen. It looked like he executed the query, but the results did not match what the query
was doing at all. On the contrary, it was a large amount of meta-data about a table.

How is this possible? All you need to do is an object in the query editor and press 
<kbd>Alt</kbd> + <kbd>F1</kbd>, then you will see a lot of useful meta-data in the query results
window. No need to go traverse the tree in object explorer anymore!

* When you execute this on a table, you'll get very useful information such as information about the
columns, indexes, constraints, and who references the table.
* When you execute this on a table function you get the output columns and the parameters. 
* When you run this on a view you get the output columns.
* When you execute it on stored procedures and scalar functions you get the input parameters.
* When you execute it on types (I assume this works on user-defined types as well) you get information
about the type.
* I'm sure there are many other object types that this shortcut will work on

Hopefully this information will make you as happy it made me.