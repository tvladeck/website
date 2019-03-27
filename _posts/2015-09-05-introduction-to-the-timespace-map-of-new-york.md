---
id: 350
title: Introduction to the Timespace Map of New York
date: 2015-09-05T19:50:17+00:00
author: tvladeck
layout: post
guid: http://tomvladeck.com/?p=350
permalink: /2015/09/05/introduction-to-the-timespace-map-of-new-york/
dsq_thread_id:
  - "4101003689"
categories:
  - Data
  - Visualization
---
[2017-12-07: <a href="http://blog.gradientmetrics.com/2017/12/07/new-york-city-in-timespace/" target="_blank">This has been updated in a big way</a>.]

This summer I lived in New York for the first time, working for Venmo. Venmo's offices are in the West Village, and I lived in Park Slope, so every day I would take the F train. As I was getting to know the city I would often look at this map:

<a href="https://upload.wikimedia.org/wikipedia/commons/3/3a/Official_New_York_City_Subway_Map_vc.jpg"><img class="alignnone" src="https://upload.wikimedia.org/wikipedia/commons/3/3a/Official_New_York_City_Subway_Map_vc.jpg" alt="" width="278" height="333" /></a>

&nbsp;

Looking at that map every day got me thinking that so much of the geography of the city was defined by its public transit infrastructure, not by the lay of the land. What determined if two points were practically far or close depended not on how many miles separated them, or natural features like rivers that may lay between them, but whether or not it was easy to get from one place to the other on public transit.

I started thinking about how we could make a new map of New York, where distances between points on the map would reflect how <strong>long</strong> it took to get between them, not how <strong>far</strong> apart they were. It would project time onto the two-dimensional space of the map.

Without knowing how I would do that, I started collecting data. I took the 195 <a href="http://www.nyc.gov/html/dcp/pdf/bytes/nynta_metadata.pdf" target="_blank">neighborhood tabulation areas</a> of New York, geocoded them into Lat/Long pairs, and started hitting <a href="https://developers.google.com/maps/documentation/directions/intro" target="_blank">Google's Directions API</a> for each of the pairs of neighborhoods to see how long the transit route between them took. After doing this over many days in batches (because Google limits how many directions you can ask for any given day), I had a distance matrix that looked like this:

&nbsp;

<a href="http://tomvladeck.com/wp-content/uploads/2015/09/Screenshot-2015-09-05-15.20.20.png"><img class="alignnone wp-image-351 " src="http://tomvladeck.com/wp-content/uploads/2015/09/Screenshot-2015-09-05-15.20.20.png" alt="Screenshot 2015-09-05 15.20.20" width="769" height="138" /></a>

&nbsp;

With the cells being the time (in seconds) it took to get between the two locations.

Next, I needed to figure out how to lay out these points in a two-dimensional space that reflected these distances in time. Turns out, this is a common problem in data visualization, and the most common tool is called <a href="https://en.wikipedia.org/wiki/Multidimensional_scaling" target="_blank">multidimensional scaling</a>. Running <code>cmdscale</code> (in <code>R</code>) on my distance matrix allowed me to lay out the "neighborhoods" of New York onto a 2d map with distances between them reflecting time.

With just this information, about the best I could do was to create a <a href="https://en.wikipedia.org/wiki/Voronoi_diagram" target="_blank">Voronoi diagram</a> of the points scattered about.

&nbsp;

<a href="http://tomvladeck.com/wp-content/uploads/2015/09/voronoi.png"><img class="alignnone size-large wp-image-353" src="http://tomvladeck.com/wp-content/uploads/2015/09/voronoi-1024x1024.png" alt="voronoi" width="525" height="525" /></a>

Cool, but we can do better.

I started thinking about these 195 points as a "skeleton" that I could build on. I wanted to project more information onto this map, but I couldn't just go on adding points to my distance matrix (since the number of entries in the distance matrix goes up with the square of the number of neighborhoods), I had to figure out how to project an arbitrary Lat/Long pair onto this map.

I tried a few things out, but the method I settled on was <a href="https://en.wikipedia.org/wiki/Local_regression" target="_blank">LOESS regression</a> as it was simple, made few assumptions, and could handle the obvious nonlinearities in the transformation between Lat/Long space and Timespace.

The model was simple (this is the model to predict one axis of the timespace dimension):

<code> model.dim1 &lt;- loess(data = model.data,
formula = dim1 ~ lat + lng,
degree = 2,
span = span)
</code>

And the <code>span</code> term allows you to control how much impact the skeleton points have in "pulling" the new Lat/Long points away from their starting point. Here is a gif of different projections of the five boroughs with <code>spans</code> ranging from <code>0.99</code> to <code>0.2</code>: (there a few errors in the borough shapefiles that I'm aware of...)

<a href="http://tomvladeck.com/wp-content/uploads/2015/09/animation2.gif"><img class="alignnone wp-image-355 size-full" src="http://tomvladeck.com/wp-content/uploads/2015/09/animation2.gif" alt="animation2" width="2625" height="2625" /></a>

So what's next?

Well, now that I have a method of projecting arbitrary Lat/Long pairs onto the "Timespace" dimension, I am working on doing it with other data sources. I'm starting with subways, but plan to add other features like roads, parks, and the like. Stay tuned!