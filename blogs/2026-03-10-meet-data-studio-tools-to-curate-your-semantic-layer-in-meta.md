---
title: "Meet Data Studio: tools to curate your semantic layer in Metabase"
url: "https://www.metabase.com/blog/meet-data-studio-semantic-layer"
date: "Tue, 10 Mar 2026 00:00:00 +0000"
author: ""
feed_url: "https://www.metabase.com/feed.xml"
---
<p>Metabase has grown a lot over the past few years. We’ve added a bunch of tools to help people stay on top of their analytics as things scale.</p>

<p>Eventually, it became clear these tools needed their own home.</p>

<p>Today, we’re introducing <a href="/product/data-studio/">Metabase Data Studio</a>, a place where teams can shape their data and define shared metrics.</p>

<h2 id="analytics-starts-simple-then-it-gets-less-simple">Analytics starts simple. Then it gets… less simple</h2>

<p>One goal at Metabase has always been to help non-technical people answer questions with data. And at first, that’s easy. You connect Metabase to your database, build a few dashboards, and things feel straightforward. But over time, cracks in the data model inevitably start to show.</p>

<p>People aren’t sure which tables to use, dashboard loading times get annoying, and there are three different queries that say “ARR”. AIs don’t stand a chance sifting through this stuff.</p>

<h2 id="data-studio-has-all-the-tools-you-need-to-clean-up-the-mess">Data Studio has all the tools you need to clean up the mess</h2>

<div style="margin-bottom: 2em;">
    
        <div class="youtube-container" style="width: 100%; height: 0; padding-bottom: 56.25%;">

          
	  
        </div>
    </div>

<p>Data Studio lets teams transform raw tables into analytics-ready datasets. You can define reusable metrics (like MRR) and segments (like Active Customers) that everyone (including AI!) can trust when building dashboards and questions.</p>

<p>Data Studio lives in Metabase: no extra tools, no duplicate work, no workflow overhauls, just publish and share instantly. You can start small and grow into it naturally as analytics becomes more shared and harder to change.</p>

<h2 id="the-tools-in-the-toolbox">The tools in the toolbox</h2>

<p>The first version of Data Studio ships with the following tools:</p>

<ul>
  <li><strong><a href="/docs/latest/data-studio/library">Library</a></strong>: A curated space for your organization’s most trusted analytics content—tables, metrics, and SQL snippets that your data team recommends.</li>
  <li><strong><a href="/docs/latest/data-studio/data-structure">Data structure</a></strong>: Add table metadata to make tables easier to work with.</li>
  <li><strong><a href="/docs/latest/exploration-and-organization/data-model-reference#glossary">Glossary</a></strong>: Define terms relevant to your business, both for people and agents trying to understand your data.</li>
  <li><strong><a href="/docs/latest/data-studio/dependency-graph">Dependency graph</a></strong>: A visual map of how your content connects, so you can understand the impact of changes before you make them.</li>
  <li><strong><a href="/docs/latest/data-studio/dependency-diagnostics">Dependency diagnostics</a></strong>: See which items have broken dependencies, or that aren’t used.</li>
  <li><strong><a href="/docs/latest/data-studio/transforms/transforms-overview">Transforms</a></strong>: Wrangle your data in Metabase, write the query results back to your database, and reuse them in Metabase as sources for new queries.</li>
</ul>

<p>And we have more to come, so stay tuned.</p>

<h2 id="open-source-at-the-core">Open source at the core</h2>

<p>We want data structure and curation to be accessible to everyone, which is why foundational features of Data Studio are available in our open source edition, with Pro and Enterprise features to grow into as you need them.</p>

<h2 id="do-i-even-need-to-care-about-data-studio">Do I even need to care about Data Studio?</h2>

<p>People tend to need some kind of data transformations when they have multiple sources of data (like your application and payments data), or a bunch of normalized tables. If you’re under 50 tables in your schema, don’t stress, watercress. If you have multiple data sources or a lot of tables, chances are you’ve been paying a tax on clarity, correctness, and performance. Data Studio can help get you sorted.</p>

<div class="blog-callout p2 mb4 bordered rounded">
Data Studio is just one part of a bumper release. <a href="/releases/metabase-59">Check out what else is new in v59</a>
</div>

<h2 id="how-to-get-started-with-data-studio">How to get started with Data Studio</h2>

<p>Data Studio ships with both OSS and EE editions (<a href="/pricing">with some paid features</a>).</p>

<p>Admins can find Data Studio from the top right grid icon. Some paid plans can grant non-admins access to Data Studio by adding people to the Data Analysts group.</p>

<p><a href="https://store.metabase.com/checkout">Try Metabase Pro for free</a>.</p>
