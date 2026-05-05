---
title: "10 B2B SaaS product metrics that should be on your dashboard"
url: "https://www.metabase.com/blog/product-metrics"
date: "Mon, 10 Nov 2025 00:05:05 +0000"
author: ""
feed_url: "https://www.metabase.com/feed.xml"
---
<p>For SaaS teams, product metrics are the backbone of decision-making. This guide walks through the core metrics every product and data team should track: conversion, activation, retention, churn, and NDR-showing how to define each one in SQL, calculate it correctly, and visualize results in Metabase.</p>

<h2 id="website-to-signup-conversion-rate">Website to signup conversion rate</h2>

<p><strong>Why it matters:</strong> Shows how well your website turns visitors into potential customers.</p>

<p><strong>How to build:</strong> Divide number of people who signed up to number of unique visitors and group by date (e.g. week/month).</p>

<p><strong>Bonus points</strong>: if your signup flow has several steps, calculate conversion rate into each step and display as a set of line charts</p>



<h2 id="signups-segmentation">Signups segmentation</h2>

<p><strong>Why it matters:</strong> Shows which marketing channels bring you most customers and which are growing faster.</p>

<p><strong>How to build:</strong> Group signups by marketing_channel or another attribution parameters that you have.</p>



<h3 id="traffic-quality-aka-signups-quality-share-of-icp-signups-share-of-high-quality-signups-share-of-mqls">Traffic quality (aka signups quality, share of ICP signups, share of high quality signups, share of MQLs)</h3>

<p><strong>Why it matters:</strong> Not all signups are equal, and you should be monitoring what kind of users you are acquiring. Sudden spikes in traffic quality might influence a lot of metrics down the funnel.</p>

<p><strong>How to build:</strong> The quality of signups is usually defined by the marketing and product team together, historically looking at easy to define qualities of a signup.</p>

<p>Those could be, for B2B: share of signups with business domain, share of signups from a specific country or a list of countries. Calculate the % of ICP signups over the total number of signups and group by week or month.</p>



<h3 id="activation-rate">Activation rate</h3>

<p><strong>Why it matters:</strong> Activation rate is the most important metric every product manager should watch.</p>

<p>Activation rate is a share of <strong>new signups</strong> that reached the point where they understand your product’s value and are taking <strong>meaningful actions.</strong></p>

<p>There’re 3 key things about a well defined activation metric. It should:</p>

<ol>
  <li>Be easily measurable and represent value</li>
  <li>Correlate with conversion well</li>
  <li>Show result in a 2-3 day window after signup (great if you can do it in a 1-day window).</li>
</ol>

<h3 id="how-to-define-and-validate-activation-rate-metrics">How to define and validate activation rate metrics</h3>

<p><strong>List hypotheses</strong></p>

<p>Very likely you can name 1-2-5 actions that users should perform in your product in order to signal that they understood what it is about. For example in Metabase, likely users understood how to use the product if they were able to build a nice chart from their data. Make a list of activation metric hypotheses in the format:</p>

<p><strong>Performed X events in Y days after signup, Y&lt;3</strong></p>

<p>Examples:</p>

<p><strong>New user should get 7 friends in 10 days (Facebook)</strong></p>

<p><strong>Sent 1 document within 2 days after trial start (PandaDoc)</strong></p>

<p><strong>User executes 100 queries OR invites &gt; 1 user within 3 days (Metabase)</strong></p>

<p><strong>Find correlations</strong></p>

<p>For the hypotheses you listed, build the shares: share of users who performed event x / total signups. Plot these shares on a chart and add your historical conversion rate (share of users who paid / total signups).</p>

<p>You should get a picture like this:</p>



<p>Run the correlation analysis for the curves of potential activation metrics and conversion on a weekly trend. Pick the best metric, the one that correlates with conversion better. You can check if your metrics correlate, using basic CORR function in SQL.</p>

<p><strong>Not seeing correlations?</strong></p>

<p>If correlation analysis did not work, try running an Odds Ratio analysis or <a href="https://www.listendata.com/2015/03/weight-of-evidence-woe-and-information.html">WOE/IV</a> analysis.</p>

<p>Or add an “<strong>OR did Z events</strong>” condition to the hypotheses that you have and repeat the correlation exercise.</p>

<p>Hopefully these steps would help. In order to further dissect the activation rate, it is often suggested to divide into “setup moment” and “aha moment”. E.g. in case of Metabase, our “setup moment” is connecting a database, which is super important but is not an activation action per se, while the “aha moment” in our case would be the creation of a chart.</p>

<h2 id="conversion-rate-from-trial-to-paid--conversion-rate-from-free-to-paid-customer">Conversion rate from trial to paid / conversion rate from free to paid customer</h2>

<p><strong>Why it matters:</strong> To know how big is the share of traffic that actually pays you. It’s also a metric that is easy to benchmark (see Benchmarks below).</p>

<p><strong>How to build:</strong> Divide the number of converted customers by the number of total signups and group by the date of signup (week/month).</p>

<p>To make this metric more stable, add a conversion window that is typical for your customers. E.g. if you have a time-limited 14d trial without a credit card, people can convert any time after 14 days, sometimes it would be after 60 or 90 days, so pick something that is easy enough for you to make decisions, e.g. 15 days.</p>

<p><strong>Note:</strong> Conversion rate is a lagging metric and is not very suitable for being a goal for the product team. If you’d like to have goals on something, try activation rate or self-service revenue, not conversion.</p>



<h2 id="new-business-revenue--new-business-mrr">New business revenue / new business MRR</h2>

<p><strong>Why it matters:</strong> Your product’s most important metric is revenue and you need to know what’s going on with it.</p>

<p><strong>How to build:</strong> Sum the revenue/MRR of all new customers who converted in the given month</p>

<p><strong>Pro tip</strong>: Group by pricing plans to see which products are bringing you more new revenue</p>



<h2 id="feature-adoption-share-of-total-paid-customers--of-total-mrr">Feature adoption share (of total paid customers / of total MRR)</h2>

<p><strong>Why it matters:</strong> The bigger the product becomes, the more features it has. Oftentimes new features are being added without consideration of their actual performance. This product strategy is called “Fire and forget”. For that not to happen, monitor the adoption rate of your features.</p>

<p><strong>How to build:</strong> Divide the number of customers who used the feature in a given month by the number of paid customers in the given month. Do it for all features in a form of a set of line charts over time to see the trends (and also see which features are doing well, and which aren’t).</p>

<p><strong>Pro tip:</strong> segment the features by pricing plans - to see what is actually driving customers to your higher plans.</p>



<h2 id="retention-rate-general-and-by-specific-featureuse-case">Retention rate (general and by specific feature/use case)</h2>

<p><strong>Why it matters:</strong> Retention is king, and knowing where the trends go is important. For anything new you ship you have to be sure people stick with the new features - retention cohorts to the rescue. If you see that people are dropping the feature after a first week and don’t come back — it’s time to iterate and improve. Otherwise your product might turn into a Frankenstein.</p>

<p><strong>How to build</strong>: Divide your customers to monthly cohorts and calculate how big % of the cohort uses the feature on month 1, 2, and so on.</p>



<h2 id="net-dollar-retention-rate--expansion-mrr">Net Dollar Retention rate / Expansion MRR</h2>

<p><strong>Why it matters:</strong> The only thing that can save your business long term is your ability to upsell and expand your existing customers. There’s a limit to user acquisition efforts your marketing team can apply in order to bring new customers in a cheap way. The more you will be growing, the more expensive acquisition will become.</p>

<p>Thus invest early in upgrade and upsell paths in your product — and monitor the Expansion MRR that will eventually drive your Net Dollar Retention rate up as well.</p>

<p><strong>How to build:</strong></p>

<ul>
  <li><strong>Expansion MRR:</strong> Sum the revenue from Expansion MRR events for the given month.</li>
  <li><strong>NDR in $</strong>: Expansion MRR + Reactivation MRR - Contraction MRR - Churn MRR</li>
</ul>



<h2 id="churn-rate-logo-mrr">Churn rate (logo, MRR)</h2>

<p><strong>Why it matters:</strong> The opposite metric to retention rate, churn is telling you how much money you’re losing each month from customers who paid you but stopped paying you in this month. Churn in logo and in MRR % is a well benchmarked metric.</p>

<p><strong>How to build:</strong> For all customers who paid in the previous month, but didn’t pay this month, generate Churn MRR events with a negative value. Sum these events to get the Churn MRR.</p>

<p>Logo churn in % is the number of customers who paid you in the previous month but stopped paying you this month divided by the total number of paying customers in this month.</p>



<h2 id="essential-benchmarks-for-top-of-funnel-saas-metrics">Essential benchmarks for top-of-funnel SaaS metrics</h2>

<p>Some of these metrics, mostly conversion rates, are easy to benchmark, especially if you’re building a product that has a similar business model to the canonical digital products.</p>

<p>Here’re some examples of top of the funnel metrics benchmarks for B2b SaaS types of products:</p>

<table>
  <thead>
    <tr>
      <th><strong>Metric</strong></th>
      <th><strong>Benchmark</strong></th>
      <th><strong>Comment</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Website visit (unique) → signup (finished)</strong></td>
      <td>8%</td>
      <td>8% is for a mix of paid and organic traffic. Depending on your user acquisition strategy, it could be higher or lower for paid traffic. A very nice early indicator of how well your marketing campaigns perform — if they bring in traffic that converts to signups better than organic, you’re on the right track.<br /><br />This number might be lower if you’re offering a trial with credit card collection.</td>
    </tr>
    <tr>
      <td><strong>Signup start → signup finished</strong></td>
      <td>25%</td>
      <td>Depends on the number of required fields/steps and again, on whether you collect a credit card or not.</td>
    </tr>
    <tr>
      <td><strong>Signup finished → Activated</strong></td>
      <td>35%</td>
      <td>This is the canonical B2B SaaS activation rate, with activation defined as mentioned above.</td>
    </tr>
    <tr>
      <td><strong>Signup finished → paid (7 or 14d trial)</strong></td>
      <td>10% (trial without credit card, 30-day window)<br />40% (trial with credit card)<br />2.5–5% (freemium)</td>
      <td>Trials with credit cards collected convert better, but you should be mindful of retention and refund rates.<br /><br />If you have a sales team engaging with leads from the self-service funnel, this number can be higher.<br /><br />Freemium products convert at best around 5%, which is considered very high — your product must be very sticky and have a large user base to sustain revenue at this rate.</td>
    </tr>
    <tr>
      <td><strong>Churn MRR %</strong></td>
      <td>4%</td>
      <td>For B2B SaaS, under this amount churn is considered healthy. If you’re doing better — great!</td>
    </tr>
    <tr>
      <td><strong>Net Dollar Retention %</strong></td>
      <td>100%+</td>
      <td>If you’re upselling well, you might reach 100%+ NDR, meaning your business would grow even if user acquisition stopped. Investors love this metric because it signals strong expansion and retention.</td>
    </tr>
  </tbody>
</table>

<p><a href="https://openviewpartners.com/2023-product-benchmarks/">Source: OpenView Product Benchmarks</a></p>

<h2 id="how-to-turn-your-product-metrics-into-actionable-insights">How to turn your product metrics into actionable Insights</h2>

<p><strong>First, get a full picture of your metrics in a nice bird view - make a dashboard that will serve as a navigating compass for your decisions. <a href="https://store.metabase.com/checkout">Metabase can help!</a></strong></p>

<p>Look at the metrics and find the laggards: which metrics are far below the benchmarks?</p>

<p>Try to pick 1-2 of them and find the reasons of why those could be so low.</p>

<p>Low activation and user engagement on trial will naturally lead to low conversions. Is your product too complicated to use? Are you acquiring the right users?</p>

<p>Perform user interviews, listen to sales calls to find the gaps in the first time user experience and focus on fixing those. Oh, and bugs, don’t forget to fix these as buggy software is also not converting people into paid and loyal customers well.</p>

<p>Activation problem is not fixed by adding tooltips, walkthrough guides or extra documentation. The only thing that works well is thoughtful design of the navigation and first time user experience, that should help to overcome setup hurdles, if they are necessary.</p>

<p>Churn is the derivative metric and won’t improve if you don’t fix the activation rate for new customers and adoption for the existing.</p>

<h2 id="final-thoughts-making-product-metrics-accessible-and-actionable">Final thoughts: making product metrics accessible and actionable</h2>

<p>This post showed you how to choose the right product metrics and visualize them. Metabase is a powerful tool not only for measuring and displaying these metrics but also for giving the entire team access to them, so everyone can explore the data and discover valuable insights on their own, no <a href="/features/query-builder">data expertise or SQL needed</a>.</p>

<h2 id="more-product-metrics-resources">More product metrics resources</h2>

<ul>
  <li><a href="/events/how-to-build-a-product-dashboard-in-metabase">How to build a product dashboard in Metabase</a></li>
  <li><a href="https://www.youtube.com/watch?v=GqfWIKgpnHw">From scattered metrics to move-ready insights</a></li>
  <li><a href="https://medium.com/appchoose/bye-bye-amplitude-c581b52ff762">Bye bye Amplitude. Our journey to Metabase</a></li>
</ul>
