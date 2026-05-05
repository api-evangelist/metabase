---
title: "Lessons learned from building AI analytics agents: build for chaos"
url: "https://www.metabase.com/blog/lessons-learned-building-ai-analytics-agents"
date: "Tue, 03 Feb 2026 00:00:00 +0000"
author: ""
feed_url: "https://www.metabase.com/feed.xml"
---
<p>Last year, I came back from a conference, pulled the latest code, and fired up our brand new version of <a href="/features/metabot-ai">Metabot</a> to show our CEO the progress we’d made. While I was away, the team had been shipping new features and improvements.</p>

<p>My excitement quickly turned into one of the most embarrassing moments of my professional career. Metabot had transformed into a confused intern: eager to help but unable to remember what tools it had or how to use them.</p>

<p><img alt="A broken Metabot on fire, and an embarrassed engineer" src="../images/posts/metabot-header.png" /></p>

<p>But here’s the thing: this disaster taught us a lot about building production AI agents. In this post, I’ll walk you through what actually broke, why it happened, and the patterns we discovered that actually work in production for us.</p>

<p>If you want the full story with all the details, you can watch <a href="https://www.youtube.com/watch?v=EnvozxnWjP4">the talk we gave at the AI Engineering conference 2025 in Paris</a>.</p>

<h2 id="what-we-were-building-and-why-its-hard">What we were building (and why it’s hard)</h2>

<p>When we started building <a href="/features/metabot-ai">Metabot</a>, the text-to-SQL space was already crowded: you describe what you want, give an LLM your database schema, and it generates SQL. Easy, right?</p>

<p>Yes and no.</p>

<p>The happy path works great - you’ve probably seen the demos with 5 well-documented tables and simple questions. But even if it works, there’s a problem: <strong>not everyone speaks SQL</strong>. A query can look fancy and return results, but how do you know if it’s answering the right question?</p>

<p>We wanted to go beyond SQL generation. Our goal was to leverage the <a href="/features/query-builder">Metabase query builder</a>, a visual interface where users can click together queries and actually see what filters and aggregations are applied. This gives non-SQL users a way to validate and iterate on results themselves more easily.</p>

<p>But here’s where it gets hard:</p>

<ul>
  <li>SQL is baked into LLM training data. Our query builder language? Not so much.</li>
  <li>Real customer data is messy: hundreds of tables, vague descriptions, legacy cruft.</li>
  <li>Humans are notoriously bad at providing context: “How many customers did we lose?” (Which time period? What’s a “customer”? Logo churn or revenue churn?)</li>
</ul>

<p>The real challenge wasn’t query generation. It was building an agent that could navigate this chaos by understanding what users are looking at, what they actually mean, and helping them find answers even when they don’t know how to ask the question.</p>

<p>That’s what Metabot set out to do. And that’s what spectacularly broke.</p>

<h2 id="what-broke-local-optimization">What broke: local optimization</h2>

<p>The demo failure traced back to parallel development without integration testing. One engineer perfected the context awareness to make sure Metabot knew exactly what dashboard you were looking at. Another engineer optimized the querying tool, fine-tuning descriptions, parameters, and prompts until it worked beautifully in isolation.</p>

<p>Together, they created chaos.</p>

<p>The LLM doesn’t experience your architecture. It sees one context window: every instruction, every tool description, every piece of dynamic state, flattened into a single prompt. Our individually-optimized components were sending contradictory signals. Tool descriptions assumed different conventions. Instructions overlapped and conflicted.
The model couldn’t figure out what we wanted because we were telling it multiple inconsistent things simultaneously.</p>

<p>The fix required thinking differently about what we were building. We weren’t building a querying tool with some context features. <strong>We were building a context engineering system</strong>. The LLM handles the generation; our job is to ensure it sees clean, unambiguous context at every decision point.</p>

<h2 id="what-worked-context-engineering-over-prompt-engineering">What worked: context engineering over prompt engineering</h2>

<p>We stopped front-loading prompts and started engineering context throughout the agent’s lifecycle. Three patterns—optimized data representations, just-in-time instructions, and actionable error guidance—transformed how the LLM understood and used its tools</p>

<h3 id="llm-optimized-data-representations">LLM-optimized data representations</h3>

<p>We stopped dumping raw API responses into the context. Instead, we built explicit serialization templates for every data object Metabot works with — tables, fields, dashboards, questions — optimized for LLM consumption:</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;table</span>
  <span class="na">id=</span><span class="s">""</span>
  <span class="na">name=</span><span class="s">""</span>
  <span class="na">database_id=</span><span class="s">""</span>
<span class="nt">&gt;</span>
  ### Description  ### Fields | Field Name | Field ID |
  Type | Description | |------------|----------|------|-------------| 
<span class="nt">&lt;/table&gt;</span>
</code></pre></div></div>

<p>This structured format gives the LLM consistent, hierarchical context it can parse reliably. Table metadata, field types, and relationships are always in the same place, reducing hallucination and tool misuse. The format is also reusable across all of Metabot’s tools, so when one person optimizes it, everyone benefits.</p>

<p>Use this pattern when your agent works with complex domain objects that appear across multiple tools or conversation turns.</p>

<h3 id="just-in-time-instructions">Just-in-time instructions</h3>

<p>Our original architecture front-loaded everything into the system prompt. The LLM ignored most of it.</p>

<p>So we tried something different: include instructions in tool results, right in the relevant moment:</p>

<div class="language-json highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="p">{</span><span class="w">
  </span><span class="nl">"data"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Chart created with ID 123"</span><span class="p">,</span><span class="w">
  </span><span class="nl">"instructions"</span><span class="p">:</span><span class="w"> </span><span class="s2">"""Chart created but not yet visible to the user.

  To show them:
  - Navigate: use navigate_to tool with chart_id 123
  - Reference: include [View chart](metabase://chart/123) in your response
  """</span><span class="w">
</span><span class="p">}</span><span class="w">
</span></code></pre></div></div>

<p>When a chart gets created, tell the LLM <em>right then</em> how to show it. The LLM pays attention to context that shows up exactly when it’s relevant, not buried in a system prompt from 20 messages ago.</p>

<h3 id="explicit-error-guidance">Explicit error guidance</h3>

<p>This pattern is more commonly known, but worth emphasizing: don’t just return error messages, return recovery paths instead.</p>

<div class="language-json highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="p">{</span><span class="w">
  </span><span class="nl">"error"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Table 'orders_v2' not found"</span><span class="p">,</span><span class="w">
  </span><span class="nl">"guidance"</span><span class="p">:</span><span class="w"> </span><span class="s2">"""This table may have been renamed or deprecated.

  Try:
  1. Search for tables matching 'orders'
  2. Check if 'orders' or 'order_items' fits your query
  3. Ask the user which orders table they want to use"""</span><span class="w">
</span><span class="p">}</span><span class="w">
</span></code></pre></div></div>

<p>The LLM handles ambiguity much better when you tell it how to handle ambiguity.</p>

<p><img alt="Diagram showing how tool use improved after the changes were made" src="../images/posts/metabot-tool-use.png" /></p>

<h2 id="the-benchmark-problem-in-ai-analytics-agents">The benchmark problem in AI analytics agents</h2>

<p>After the demo disaster, we built benchmarks. Scores climbed into the 90s, but perceived quality dropped.</p>

<p>The issue is subtle but important: engineers write benchmark prompts like engineers. “Count of orders grouped by created_at week.” Clean, precise, all context provided.</p>

<p>Real people say: “Why is revenue down?”</p>

<p>That question is missing a time period, revenue definition, comparison baseline, and probably some context about what triggered the question in the first place. And even if you nail the realistic test cases, there’s the uncontrollable data mess underneath - legacy tables, missing descriptions, inconsistent naming. The gap between benchmark coverage and production chaos is where AI products fail.</p>

<p>We now treat benchmarks as integration tests, not pure quality measures. If a change drops the score, something broke. But a passing score doesn’t mean the agent works, just that it handles clean inputs correctly. The real evaluation is production feedback, analyzed through a lens of what people actually asked versus what they needed.</p>

<h2 id="build-for-chaos-not-happy-paths">Build for chaos, not happy paths</h2>

<p>Our initial Metabot hackathon prototype had 5 perfectly documented tables and worked beautifully. Production has hundreds of tables with varying quality, used by people who phrase questions in wildly creative ways, and with edge cases we never imagined.</p>

<p><img alt="The imagined clean and simple data warehouse we like to imagine on the left, the broken chaos that is often the reality on the right" src="../images/posts/metabot-data-warehouse.png" /></p>

<p>That’s the core lesson: don’t build for the happy path. Every polished demo with clean data creates expectations you can’t meet. People wander off the happy path in seconds.
Better to understand the chaos, build for it, and deliver consistently than to show something impressive that falls apart on contact with reality.</p>

<p>We learned this the hard way. But it forced us to focus on what actually matters: robust context engineering, handling messy data gracefully, and building for the chaos that production inevitably brings.</p>

<h2 id="try-it-yourself">Try it yourself</h2>

<p><a href="/features/metabot-ai">Metabot is now out of beta</a> in Metabase. You can try it with your own data and see how these patterns work in practice. Pro tip: Do your homework and <a href="/docs/latest/data-modeling/metadata-editing">set up your semantic types</a> like foreign key relations, <a href="/docs/latest/data-modeling/metadata-editing#adding-a-metric">metrics</a> and <a href="/docs/latest/data-modeling/segments-and-metrics">segments</a>. This will help improve the experience.</p>

<p>Want the full technical deep dive? <a href="https://www.youtube.com/watch?v=EnvozxnWjP4&amp;t=1s&amp;pp=ygUiYWkgZW5naW5lZXIgbWV0YWJvdCB0aG9tYXMgc2NobWlkdA%3D%3D">Watch the complete talk from AI Engineer Paris 2025</a> where we go deeper into the implementation details.</p>

<p>These patterns apply beyond analytics agents. Any time you’re building with LLMs, think about the full context window, deliver instructions when they’re actionable, and always (always!) build for the chaos.</p>
