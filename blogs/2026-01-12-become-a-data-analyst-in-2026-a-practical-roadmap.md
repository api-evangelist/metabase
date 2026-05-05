---
title: "Become a data analyst in 2026: a practical roadmap"
url: "https://www.metabase.com/blog/data-analyst-roadmap"
date: "Mon, 12 Jan 2026 00:00:00 +0000"
author: ""
feed_url: "https://www.metabase.com/feed.xml"
---
<p>Becoming a data analyst is both a great goal and a big undertaking. It’s tempting to try to list everything you might need to learn, but that quickly becomes overwhelming and still incomplete.</p>

<p>Instead, this guide gives you a solid starting point. It focuses on building a strong foundation that will support all the topics you’ll want, or need, to learn later. The topics are broken down below into chunks of related topics, and organized by tools you might know or want to learn: <strong>Spreadsheets</strong>, <strong>Metabase</strong>, and <strong>SQL</strong>.</p>

<p>Stay tuned for part 2, which will include pointers to more advanced topics for you to tackle once you’ve mastered the basics laid out here.</p>

<p><img alt="Data analytics roadmap" src="/images/posts/data-analyst-roadmap.png" /></p>

<h2 id="step-1-data-basics">Step 1: Data basics</h2>

<p>First, let’s look at what data and data analytics even are. This is the foundation for all further steps. Even if you’re familiar with some of the material here, it will pay off to remind yourself of these fundamental concepts.</p>

<h3 id="data-analytics-fundamentals">Data analytics fundamentals</h3>

<p>Analytics is all about understanding what your data means and what it can tell you about your business (or whatever your data is about). Analytics serves a number of purposes, and doesn’t just react to the data. There are different types of analytics you should be aware of, even if we are mostly covering descriptive analytics in this guide.</p>

<p><strong>Core concepts</strong></p>

<ul>
  <li>What data analysts do, explained in <a href="https://www.datacamp.com/blog/what-is-data-analysis-expert-guide">this expert guide</a></li>
  <li>The main <a href="https://www.geeksforgeeks.org/data-analysis/types-of-analytics/">types of analytics</a>: descriptive, diagnostic, predictive, and prescriptive</li>
  <li>The difference between <a href="https://www.ibm.com/think/topics/data-science-vs-data-analytics">data science and data analytics</a></li>
</ul>

<h3 id="data-types-and-structures">Data types and structures</h3>

<p>Data comes in many forms, but we’re really only looking at a fairly specific kind of data here: tabular data the way it is used in spreadsheets and databases. Much of the worlds of business and technology run on this kind of data, however. In this section, we’re looking at how this data is organized and what kinds of information you can find in databases and spreadsheets.</p>

<ul>
  <li>How data tables work, including <a href="/learn/grow-your-data-skills/spreadsheets-to-databases/sheets-vs-tables">rows and columns</a></li>
  <li>The difference between <a href="https://careerfoundry.com/en/blog/data-analytics/difference-between-quantitative-and-qualitative-data/">quantitative and qualitative data</a></li>
  <li>Understanding <a href="https://eagereyes.org/blog/2013/data-continuous-vs-categorical">discrete vs continuous variables</a></li>
  <li>What <a href="https://www.ibm.com/think/topics/structured-vs-unstructured-data">structured and unstructured data</a> mean</li>
  <li>Why <a href="https://www.coursera.org/articles/data-granularity">data granularity</a> matters</li>
  <li>Additional reading on <a href="https://365datascience.com/tutorials/statistics-tutorials/numerical-categorical-data/">numerical and categorical data</a></li>
</ul>

<h2 id="step-2-getting-started-with-your-tools">Step 2: Getting started with your tools</h2>

<p>Before diving into analysis, you need to get comfortable with your tools. The three most common are <strong>Excel</strong> (for spreadsheets), <strong>Metabase</strong> (a BI tool), and <strong>SQL</strong> (for querying databases).</p>

<p>Start with whichever tool is most relevant to your work. You don’t need to learn all three at once.</p>

<h3 id="excel-basics">Excel basics</h3>

<p>You can skip this if you’re comfortable in Excel, but take this opportunity to reacquaint yourself with the basics of spreadsheets if you’re unsure.</p>

<ul>
  <li>An overview of essential formulas like <code class="language-plaintext highlighter-rouge">SUM</code>, <code class="language-plaintext highlighter-rouge">AVERAGE</code>, <code class="language-plaintext highlighter-rouge">COUNT</code>, <code class="language-plaintext highlighter-rouge">IF</code>, and <code class="language-plaintext highlighter-rouge">VLOOKUP</code> in <a href="https://www.datacamp.com/tutorial/basic-excel-formulas-for-everyone">this Excel tutorial</a></li>
</ul>

<h3 id="metabase-basics">Metabase basics</h3>

<p>As a BI tool, Metabase allows you to work directly with databases without necessarily knowing SQL, and also create data visualizations.</p>

<ul>
  <li>An introduction to core Metabase concepts in the <a href="/learn/metabase-basics/overview/concepts">Metabase basics overview</a></li>
</ul>

<h3 id="sql-basics">SQL basics</h3>

<p>SQL is the native language of databases. Even if it might look difficult, it’s worth knowing for more complex data analysis (and because it’s the industry standard). It’s also much less scary than it initially looks, once you get to know it a little bit.</p>

<ul>
  <li>A beginner-friendly <a href="/learn/sql/getting-started/introduction">SQL introduction</a> from Metabase</li>
  <li>Hands-on practice with <a href="https://sqlbolt.com/lesson/introduction">SQLBolt’s interactive lessons</a></li>
  <li>A concise <a href="/learn/cheat-sheets/sql-cheat-sheet">SQL cheat sheet</a> for quick reference</li>
</ul>

<h2 id="step-3-exploring-and-preparing-data">Step 3: Exploring and preparing data</h2>

<p>Now let’s get to the actual work with data! The first step is being able to organize it. Filtering and sorting data are the first operations, and they are part of all the next steps below.</p>

<p>The good news is that once you understand the concept in one tool, it transfers easily to others.</p>

<h3 id="filtering-data">Filtering data</h3>

<ul>
  <li>
    <p><strong>Filtering in Excel</strong>
Learn how to narrow down rows in spreadsheets using built-in filters, as shown in <a href="https://edu.gcfglobal.org/en/excel/filtering-data/1/">this Excel filtering guide</a>.</p>
  </li>
  <li>
    <p><strong>Filtering in Metabase</strong>
Apply filters visually in the query builder without writing SQL, as explained in the <a href="/learn/metabase-basics/getting-started/filter">Metabase filtering guide</a>.</p>
  </li>
  <li>
    <p><strong>Filtering in SQL</strong>
Filter rows in database queries using <code class="language-plaintext highlighter-rouge">WHERE</code> clauses, text and date conditions, and logical operators like <code class="language-plaintext highlighter-rouge">AND</code>, <code class="language-plaintext highlighter-rouge">OR</code>, and <code class="language-plaintext highlighter-rouge">NOT</code>, using examples from <a href="/learn/sql/filtering/by-text">filtering by text</a> and <a href="/learn/sql/filtering/by-date">filtering by date</a>.</p>
  </li>
</ul>

<h3 id="data-quality">Data quality</h3>

<p>Data is rarely clean and perfect. It may come from multiple sources, use inconsistent formats, or contain missing values.</p>

<ul>
  <li>Learn how to prevent errors using <a href="https://support.microsoft.com/en-us/office/apply-data-validation-to-cells-29fecbcc-d1b9-42c1-9d76-eff3ce5f7249">data validation in Excel</a></li>
  <li>Understand different strategies for handling missing values in <a href="https://insightsoftware.com/blog/how-to-handle-missing-data-values-while-data-cleaning/">this guide to data cleaning</a></li>
  <li>Format and clean results directly in Metabase using <a href="/learn/metabase-basics/querying-and-dashboards/questions/formatting">question formatting options</a></li>
  <li>A checklist covering common data cleaning tasks in <a href="https://www.datacamp.com/blog/infographic-data-cleaning-checklist">this data cleaning checklist</a></li>
</ul>

<h2 id="step-4-summarizing-and-analyzing-data">Step 4: Summarizing and analyzing data</h2>

<p>Once your data has been checked and cleaned, and you’re able to perform basic sorting and filtering, the more advanced operations can begin. Data can be vast, so it is necessary to reduce it in different ways to make sense of it. This is done using various kinds of aggregations: groupings that compute values. They can be simple sums, or various statistical values like means or medians.</p>

<h3 id="basic-statistics">Basic statistics</h3>

<p>You’ve heard of means and medians, but what do they mean? And how are they computed?</p>

<ul>
  <li>An explanation of mean, median, and mode in <a href="https://www.geeksforgeeks.org/maths/mathematics-mean-variance-and-standard-deviation/">this statistics overview</a></li>
  <li>An introduction to statistics concepts commonly used in data analysis in <a href="https://makemeanalyst.com/basic-statistics-for-data-analysis/">Basic statistics for data analysis</a></li>
</ul>

<p>It’s also useful to understand data aggregation, how individual data points are grouped into summary values. See <a href="https://www.techtarget.com/searchdatamanagement/definition/data-aggregation">this data aggregation overview</a>.</p>

<h3 id="aggregation-in-excel-pivot-tables">Aggregation in Excel: Pivot tables</h3>

<p>Pivot tables are a powerful way of computing aggregations in spreadsheets. While they might seem daunting at first, the basic idea is the same as in SQL: subdivide the data and compute values over each segment. Much of data analysis is built on top of this approach.</p>

<ul>
  <li>Learn how to create them with <a href="https://support.microsoft.com/en-us/office/create-a-pivottable-to-analyze-worksheet-data-a9a84538-bfe9-40a9-a8e9-f99134456576">Microsoft’s pivot table guide</a></li>
  <li>Read why they matter in this <a href="https://www.reddit.com/r/excel/comments/1e7d03u/whats_the_point_of_a_pivot_table/">Reddit discussion on pivot tables</a></li>
  <li>See a visual explanation on <a href="https://en.wikipedia.org/wiki/Pivot_table">Wikipedia’s pivot table page</a></li>
</ul>

<h3 id="aggregation-in-metabase">Aggregation in Metabase</h3>

<p>Similarly to Excel, Metabase has the tools for computing sums and breaking down large datasets into sections.</p>

<ul>
  <li>Learn how to summarize data using the <a href="/learn/metabase-basics/getting-started/summarize">Metabase summarize feature</a></li>
</ul>

<h3 id="aggregation-in-sql-group-by">Aggregation in SQL: <code class="language-plaintext highlighter-rouge">GROUP BY</code></h3>

<p>The SQL way of computing aggregations and statistics is done using the GROUP BY keyword. These resources will help you understand how it works.</p>

<ul>
  <li>A clear explanation of <code class="language-plaintext highlighter-rouge">GROUP BY</code> in <a href="https://www.geeksforgeeks.org/sql/sql-group-by/">this SQL guide</a></li>
</ul>

<h3 id="metrics-and-kpis">Metrics and KPIs</h3>

<p>Once you can aggregate data, the next step is deciding <em>what</em> to measure. Metrics and key performance indicators (KPIs) help turn raw numbers into signals you can track over time and use to guide decisions.</p>

<ul>
  <li>An overview of common business metrics in <a href="https://stripe.com/en-hu/resources/more/essential-saas-metrics">Essential SaaS metrics</a></li>
  <li>Tips for designing better metrics in <a href="https://medium.com/data-science/how-to-design-better-metrics-9bad7bc8c875">How to design better metrics</a></li>
</ul>

<h2 id="step-5-analysis-across-data-tables">Step 5: Analysis across data tables</h2>

<p>In real databases, data is usually organized into several data tables. To answer questions, these have to be connected through joins. This section explains how joins work, and how this operation can be performed even in Excel, but also in a BI tool like Metabase and using SQL.</p>

<h3 id="xlookup-and-vlookup-in-excel">XLOOKUP and VLOOKUP in Excel</h3>

<p>To combine data from different tables, Excel has the XLOOKUP and VLOOKUP functions.</p>

<ul>
  <li>See how spreadsheet lookups relate to database joins in <a href="/learn/grow-your-data-skills/spreadsheets-to-databases/xlookup-vs-joins">From XLOOKUP to joins</a></li>
</ul>

<h3 id="joining-tables-in-metabase">Joining tables in Metabase</h3>

<p>In databases, this operation is called a join. Metabase can create joins in its query editor.</p>

<ul>
  <li>Learn how joins work in the <a href="/learn/metabase-basics/querying-and-dashboards/questions/joins-in-metabase">Metabase joins guide</a></li>
</ul>

<h3 id="database-joins-in-sql">Database joins in SQL</h3>

<p>SQL of course allows you to create joins using the JOIN keyword.</p>

<ul>
  <li>A practical introduction to joins in <a href="/learn/sql/working-with-sql/sql-joins">SQL joins explained</a></li>
</ul>

<h2 id="step-6-data-visualization-and-dashboards">Step 6: Data visualization and dashboards</h2>

<p>Finally, once your analysis is done, it is time to show your results to the world. The way to do this is using charts and visualizations. Here we cover the basics of creating visualizations from data, and how to turn them into a interesting and compelling story.</p>

<h3 id="visualization-fundamentals">Visualization fundamentals</h3>

<ul>
  <li>Learn how to visualize trends with <a href="/blog/how-to-visualize-time-series-data">time series charts</a></li>
  <li>Improve chart clarity with <a href="/blog/how-to-build-better-line-and-bar-charts">better line and bar charts</a></li>
  <li>Explore geographic data using <a href="/blog/maps-data-visualization">maps and geospatial visualizations</a></li>
  <li>Choose the right chart with <a href="/blog/the-right-visualization">this visualization guide</a></li>
</ul>

<h3 id="designing-clear-dashboards">Designing clear dashboards</h3>

<ul>
  <li>Understand what dashboards are in <a href="https://www.coursera.org/articles/what-is-dashboard">this Coursera overview</a></li>
  <li>Avoid common mistakes highlighted in <a href="/blog/top-5-dashboard-fails">Top dashboard fails</a></li>
</ul>

<h3 id="data-storytelling">Data storytelling</h3>

<ul>
  <li>Learn what data storytelling means in <a href="https://www.duarte.com/resources/communication-skills/what-is-data-storytelling/">this introduction</a></li>
  <li>Improve clarity by reducing clutter, as explained in <a href="https://www.storytellingwithdata.com/blog/what-clutter-can-we-eliminate">What clutter can we eliminate?</a></li>
  <li>Build better charts with the idea of a <a href="https://www.storytellingwithdata.com/blog/2021/10/13/your-graph-skeleton-shouldnt-be-spooky">clear graph skeleton</a></li>
</ul>

<h2 id="conclusion">Conclusion</h2>

<p>Data analysis is a fascinating field to dive into, but it is easy to get lost in the many different things you can do, and all the possible ways to do it. If you’re new to this field, our guide will give you a first foundation for you to then base your own explorations on.</p>

<p>We very consciously kept this guide quite simple and basic. It’s better to have a shorter list of items to really work through, than a massive list that you have no hope of ever completing.</p>

<p>In a few months, we will follow up with more advanced tutorials and topics, so stay tuned!</p>
