---
id: 597
title: Thinking in Prolog
date: 2018-09-24T12:50:23+00:00
author: tvladeck
layout: post
guid: http://tomvladeck.com/?p=597
permalink: /2018/09/24/thinking-in-prolog/
dsq_thread_id:
  - "6929602004"
categories:
  - Uncategorized
---
I've been having a lot of fun working on a side project written in <a href="https://en.wikipedia.org/wiki/Prolog" target="_blank" rel="noopener">Prolog</a>, which is a <em>logical</em> programming language. It has been incredibly difficult to wrap my mind around how it works but I'm getting there, and I can see that "there" is a pretty powerful place. It makes me wonder why this paradigm isn't used for more applications.

It turns out (you may already know this if you write code) that programming languages work <em>very differently</em> depending on which <a href="https://en.wikipedia.org/wiki/Programming_paradigm" target="_blank" rel="noopener">paradigm</a> they use.

From Wikipedia:

Common programming paradigms include:
<ul>
 	<li><strong>imperative</strong> in which the programmer instructs the machine how to change its state,
<ul>
 	<li><strong>procedural</strong> which groups instructions into procedures,</li>
 	<li><strong>object-oriented</strong> which groups instructions together with the part of the state they operate on,</li>
</ul>
</li>
 	<li><strong>declarative</strong> in which the programmer merely declares properties of the desired result, but not how to compute it
<ul>
 	<li><strong>functional</strong> in which the desired result is declared as the value of a series of function applications,</li>
 	<li><strong>logic</strong> in which the desired result is declared as the answer to a question about a system of facts and rules,</li>
 	<li><strong>mathematical</strong> in which the desired result is declared as the solution of an optimization problem</li>
</ul>
</li>
</ul>
I've found functional programming to be so intuitive and object-oriented (probably the most common paradigm today) to be so counterintuitive that I almost always write my programs in a functional style.

In a logic program, you write down a set of facts and rules. That's it. That's your program. And then you run the program by asking it if it can find a solution to a new query.

Prolog in particular is built around a type of logical statement called a <a href="https://en.wikipedia.org/wiki/Horn_clause" target="_blank">Horn clause</a> that basically reads "X is true if Y and Z are true". In Prolog that would look like:
<pre>X :- Y, Z.</pre>
A more complete program (but still tiny) program would look like:
<pre>food(sandwich). % declaring a fact that sandwich is a food
drink(water). % declaring that water is a drink

meal(X, Y) :- food(X), drink(Y).
% X and Y (variables) make a meal if X is a food &amp; Y is a drink</pre>
When you run this program, you'll get:
<pre>?- meal(X, Y).
X = sandwich,
Y = water.</pre>
That's a toy example, but you can see how quickly the complexity could build up with ever-more relationships and so on. I'm working on writing a program to solve the common wedding-seating problem where you have a set number of tables, people you want to sit next to each other, and people that you want to sit at different tables. It's fun.