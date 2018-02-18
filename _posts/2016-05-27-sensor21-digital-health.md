---
title: Why Sensor21 might be a big deal for digital health
layout: post
tags: research
---

[21.co](http://21.co/) is creating new ways to monetize compute and data:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Stop being a test subject, start monetizing your genome!<br>Lease access to your DNA for BTC - and log all uses.<a href="https://t.co/FC1uMtyxu1">https://t.co/FC1uMtyxu1</a></p>&mdash; 21 (@21) <a href="https://twitter.com/21/status/672636021943635968">December 4, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
<br>

Recently, [they described steps towards a "sensor data marketplace"](https://medium.com/@21/sensor21-earn-bitcoin-by-collecting-environmental-data-218a4132ca70#.om1j9qwaq):

> In a previous post, we showed how to rent out your machine for bitcoin micropayments to facilitate distributed uptime monitoring. In this post we’ll demonstrate another fun kind of “grid computing” application: selling access to sensor data collected by a hardware device in return for bitcoin.

This excites me because my work relies on de-identified but otherwise unprocessed biomedical sensor data.

[sensor.21.co](https://21.co/learn/sensor21/) does three things to make data markets efficient:

1. Finacially incentives data suppliers[^1].
2. Enables data demanders to purchase just what they want.
3. Connects suppliers with demanders.

The implications for biomedical data science are exciting. Some example use cases:

+ A healthy person regularly monitors their heart rate for fitness purposes. These data previously had no financial value, but now are monetizable because studies often need data from healthy control subjects. An investigator could browse the 'data marketplace', find appropriate subjects, and purchase exactly what the sample size calculations suggest the study requires.
	
+ Data (heart rate, activity, the genome...) from someone with a rare disease is presumably worth more than data from a healthy person. On the other side of the table, investigators struggle to enroll patients with rare disease in trials. Imagine the value in a market that connects these parties.

+ Startups could obtain patient data to test their algorithms. This is often done through partnerships with academic medical centers. But it seems more efficient to "Airbnb" the data you need instead of buying a "mansion" that is an entire prospective study[^2].

A market for sensor data could address several challenges we see in biomedical data science:

1. **Privacy & regulation:** De-identifying data is legally required and performed by some expensive third party. This could be automated.
2. **Cultural disincentives:** Sharing data does not usually benefit the original investigator, but publishing on it does. Also, incumbents like Epic and Cerner only lose by being open. Skipping the middlemen between data supply and demand completely bypasses this.
3. **Low technical sophistication:** People still mail hard drives. Data formats are outdated, messy, and non-standardized. A data-driven tech startup is more likely to get this right than an outsourced IT department at a hospital.
4. **Local scale:** Trials are often regional, but newer studies are beginning to capitalize on how tech transcends geography[^3]. We should be able to instantly purchase sensor data on a balanced cohort of patients from anywhere on earth.
5. **Low quality:** Often due to human error during data entry, low-quality / uncalibrated hardware, or most commonly the noise of daily life. Passive monitoring and advances in data quality metrics will help.
6. **Expensive personnel:** Physicians, study coordinators, and administrators are expensive. Skipping these middlemen will help data reach analysts, collaborators, companies, etc. in less time and for lower cost. Outdated incumbents may call this "research parasitism"[^4], but others call it efficiency and progress.

21 is *reducing market friction* for data. Historically, such platforms connect buyers and sellers in new and better ways and create a lot of value. I expect we'll hear more about incumbents and new players[^5] using this to advance digital health and many other verticals.

---
[^1]: 21's clear documentation and simple syntax help too.
[^2]: Buying just the data you need is not just capital-efficient; cost can mark the difference between possible and impossible because large and high-quality biomedical data sets can be too expensive for startups. For example, IBM acquired Truven, a gold mine of radiology images, [for $2.6B](http://www.wsj.com/articles/ibm-to-buy-truven-health-analytics-for-2-6-billion-1455804004).
[^3]: See [Emory's Healthy Aging study](http://healthyaging.emory.edu/), [Stanford's MyHeart Counts](https://med.stanford.edu/myheartcounts.html), and [UCSF's Health eHeart study](https://www.health-eheartstudy.org/).
[^4]: [Longo, Dan L., and Jeffrey M. Drazen. "Data Sharing." New England Journal of Medicine 374.3 (2016): 276-277.](http://www.nejm.org/doi/full/10.1056/NEJMe1516564)
[^5]: Anyone want to apply to [YC Fellowship](https://fellowship.ycombinator.com/)? So many ideas... JK, I'm focusing on research.