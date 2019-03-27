---
id: 629
title: Microsoft should provide an R environment in Excel (and buy RStudio to help do it)
date: 2018-11-08T00:48:03+00:00
author: tvladeck
layout: post
guid: http://tomvladeck.com/?p=629
permalink: /2018/11/08/microsoft-should-provide-an-r-environment-in-excel-and-buy-rstudio-to-help-do-it/
dsq_thread_id:
  - "7027566344"
categories:
  - Uncategorized
---
Excel doesn't seem to be going anywhere, and nor should it — it's a great tool, and the best one for countless use cases. At the same time, analyses that would previously live in Excel are now being done in R and Python, and many more analyses that would previously not have not been done at all are now being done in R and Python.

(Everything I'm about to write applies to both R and Python equally, but I know the R space a lot better.)

At the same time, the consumers of these analyses are often not code-literate. But they want to feel ownership of the analysis, be able to play with assumptions, etc. They don't just want the results handed to them. This is a core benefit of Excel.

Additionally, while un- or semi-structured data (e.g. natural text, JSON files) and media (images, audio, video) are becoming more common as the subject of analyses, tabular data is, and will be, the most common data format for the foreseeable future. Excel handles these formats natively and transparently as worksheets.

I can't tell you how many times I would have loved to have done the following for a client:
<ol>
 	<li>Store the raw data as (a) worksheet(s)</li>
 	<li>Have a key assumptions sheet that lists the key inputs for an analysis, which the user can change</li>
 	<li>Have R scripts that read from the assumptions and the raw data, and produce outputs (charts, statistics, tables, etc) right into Excel</li>
 	<li>Have a button that, when pressed, sources a script (that can of course call into other scripts)</li>
</ol>
Obvious bonus points would be if you could fire up an R editor right within Excel that could refer to worksheets and cells inside of its environment as just another tab inside of Excel. In short, why can't Excel look like this:

<a href="http://tomvladeck.com/wp-content/uploads/2018/11/Screenshot-2018-11-07-19.20.30.png"><img class="alignnone size-full wp-image-630" src="http://tomvladeck.com/wp-content/uploads/2018/11/Screenshot-2018-11-07-19.20.30.png" alt="Screenshot 2018-11-07 19.20.30" width="964" height="734" /></a>

Of course, it would be great if the text files could be used with version control like Git.

Obviously, this would not be straightforward to do, but Microsoft could make it happen and build Excel for the next generation. It would also open up a zillion new use cases and deliverable formats for internal or external consulting firms (like mine).

Why do I make the additional assertion that Microsoft should buy RStudio? Because <a href="https://stackoverflow.blog/2017/10/10/impressive-growth-r/" target="_blank">R is growing like crazy</a>, and RStudio and Jupyter are becoming the default environments for doing analysis in R. But Jupyter is an open source organization, so not something Microsoft can buy, while RStudio is a company focused on supporting its development products.

If Microsoft wanted to catch up to the curve in terms of where the best teams do their analysis (no longer Excel), it should find a way to own the R space.

And it is trying to do that — it has <a href="https://blog.revolutionanalytics.com/2018/02/what-does-microsoft-do-with-r.html" target="_blank">an entire R ecosystem</a>. But even as someone that works day-in, day-out in R, and follows everything going on in R, I have no idea how it works. It feels to me that it's aimed at IT managers in enterprises and not the individual data scientists that are doing the most to drive adoption within those companies. It's not the right approach — as they know, because they bought Github, whose primary adoption vector was through individual (or small team) adoption within larger enterprises.

If I were in corporate development and/or product strategy at Microsoft, this would be a no-brainer to me.