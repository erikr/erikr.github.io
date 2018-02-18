---
layout: post
title: Uber flat fare math 
tags:
---

Uber is offering [$2 flat fares](https://www.uber.com/info/plus/sanfrancisco) in San Francisco for September.

Is this a good deal? In other words, if you take uberPOOL, how many flat fare rides does it take to **break even** with regular uberPOOL?

In this simple model, we set the price of regular uberPOOL rides (left side of equation) equal to the price of flat fare uberPOOL rides (right side of equation).

The typical ride price for regular uberPOOL rides is *MRP*, the number of rides is *N*, and we assume we only pay for the $20 upfront fee which enables a $2/ride flat fare (up to 20 rides max).

![ubermath](/assets/ubermath.png)

We can plot *N* versus *MRP*:

![uberplot](/assets/uberplot.png)

Note we round the number of rides up to an integer value.

To determine if the new flat fare is a good deal, estimate your typical ride price for uberPOOL, then check how many rides you'd need to take to at least break even. If you take more rides than that, flat fare makes sense.

If you take fewer than *N* rides, you are not taking advantage of the flat fare's cheaper cost per ride. Given the $20 upfront fee, you should stick with regular uberPOOL.

For example, my typical uberPOOL rides cost about $6. Flat fares are therefore cost-effective if I take 5+ rides that month.

If you take longer (and thus more expensive) uberPOOL rides, e.g. Sunset to FiDi, the savings from flat fare are even greater.

This same model applies to other cities, or for UberX. Just adjust the numbers.

Remember, flat fares are only good for specific times, cities, and regions within those cities.

Innovative payment structures like flat fares will help more people afford transportation. Kudos to Uber!
