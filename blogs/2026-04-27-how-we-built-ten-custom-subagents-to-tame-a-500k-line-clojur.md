---
title: "How we built ten custom subagents to tame a 500K-line Clojure codebase"
url: "https://www.metabase.com/blog/ten-custom-subagents"
date: "Mon, 27 Apr 2026 00:00:00 +0000"
author: ""
feed_url: "https://www.metabase.com/feed.xml"
---
<p>Metabase’s backend is big. We’re talking 500K lines of Clojure code spread across a query processor, permissions system, numerous database drivers, a notification pipeline, serialization layer, search engine, and more. And like all big codebases, each subsystem has its own idioms, gotchas, and “you just have to know” moments.</p>

<p>I’ve been using Claude Code for backend work on Metabase for a while now. It’s pretty good. But overloads Claude’s context window quickly. Every time Claude needs to understand a subsystem, it explores, greps, and reads files. All of that exploration eats your context window. Even when Claude spawns subagents, they will need to do a lot of extra work to get up to speed on the domain.</p>

<p>I <a href="https://gist.github.com/escherize/1cdd92a89cb52a1ce4be1a0cce0467b5">built some custom subagents</a> to fix this.</p>

<h2 id="what-are-subagents-and-why-did-i-make-ten-of-them">What are subagents and why did I make ten of them?</h2>

<p>Metabase’s backend has natural domain boundaries. The query processor is a 68-stage middleware pipeline that compiles MBQL (the Metabase Query Language) to SQL across 18 database dialects. The <a href="/docs/latest/permissions/start">permissions</a> system is a multi-granularity graph that handles <a href="/docs/latest/permissions/row-and-column-security">row-level security</a>, <a href="/docs/latest/permissions/database-routing">database routing</a>, and <a href="/docs/latest/permissions/impersonation">connection impersonation</a>. The notification system renders charts to images inside a JVM. These are different worlds.</p>

<p>A single generalist Claude session can navigate any of them, but it pays a context tax every time it switches domains. Subagents eliminate that tax by front-loading domain knowledge into the system prompt.</p>

<p><a href="https://code.claude.com/docs/en/sub-agents">Subagents</a> are a Claude Code feature that lets you define specialized AI assistants as markdown files. They each get their own context window, system prompt, memory, toolkit, and model selection.</p>

<p>I used Claude to write the “job descriptions” for each agent. I described the domain and what an expert would know, and Claude helped me flesh out the codebase locations, investigation patterns, caveats, and testing strategies. Each agent ended up being roughly 2,000-3,000 tokens worth (about 150 lines of markdown) of dense, useful context that can’t be easily inferred from the code.</p>

<h2 id="whats-inside-an-agent-file">What’s inside an agent file?</h2>

<p>Each agent is a markdown file that follows the same pattern of, domain knowledge → codebase locations → investigation approach → caveats → testing strategies. It’s a “here’s everything you need to be useful in this corner of the codebase” document.</p>

<p>Every file starts with YAML frontmatter:</p>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nn">---</span>
<span class="na">name</span><span class="pi">:</span> <span class="s">mbql-expert</span>
<span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Use</span><span class="nv"> </span><span class="s">this</span><span class="nv"> </span><span class="s">agent</span><span class="nv"> </span><span class="s">when</span><span class="nv"> </span><span class="s">working</span><span class="nv"> </span><span class="s">on</span><span class="nv"> </span><span class="s">Metabase's</span>
  <span class="s">query</span><span class="nv"> </span><span class="s">processor,</span><span class="nv"> </span><span class="s">MBQL</span><span class="nv"> </span><span class="s">query</span><span class="nv"> </span><span class="s">language,</span><span class="nv"> </span><span class="s">SQL</span><span class="nv"> </span><span class="s">compilation..."</span>
<span class="na">model</span><span class="pi">:</span> <span class="s">opus</span>
<span class="na">memory</span><span class="pi">:</span> <span class="s">user</span>
<span class="nn">---</span>
</code></pre></div></div>

<p>The <code class="language-plaintext highlighter-rouge">description</code> tells Claude <em>when</em> to delegate. The <code class="language-plaintext highlighter-rouge">model</code> picks which Claude model the subagent uses. And <code class="language-plaintext highlighter-rouge">memory: user</code> gives the agent a persistent directory at <code class="language-plaintext highlighter-rouge">~/.claude/agent-memory/mbql-expert/</code> where it records learnings across sessions.</p>

<p>The body of the file is the actual domain knowledge. Here’s a trimmed look at what the mbql-expert knows:</p>

<div class="language-markdown highlighter-rouge"><div class="highlight"><pre class="highlight"><code>You are a senior backend engineer with deep expertise
in Metabase's query processor (QP), MBQL query language,
and the entire query compilation pipeline.

<span class="gu">## Your Domain Knowledge</span>

<span class="gu">### The Query Processor Pipeline</span>
You understand the QP's ring-style middleware pipeline
with its four phases:
<span class="p">
-</span> <span class="gs">**Around middleware**</span> (3 layers)
<span class="p">-</span> <span class="gs">**Preprocessing**</span> (44 layers) — source card resolution,
  parameter substitution, join resolution, temporal bucketing...
<span class="p">-</span> <span class="gs">**Execution**</span> (8 layers) — caching, permissions, result metadata
<span class="p">-</span> <span class="gs">**Postprocessing**</span> (13 layers) — formatting, timezone conversion...

<span class="gu">### Key Codebase Locations</span>
<span class="p">-</span> <span class="sb">`src/metabase/query_processor/`</span> — QP core
<span class="p">-</span> <span class="sb">`src/metabase/driver/sql/`</span> — SQL driver base
<span class="p">-</span> <span class="sb">`modules/drivers/`</span> — database-specific drivers

<span class="gu">### Important Caveats</span>
<span class="p">-</span> Middleware ordering matters. Adding middleware in the wrong
  position causes subtle bugs.
<span class="p">-</span> A fix at the <span class="sb">`:sql`</span> level affects ALL SQL databases.
<span class="p">-</span> BigQuery is not standard SQL. Oracle has no BOOLEAN type.

<span class="gu">### REPL-Driven Development</span>
Use <span class="sb">`clj-nrepl-eval`</span> to evaluate middleware transformations
step by step...
</code></pre></div></div>

<h2 id="the-ten-subagents">The ten subagents</h2>

<p>I used Claude to help with defining these specific agents, framed as “job descriptions” like you’d post online (but specific to each section of our code). Our module system and namespace documentation help here, but I reviewed it to make sure it was reasonable.</p>

<table>
  <thead>
    <tr>
      <th>Agent</th>
      <th>Domain</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>mbql-expert</strong></td>
      <td>Query processor, MBQL language, SQL compilation, middleware pipeline, HoneySQL, streaming execution</td>
    </tr>
    <tr>
      <td><strong>permissions-expert</strong></td>
      <td>Access control, sandboxing, SSO (SAML/OIDC/LDAP), connection impersonation, embedding security</td>
    </tr>
    <tr>
      <td><strong>platform-expert</strong></td>
      <td>App database, HTTP server, API framework, settings system, migrations, Quartz scheduling</td>
    </tr>
    <tr>
      <td><strong>enterprise-expert</strong></td>
      <td>Serialization, SCIM provisioning, multi-tenancy, database routing, dependency tracking</td>
    </tr>
    <tr>
      <td><strong>content-expert</strong></td>
      <td>Collections, dashboards, cards, models, metrics, revisions, parameter mappings</td>
    </tr>
    <tr>
      <td><strong>notifications-expert</strong></td>
      <td>Dashboard subscriptions, alerts, email/Slack rendering, chart image generation</td>
    </tr>
    <tr>
      <td><strong>drivers-and-sync</strong></td>
      <td>Database drivers, metadata sync, fingerprinting, type mapping, connection management</td>
    </tr>
    <tr>
      <td><strong>search-expert</strong></td>
      <td>Search indexing, scoring/ranking, X-ray auto-analysis, semantic search</td>
    </tr>
    <tr>
      <td><strong>ai-expert</strong></td>
      <td>Metabot v3, LLM tool calling, context engineering, SQL generation</td>
    </tr>
    <tr>
      <td><strong>transforms-expert</strong></td>
      <td>Data actions, CSV uploads, transform pipeline, workspace management, model persistence</td>
    </tr>
  </tbody>
</table>

<p>I’ve made <a href="https://gist.github.com/escherize/1cdd92a89cb52a1ce4be1a0cce0467b5">all ten markdown files available for you to take a look at</a>.</p>

<h2 id="my-favorite-mbql-expert">My Favorite: mbql-expert</h2>

<p>The query processor is the heart of Metabase and the hardest thing to navigate. It’s a 68-stage middleware pipeline where a query enters as MBQL, gets rewritten 44 times during preprocessing, compiled to SQL via HoneySQL, executed, and then post-processed through 13 more stages. Oh, and some middleware runs <em>twice</em> because later stages can introduce structure that earlier stages need to process again. Query processing is a complex problem, especially when dealing with many different databases.</p>

<p>The mbql-expert already knows all of this. When I say “trace why this nested query with joins produces wrong results on Redshift,” it doesn’t start by grepping. It reasons about which middleware stages touch join aliases, checks Redshift-specific driver overrides, and examines the HoneySQL output. That’s the difference between a generalist exploring and a specialist investigating.</p>

<h2 id="how-i-actually-use-the-agents">How I actually use the agents</h2>

<p>The nice thing is you don’t need special syntax. Just mention the agent:</p>

<ul>
  <li>“Bounce this off the enterprise expert — will this serialization change break round-trip import/export?”</li>
  <li>“Ask the permissions expert how row-and-column security interacts with joined tables.”</li>
  <li>“Have the mbql expert review this HoneySQL compilation change.”</li>
</ul>

<p>Claude reads the intent and delegates work to the right agent. If you want to be explicit, you can also @-mention agents directly.</p>

<p>One useful pattern for explicit mentions: <strong>launching multiple agents in parallel</strong>. When reviewing a change that touches the query processor and permissions, I’ll ask Claude to have both experts weigh in simultaneously. Each expert investigates in its own context, and the results come back without cross-contaminating each other’s exploration.</p>

<hr />

<h2 id="tips-for-making-your-own-subagents">Tips for making your own subagents</h2>

<p>The pattern I’ve described works for any large codebase with distinct subsystems. See <a href="https://code.claude.com/docs/en/sub-agents#quickstart-create-your-first-subagent">Claude’s subagent documentation</a> for the details on how to structure the files.</p>

<p>Here are a few things that worked for me:</p>

<ul>
  <li>Have Claude help you write the agents: describe the domain and what an expert would know, and iterate on the system prompt together. (That’s how I built these.)</li>
  <li>Spend more time iterating on the description than the actual system prompts. The 2-3 sentence description in the frontmatter of each Markdown file is what Claude reads to decide when to delegate. A description that says “use for query processor work” is too vague, Claude won’t reliably match it. You want specific trigger words: “MBQL query language, SQL compilation, middleware pipeline, HoneySQL, streaming execution.” Think of it as writing a routing rule, not a job title.</li>
  <li>I include codebase locations in every agent, but the most durable content is the investigation patterns and caveats. Directories get renamed but the fact that “some middleware runs twice because later stages introduce structure that earlier stages need to re-process” isn’t going away anytime soon.</li>
</ul>

<p>Personal agents live in <code class="language-plaintext highlighter-rouge">~/.claude/agents/</code>, and project-local agents go in <code class="language-plaintext highlighter-rouge">.claude/agents/</code>.</p>

<p>Now go build some agents!</p>

<h2 id="more-from-metabase-engineering">More from Metabase Engineering</h2>

<ul>
  <li><a href="/blog/reprobot-github-issue-triage-agent">Meet Repro-Bot, our GitHub issue triage agent</a></li>
  <li><a href="/blog/lessons-learned-building-ai-analytics-agents">Lessons learned from building AI analytics agents: build for chaos</a></li>
</ul>
