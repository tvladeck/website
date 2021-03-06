---
id: 83
title: Introducing v10r
date: 2013-09-13T19:24:01+00:00
author: tvladeck
layout: post
guid: http://tomvladeck.com/?p=83
permalink: /2013/09/13/introducing-v10r/
dsq_thread_id:
  - "1756830564"
categories:
  - Uncategorized
---
<h1>A less impractical many-event conditional prediction market</h1>
<h3>What is a prediction market?</h3>
A prediction market is a market where there is a binary payoff based on whether or not an event occurs or not. Take, for example, an election: you might place a bet that a democrat wins the next presidential election. If you win, you get a dollar. If you lose, you get nothing. What would you pay today to buy that payoff?

A prediction market is a clearinghouse for these types of securities. The most famous was Intrade, which is now defunct. Someone who thought the probability of Obama winning the election was 70% could buy that a security that would pay a dollar if Obama won from another person who thought the probability was only 50%. The buyer might pay the seller $0.60 for a $1.00 payoff if Obama won.
<h3>What is a conditional prediction market?</h3>
Ok, so hopefully this is relatively clear in the case of one event either happening or not (i.e., a democrat winning the next presidential election). What about, though, if you could specify conditions that activated the bet? i.e., something like “a democrat will win the next election if the unemployment rate is less than 8%”.

As you might guess, this is significantly more complicated. You might have noticed, though, that the equations above did not depend on the length of q (the vector of events in question). In this case, if you had a market with four events:
<ul>
	<li>A: Democrat wins AND unemployment rate less than 8%</li>
	<li>B: Democrat wins AND NOT unemployment rate less than 8%</li>
	<li>C: Democrat loses AND unemployment rate less than 8%</li>
	<li>D: Democrat loses AND NOT unemployment rate less than 8%</li>
</ul>
You could transact the bet above by having the trader simultaneously buy A and sell C. You can work out the math, but it works out that the amount the trader is “paying” for A and is “receiving” for C are the same, so there is no money transacted. If the unemployment rate is greater than 8%, the trader will lose on A and win on C (regardless of what happens in the election), so in effect the trade won't have happened: the trader won't have paid anything, and won't have received anything.

But if the unemployment rate is less than 8%, the trader must either win or lose. If the trader wins on A, she'll lose on C, and vice versa. So the bet only becomes “live” if the condition is satisfied.

One thing you probably noticed: even though we have two binary events, now the market is having to keep track of four atomic events. This is a central challenge of conditional prediction markets: with N independent binary events, the number of atomic events you have to track is 2^N - it rises exponentially.

This creates two problems: the first is that with so many events, the probability that there will be a counterparty for any one of the many things you want to trade is very low; second, as you add more and more events, there becomes huge computational overhead to tracking the atomic events.

The first problem is solved by having a market maker ready to price any trade and accept the trade at that price - no other counterparty required. The second is more tricky, and I'll get to that in a bit.
<h3>Market scoring rules</h3>
An automated prediction market is a step up in sophistication. In most prediction markets - Intrade was an example - the buyer and seller are people and the clearinghouse is taking no part of the trade. It's just listing the available trades. In an automated prediction market, the clearinghouse is the counterparty to<b> all</b> trades.

To do this, the clearinghouse has to keep track of what the prices are for each security. To do this, it will use a <b>market scoring rule</b> that keeps track of all the securities for sale, and updates them as orders get served. As securities get bought, the price goes up, and as they get sold, the price goes down.

The most common form of market scoring rule (common in the sense of appearing in academic journals - there aren't that many of these prediction markets “in the wild”) is the <b><a href="http://hanson.gmu.edu/mktscore.pdf">logarithmic market scoring rule</a></b> created by Robin Hanson. The rule is:

C = beta * ln(sum (exp(q_i / beta)))

Sorry that's hard to read. C is the total amount of money that the clearinghouse has taken from market participants, q is the vector of all bets placed on each event. As you change the vector q, by buying or selling an event, you change C. The amount by which you change C is the price the clearinghouse charges the trader. Beta is a constant which defines the liquidity of the market. The higher the beta, the more liquid the market.

If you want to find the current price of a given event q_j, you just take the derivative of this cost function with respect to the event in the vector q. That is given by:

P(q_j) = exp(q_j/beta) / sum(exp(q_i/beta))

Note that we're summing over all q_i in q.

I'm sure you're asking, what if everyone only buys the event that ends up happening? Won't the market lose money? The answer, of course, is yes. This market will pretty much always lose money as more people buy the more probable event. But as more people buy a given event, the price goes up - lessening the amount the market-maker loses on that trade. If the first person buys the winning event for $0.50, the market-maker loses $0.50 on a $1.00 payout. But when the second person buys the same event for $0.60, the market-maker will only lose $0.40, and so on. If the price escalates more slowly (i.e., the market is more liquid), the market-maker will lose more; if the price escalates more quickly (i.e., if the second buyer had to pay, say $0.95 for a $1.00 payoff), the market-maker will lose less.

The total amount the market-maker can lose is this:

beta * ln(length(q))

Or, in plain english: the liquidity parameter times the log of the number of atomic events.

So that's interesting: the loss is bounded. What if you charged a fee for each trade, so that, given a sufficient volume, you could guarantee that you made covered your total loss. Theoretically, you can. We'll explore that in more detail.
<h3>The ambition</h3>
The overall ambition of this project is to create an automated conditional market with a large number of binary events. Imagine trades of the form “the average price of gas on November 1 will be above $4.50 nationally if WTI oil is above $XX a barrel and weather is unseasonably warm in October”. Or, whatever you like. And having a price for that security and a counterparty ready to trade.

Also, we want <b>a lot</b> of binary events. And we want the market to not lose money but also to not charge such a huge bid/ask spread so as to discourage trading. And we want the market to actually function, computationally.

This won't be easy. First, let's talk about liquidity senstive automated market makers.
<h3>Liquidity Sensitive Automated Market Makers</h3>
Much of this is taken from <a href="http://www.cs.cmu.edu/~sandholm/liquidity-sensitive%20market%20maker.EC10.pdf">here</a>.

So remember that the more liquid the market is, the more the market-maker can potentially lose. What if you adjusted the liquidity as the market progressed so the more trades that take place, the more liquid the market is. Turns out, that's possible. To do that, we introduce a new concept: alpha.

So instead of having beta - the liquidity paramater above - be a constant, we define beta to be

beta = alpha * sum(q)

Or, alpha times the total number of trades placed. So as more trades get placed, the market becomes more liquid. It has some tradeoffs, though.

The first tradeoff is that prices will now no longer sum to 1. In the other equations I've showed you, it's obvious that the price of all the events summed together made 1, which make sense probabilistically. Now, the prices sum to slightly over 1 - so the market is “overcharging” for securities in general. The total amount prices may go above one is given by:

alpha * (2^N) * log(2^N)

The other tradeoff is that initially, the liquidity in the market is zero (because nothing has been bought or sold). So the market maker has to “seed” the market by purchasing its own securities so that there will be some liquidity when the market starts. It turns out that, in this liquidity-sensitive market, the dollar amount the market “spends” initially on its own securities (to seed the market with liquidity) is also the total amount the market may lose.

So the real tradeoff is between the initial liquidity and the maximum loss of the market.
<h3>The scaling problems of Automated prediction markets</h3>
So we have a number of constraints that we need to consider in our design:
<ul>
	<li>The length of q - the vector of bets placed on each “atomic” event rises exponentially in N (the number of binary events we want to cover). This makes computation intractable for large N.</li>
	<li>The maximum loss of the market-maker rises linearly with the amount of initial liquidity in the market.</li>
	<li>For any given alpha parameter, the “irrationality” (the amount above 1 they may rise) of the prices rises exponentially in N.</li>
</ul>
<h3>Covering designs and a strategy for many-event prediction markets</h3>
Ok - so it's clear that above N=10, we're starting to get in hot-water. Ten events is interesting, but it's just not <b>that</b> interesting. With N=10, the number of “atomic” events is 2**10 = 1024. But at 100 events, say, we're talking about about 1.2676506e30 atomic events. A one with ... thirty zeroes after it. Not. tractable.

What if we cheated? We really only need those 100 binary events in one market if people are going to be combining them into conditional bets. One way to skirt this exponential issue is to weaken the guarantee that any arbitrary combination of events is permitted.

Instead, you might instead guarantee that of 100 events, that you could always write a contract for any, say, three events in a conditional bet. Beyond three, it may be possible in some cases, but there's no guarantee. So instead of having one giant, intractable market - you run a number of tractable markets that together contain every combination of events that you're guaranteeing.

To do this, we need to step into the world of covering designs. A covering design is a combinatorial concept that basically asks this question: Given a set of v elements, how many k-sized subsets of v do we need to guarantee that every possible t-length subset of v is present in one of the k-sized subsets?

Put more succinctly by the<a href="http://A (v,k,t)-covering design is a collection of k-element subsets, called blocks, of {1,2,...,v}, such that any t-element subset is contained in at least one block."> La Jolla Covering Design Repository</a> (LJCR): “A <i>(v,k,t)-covering design</i> is a collection of k-element subsets, called blocks, of {1,2,...,v}, such that any t-element subset is contained in at least one block.”

Covering designs are tricky beasts, and there is a lot of interesting math on them. For example, we know that there is a (99, 10, 3) covering design with 1768 blocks (available at the LJCR), but we don't know of a (99, 10, 4) covering design with less than 36,980 blocks. That escalated quickly.

Increasing block size has the opposite effect.There is a (99, 12, 3) covering design with only 874 blocks.

So the general tradeoffs are:
<ul>
	<li>The higher the size of arbitrary combinations we allow, the more markets we have to run (this rises very quickly, although I'm not sure what the scaling is - exponential, factorial, etc.)</li>
	<li>The higher the number of binary events we run in each market, the fewer markets we have to run, but the more intractable they are.</li>
</ul>
There is a “sweet spot” of possibilities, but it's small. For example, it seems that (99, 10, 3) is a computationally tractable market: 1768 markets, each with 1024 atomic elements (or 10 binary elements), and guaranteeing any trader that any combination of three binary events can be priced and traded. And some combinations of up to 10 will also be tradeable.
<h3>how v10r works</h3>
v10r needs a few parameters to get started. The first thing we need is a (v, k, t) covering design. This will give us the number of markets we're running, the size of each market, and the size of the guarantee we can make about arbitrary combinations.

Next, we need to know what the initial liquidity we'd like the market to have, and how quickly it will rise as more trades are made in the market.

Finally, we need to know the fee we're charging for each trade.

The flow
<ol>
	<li>Order comes in as a logical statement with binary events as logical variables. Assume that the order size is O units.</li>
	<li>v10r finds the markets that are “in play” with the given binary events and filters out the rest - assume that there are M markets “in play”. The markets that are “in play” are those that contain all of the binary events present in the order.</li>
	<li>For each market, the logical statement is parsed into buy/sell orders for “atomic” events (the ordering of these events inside the q vector will be different in each market)</li>
	<li>For each market, v10r computes the price change for O/M units</li>
	<li>The price changes across the M markets is summed and this price is presented to the trader along with the fee</li>
	<li>If the price is accepted, the quantity changes are added to each of the M market's q vectors</li>
</ol>
There is a major known issue here:
<ul>
	<li>The mathematics of the market as presented depend on the trades being completed in a linear sequence. But what if two people are quoted on trades concerning overlapping markets and they both accept?</li>
</ul>
Upon further inspection, this isn't a terribly huge issue if:
<ul>
	<li>The market is pretty liquid (the other person's trade would not have moved the prices that much anyway)</li>
	<li>The prices are pretty efficient (meaning that there is no systematic bias towards increasing or decreasing the price)</li>
</ul>
The two of these imply that price discrepancies due to concurrent trading will be small and will as much be in your favor as against it. We can further mitigate this issue by ensuring that prices are never more than some amount out of date.
<h3>v10r simulations and code</h3>
I have simulations of the market available for public consumption at <a href="https://github.com/tvladeck/v10r-simulations">https://github.com/tvladeck/v10r-simulations</a> and a semi-working implementation at <a href="https://github.com/tvladeck/v10r">https://github.com/tvladeck/v10r</a>

The simulations code should be relatively easy to follow if you understand the equations above.

Caveats: the implementation is really bad code. It's almost certainly unreadable to anyone other than me. The “client” code relies on un-named gems, namely “GSL” and “Algorithms” and “Redis”. Other things probably don't work. It relies on having a running Redis server that works as the middle layer between the client and server.

The server code runs on clojure because I needed something that was easily multithreadable and had very good matrix libraries. Yay clojure!

The server is just constantly looking at the current order book and computing pre-baked values for the client to grab and use. To make the calculations tractable, I split the price function calculations between the client and server. The client parses the order, finds the right markets, and computes the “rest” of the price difference.

Wow! you made it all the way down here! Interested in chatting further? <a href="mailto:thomas.vladeck@gmail.com">Email me.</a>