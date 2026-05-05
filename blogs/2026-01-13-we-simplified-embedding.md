---
title: "We simplified embedding"
url: "https://www.metabase.com/blog/we-simplified-metabase-embedding"
date: "Tue, 13 Jan 2026 00:01:00 +0000"
author: ""
feed_url: "https://www.metabase.com/feed.xml"
---
<p>TL;DR: If you’re embedding Metabase and upgrading to 58, <strong>you don’t have to do anything.</strong> Your existing embeds will continue to work exactly as before. These changes are about simplifying our embedding options so it’s easier for people to pick the option they need.</p>

<h2 id="what-changed">What changed</h2>

<p>Starting in <a href="/releases/metabase-58">Metabase 58</a>, there are two ways to embed Metabase for new embeds.</p>

<ul>
  <li><strong><a href="/docs/latest/embedding/modular-embedding">Modular Embedding</a></strong> - Embed Metabase components, as Guest or as user via SSO.</li>
  <li><strong><a href="/docs/latest/embedding/full-app-embedding">Full-app Embedding</a></strong> - The full Metabase, SSO only.</li>
</ul>

<p>If you’re already embedding Metabase, your existing embeds still work. Static embedding will still work for existing embeds, but new embeds must use Modular Embedding.</p>

<h2 id="how-the-old-options-map-to-the-new">How the old options map to the new</h2>

<table>
  <thead>
    <tr>
      <th>Before 58</th>
      <th>Starting in 58</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Static embedding</td>
      <td>Modular Embedding - Guest</td>
    </tr>
    <tr>
      <td>Embedded analytics JS</td>
      <td>Modular Embedding - SSO</td>
    </tr>
    <tr>
      <td>Embedded analytics SDK</td>
      <td>Modular Embedding SDK - Guest or SSO (React only)</td>
    </tr>
    <tr>
      <td>Interactive embedding</td>
      <td>Full-app Embedding - SSO only</td>
    </tr>
  </tbody>
</table>

<h2 id="why-we-changed-our-embedding-options">Why we changed our embedding options</h2>

<p>They were confusing. Static embedding was also somewhat interactive, but we also had interactive embedding, which was a different thing. Each had a different setup flow. It worked, but figuring out the right path could have been easier, so that’s what we did. Plus, the new Modular Embedding provides an easier upgrade path from static embedding—you can start with Guest embeds and upgrade to SSO without major code changes.</p>

<h2 id="modular-embedding-overview">Modular Embedding overview</h2>

<p>We built an in-app wizard that walks you through setup and generates a code snippet. Copy, paste, done. The choice you have to make is whether you give people in your app a Metabase account. If you do, it unlocks a lot of stuff, and reduces your maintenance burden.</p>

<ul>
  <li><strong>Guest</strong> — People don’t need Metabase accounts. You sign embed URLs with JWT (just like static embedding), and they see the dashboard or question you’ve embedded. You can add and lock filters, but that’s about it. Guest embeds are available in the OSS Edition (with a “Powered by Metabase” badge) and on Pro/Enterprise.</li>
  <li><strong>SSO</strong> — People authenticate through your identity provider and get their own Metabase account. And since your Metabase knows who’s viewing what, it can unlock everything: drill-through, the query builder, AI chat, collection browser, and more, all with the correct permissions applied. Self-service embedded analytics.</li>
</ul>

<p>Modular Embedding has an <a href="/docs/latest/embedding/sdk/introduction">SDK for React</a>, so if your app uses React, you should go with the SDK.</p>

<p>The nice part: Guest and SSO embeds share the same foundation, so it’s a much smoother upgrade path from Guest to SSO.</p>

<h2 id="upgrading-existing-embeds-to-58">Upgrading existing embeds to 58</h2>

<p><strong>You don’t have to do anything special when upgrading.</strong> Your existing embeds will continue to work exactly as before.</p>

<ul>
  <li><strong>Static embeds</strong> — Keep working. No code changes required. The iframe URLs, JWT signing, and parameter handling all work the same way. By default, you’ll see the new Modular Embedding wizard, but there’s an escape hatch: you can still access the legacy static embedding UI, configure embeds with the new wizard, and use the traditional iframe + JWT approach. Migrating to Modular Embedding SSO unlocks deep theming options that weren’t available with static embedding (Pro/Enterprise).</li>
  <li><strong>Interactive embeds</strong> — Keep working. Now called “Full-app Embedding,” but nothing changes on your end.</li>
  <li><strong>SDK embeds</strong> — Keep working. No changes to the SDK API. It’s just called the Modular Embedding SDK.</li>
</ul>

<p>The only difference is what you’ll see in the Metabase UI when setting up new embeds.</p>

<h2 id="further-reading">Further reading</h2>

<ul>
  <li><a href="/docs/latest/embedding/introduction">Embedding docs</a></li>
  <li><a href="/releases">Releases</a></li>
  <li><a href="/releases/metabase-58">Metabase 58</a></li>
</ul>
