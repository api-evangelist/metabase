---
title: "Meet Repro-Bot, our GitHub issue triage agent"
url: "https://www.metabase.com/blog/reprobot-github-issue-triage-agent"
date: "Fri, 10 Apr 2026 00:00:00 +0000"
author: ""
feed_url: "https://www.metabase.com/feed.xml"
---
<p>Reproducing bug reports is one of the most time-consuming parts of maintaining an open source project. We built an AI agent called Repro-Bot to help us with this task. In this post, we’re sharing how we built it and show you how you can build your own. If you want to skip ahead and check out our code, <a href="https://github.com/metabase/repro-bot">take a look at the Repro-Bot repo</a>!</p>

<h2 id="repro-bot-automates-the-boring-parts">Repro-Bot automates the boring parts</h2>

<p>Think about how you, a human person, would reproduce a bug report:</p>

<ol>
  <li>Set up an environment identical (or at least similar) to what the reporter has.</li>
  <li>Follow the steps provided in the issue.</li>
  <li>If you can reproduce the bug:
    <ul>
      <li>Write a test for the bug</li>
      <li>Fix it</li>
    </ul>
  </li>
  <li>If you can’t reproduce, think through why:
    <ul>
      <li>Is there information missing?</li>
      <li>Are there any hidden dependencies?</li>
      <li>Did anything change recently?</li>
      <li>etc</li>
    </ul>
  </li>
</ol>

<p>This process is a mix of judgement calls (like ”fix it,” or what constitutes a relevant change), and chores like setting up the environment, following the steps, and asking the same questions over and over.</p>

<p>Repro-Bot automates the boring parts and gets us started on fixing the issue.</p>

<h3 id="results-repro-steps-findings-possible-root-cause">Results: repro steps, findings, possible root cause</h3>

<p>As Repro-Bot attempts to repro an issue, it generates a report with its findings, a pointer to where in the code the bug probably occurs, etc.. For example, here is the first part of its output for <a href="https://github.com/metabase/metabase/issues/68802">this issue about disappearing percentages on some pie charts</a>.</p>

<p><img alt="Repro-Bot reproducing an issue with a summary and reproduction steps" src="../images/posts/engineering-2026/reprobot-repro.png" /></p>

<p>This information helps us respond to the person who reported the issue faster and get more details from them while it’s still fresh in their mind. We’ve also cleared out a number of issues from our backlog that Repro-Bot confirmed we had already fixed.</p>

<p>Of course, Repro-Bot isn’t infallible. Sometimes it can’t repro an issue. Sometimes it thinks it has reproduced a bug when it hasn’t. But even in those cases, Repro-Bot’s reports are still valuable. They give us hints and chronicle dead-ends, both of which save devs time getting to the root cause.</p>

<h3 id="how-repro-bot-works-and-how-to-build-your-own">How Repro-Bot works, and how to build your own</h3>

<p>The details of Repro-Bot are quite specific to Metabase, but we’ll walk you through its inner workings so you can build a similar agent for your own codebase and development setup. You can also <a href="https://github.com/metabase/repro-bot">fork our repo</a> and adapt it to your workflow.</p>

<p>Repro-Bot needs to be able to perform these tasks:</p>

<ol>
  <li>Parse and understand issues</li>
  <li>Spin up a test environment</li>
  <li>Work through reproduction steps</li>
  <li>Write tests</li>
  <li>Write a report</li>
  <li>Clean up and self-review</li>
</ol>

<p>Here’s how we approached each one.</p>

<ol>
  <li><a href="https://github.com/metabase/repro-bot/blob/main/.claude/commands/repro.md#phase-2-issue-parsing">Parsing issues</a> tells the LLM what you, a developer, are looking for when you analyze the issue yourself. For example, for Metabase, we need information about the Metabase version, the application database, the data warehouse. We also triage the issue as backend-focused or frontend-focussed to guide which tools should the agent use for reproduction.</li>
  <li><a href="https://github.com/metabase/repro-bot/blob/main/.claude/skills/playwright-cli/SKILL.md">Spinning up a test environment</a> is very specific to your codebase and tools, of course. At Metabase, we use Playwright for browser automation, filling forms, and taking screenshots. Repro-Bot spins up an environment in the same way that a developer would, and uses REPL access to the instance.</li>
  <li><a href="https://github.com/metabase/repro-bot/blob/main/.claude/commands/repro.md#phase-4-reproduction-max-3-attempts">To work through the reproduction steps</a>, we <a href="https://github.com/metabase/repro-bot/blob/main/.claude/skills/metabase-reference/SKILL.md">wrote a skill</a> that gives code recipes for common actions we see in reported reproduction steps, like inspecting a table or creating a query. We also wrote down some of the “folklore” domain knowledge around some features (like for example that there are two different ways to make a pivot table with two different code paths). Repro-Bot uses the reproduction steps that it previously extracted from the issue, invokes the tools based on the “triage” into backend/frontend, and uses the recipes for those tools to run through the repro steps.
It then evaluates what was tested and if its results match the reported behavior. If the agent can’t determine whether the issue was reproduced, it tries again (but no more than three times total).</li>
  <li>If the agent reproduces the issue, it <a href="https://github.com/metabase/repro-bot/blob/main/.claude/skills/write-test/SKILL.md">writes a failing test</a> so that a developer can have something to test against when they’re fixing the issue. We give some directions on the kind of tests we write for our code and troubleshooting common issues, but since the agent already has full access to the codebase, it can learn a lot from just analyzing existing tests.</li>
  <li><a href="https://github.com/metabase/repro-bot/blob/main/.claude/commands/repro.md#phase-6-report--post">Write a report and post to Linear</a>. This provides the bot with a detailed outline for the report (as you saw in the previous section), as well as instructions for how to post to Linear, and how to prevent the internal report from getting synced back to GitHub.</li>
  <li>Finally, <a href="https://github.com/metabase/repro-bot/blob/main/.claude/commands/repro.md#phase-7-cleanup">cleanup and self-review</a>. After each run, Repro-Bot <a href="https://github.com/metabase/repro-bot/blob/main/.claude/commands/auto-improve.md">reviews the notes from this and previous runs</a> to make concrete suggestions for improvements, for example to address tool errors, close any knowledge gaps it found, document new tools it might need, etc.</li>
</ol>

<h2 id="integrating-repro-bot-into-the-workflow">Integrating Repro-Bot into the workflow</h2>

<p>We use GitHub to collect reported bugs, and Linear to manage development work. To run Repro-Bot, a human tags a bug on GitHub with <code class="language-plaintext highlighter-rouge">.Run Repro-Bot</code>, which triggers a GitHub action that runs the workflow described above.</p>

<p>Running the bot is not an automated task by design: a human in the loop is essential to prevent injection attacks. Most issues come from our public GitHub issues, so it would be trivial for somebody to poison context. To guard against this, we sandbox the agent and limit its permissions. We also require a human to review issues before running it to make sure there is nothing suspicious in the issue.</p>

<p>We intentionally did not ask Repro-Bot to fix the issue. We had initially wanted to make a more end-to-end bot that could do it all, but that wider scope opened up a number of wrong paths the bot could go down. Keeping the agent’s purview limited keeps its output manageable, and we can always introduce more automation downstream.</p>

<h2 id="what-we-learned">What we learned</h2>

<p>We think that Repro-Bot is an interesting approach to using LLMs and AI tooling for software development, because it’s not about code generation. Our Repro-Bot repo is very specific to our setup, but with the code as a starting point and the description above, you can build your own.</p>

<p>Repro-Bot has become part of our daily development work, and continues to save us time. We hope that it inspires others to build (and share!) similar tools for themselves.</p>
