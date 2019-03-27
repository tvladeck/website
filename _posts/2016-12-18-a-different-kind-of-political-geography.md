---
id: 435
title: A different kind of political geography
date: 2016-12-18T16:34:09+00:00
author: tvladeck
layout: post
guid: http://tomvladeck.com/?p=435
permalink: /2016/12/18/a-different-kind-of-political-geography/
dsq_thread_id:
  - "5390432974"
categories:
  - Uncategorized
---
In the wake of the surprising (to say the least) electoral result I initiated a few projects to try and understand the politics of the country. One thing I wanted to understand was the impact of demographic and underlying situational variables (e.g. health, income, unemployment, etc.) on how people voted. Was the vote about Obamacare? Was it about lost jobs? Was it all education levels? Or was it all racism? Theories have been floated but I haven't seen a rigorous evaluation of these hypotheses. What's below is just an exploratory analysis, but the data does point in some interesting directions.

What follows below are a series of visualizations of a <a href="https://github.com/Deleetdk/USA.county.data" target="_blank">large, aggregated dataset of both demographic, situational, and electoral data</a>. Sources for the demographic and situational data are listed <a href="https://openpsych.net/paper/12" target="_blank">here</a> and the electoral data is from the New York Times.

The type of visualization is called a <a href="https://en.wikipedia.org/wiki/Self-organizing_map" target="_blank">self organized map</a>. Roughly speaking, each hexagon is a group of counties; the map arranges the counties such that similar ones are closer to each other on the map and dissimilar ones further apart:

<a href="http://tomvladeck.com/wp-content/uploads/2016/12/hs_diploma-1.svg"><img class="alignnone size-large wp-image-513" src="http://tomvladeck.com/wp-content/uploads/2016/12/hs_diploma-1.svg" alt="hs_diploma" /></a>

For any given variable (here - the proportion of residents in the counties that graduated from high school) the map is a heatmap. Redder colors means the counties index higher, bluer means they index lower. Here, the upper right are the counties where fewer people have a high school diploma, and lower left are the most educated.

<strong><em>All of the maps shown below are available at <a href="https://gradient.shinyapps.io/geography_of_voting/" target="_blank">this interactive site</a>. (A very similar set of maps, but for voting swings as opposed to voting share, <a href="https://gradient.shinyapps.io/geography_of_swings/" target="_blank">is available here</a>)</em></strong>

Below, we look at the voting share for Hillary. The counties are arranged in the same way as above, but since we're looking at different variable the map is colored differently. (Confusingly, more votes for HRC are red as opposed to the customary blue for liberals, but work with me here). The reason this map is more organized than the rest is that I used this variable to "supervise" the organization (don't worry about the details of this - basically it just guaranteed that this particular coloring, which is the reference point, would be organized.)

<a href="http://tomvladeck.com/wp-content/uploads/2016/12/base_plot.svg"><img class="alignnone size-large wp-image-512" src="http://tomvladeck.com/wp-content/uploads/2016/12/base_plot.svg" alt="base_plot" /></a>

Now that we have the basics in place, we can look at other variables: let's check a few variables and see if they line up w/ the HRC voting share map. What we can do is draw a boundary around the areas that went strongly for Trump and for HRC like so:

<a href="http://tomvladeck.com/wp-content/uploads/2016/12/base_plot_w_annotation.svg"><img class="alignnone size-large wp-image-514" src="http://tomvladeck.com/wp-content/uploads/2016/12/base_plot_w_annotation.svg" alt="base_plot_w_annotation" /></a>

And we'll keep these annotations throughout.

<strong>Health and insurance:</strong>

The breakdowns for uninsurance and health variables like obesity and diabetes don't break down along electoral lines: the split goes in the opposite direction, with the highest uninsurance and low health areas going to both candidates:

<a href="http://tomvladeck.com/wp-content/uploads/2016/12/uninsured.svg"><img class="alignnone size-large wp-image-515" src="http://tomvladeck.com/wp-content/uploads/2016/12/uninsured.svg" alt="uninsured" /></a>

<a href="http://tomvladeck.com/wp-content/uploads/2016/12/adult_obesity.svg"><img class="alignnone size-large wp-image-516" src="http://tomvladeck.com/wp-content/uploads/2016/12/adult_obesity.svg" alt="adult_obesity" /></a>

<a href="http://tomvladeck.com/wp-content/uploads/2016/12/diabetes.svg"><img class="alignnone size-large wp-image-517" src="http://tomvladeck.com/wp-content/uploads/2016/12/diabetes.svg" alt="diabetes" /></a>

<strong>Economic variables:</strong>

These graphs should put the "economic anxiety" argument to rest, as the areas with highest unemployment went strongest to HRC and those with the least went strongest to Trump.

<a href="http://tomvladeck.com/wp-content/uploads/2016/12/unemploymen.svg"><img class="alignnone size-large wp-image-518" src="http://tomvladeck.com/wp-content/uploads/2016/12/unemploymen.svg" alt="unemploymen" /></a>

<a href="http://tomvladeck.com/wp-content/uploads/2016/12/earnings.svg"><img class="alignnone size-large wp-image-519" src="http://tomvladeck.com/wp-content/uploads/2016/12/earnings.svg" alt="earnings" /></a>

<strong>Ethnic Variables:</strong>

A few graphs line up pretty well: whiteness and ethnic homogeneity. And whiteness and ethnic homogeneity line up basically on top of each other. This would support the hypothesis that the election for Trump was mainly a cultural (and not a policy) event; white enclaves are reacting against a diminishing place in the cultural landscape - hence the making things great <strong>again</strong>:

<a href="http://tomvladeck.com/wp-content/uploads/2016/12/whiteness.svg"><img class="alignnone size-large wp-image-520" src="http://tomvladeck.com/wp-content/uploads/2016/12/whiteness.svg" alt="whiteness" /></a>

<a href="http://tomvladeck.com/wp-content/uploads/2016/12/homogeneity.svg"><img class="alignnone size-large wp-image-521" src="http://tomvladeck.com/wp-content/uploads/2016/12/homogeneity.svg" alt="homogeneity" /></a>

<strong>See for yourself: </strong>

<a href="https://github.com/tvladeck/election_som">My code is available here</a> (it is not very well commented or formatted, but it's there).

As mentioned above, <a href="https://gradient.shinyapps.io/geography_of_voting/">all the maps above are available at this site</a>. If you want to see something similar but with voting <strong>swings</strong> - the amount the county changed their vote from '12 to '16, you can see that <a href="https://gradient.shinyapps.io/geography_of_swings/">here</a>.

&nbsp;

&nbsp;