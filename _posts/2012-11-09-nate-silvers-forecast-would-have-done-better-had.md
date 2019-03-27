---
id: 16
date: 2012-11-09T16:33:00+00:00
author: tvladeck
layout: post
guid: http://tomvladeck.com/?p=16
permalink: /2012/11/09/nate-silvers-forecast-would-have-done-better-had/
tumblr_tomvladeck_permalink:
  - http://tomvladeck.tumblr.com/post/35342490414/nate-silvers-forecast-would-have-done-better-had
tumblr_tomvladeck_id:
  - "35342490414"
dsq_thread_id:
  - "1475617126"
categories:
  - Uncategorized
tags:
  - "538"
  - Prediction
format: gallery
---
[gallery]
<p>Nate Silver&#8217;s forecast would have done better had he gotten a few wrong. </p>
<p>I really like 538 and they have done a great job. I have one minor quibble with how they represent their forecasts and their results. 538&#8217;s forecasts are <em>not</em> binary predictions, they are probability estimates. Meaning, that of all the races he projects to be at 70% probability, the winner should be the leading candidate 70% of the time - not 100% of the time. But 538 and many others are saying that he got 50/50 states right, and that confuses the picture a lot. What does it mean to call Florida &#8220;correctly" when the model gave Obama a 50.1 chance of winning?</p>
<p>The example I used when explaining it to some friends was that if a weatherman, for 10 straight days, said there was a 55% chance of rain, and it rained every day - you could say that the weatherman correctly predicted the weather every day, but the correct interpretation is that his estimates were too low. </p>
<p>The code below simply takes 538&#8217;s estimates right before the election, with the probability figure being that which 538 assigned to the eventual winner. The code simply assumes that 538&#8217;s probability estimates were exactly correct, and shows the distribution of states that would be in error in a run of 10,000 simulations. As you can see, if 538&#8217;s estimates were exactly correct, it&#8217;s  significantly more likely that he would have gotten 1, 2, or 3 states wrong than none at all. </p>
<p>This is not to say that his estimates <em>weren&#8217;t</em> spot on - there is a good chance they were - I only want to point out that there is a little bit more going on than saying that 538 got 50/50 states &#8220;right".</p>
<div class="gist"><a href="https://gist.github.com/4046576">https://gist.github.com/4046576</a></div>