---
title: "Security update available for Metabase - Please upgrade now"
url: "https://www.metabase.com/blog/security-vulnerability"
date: "Thu, 19 Feb 2026 12:10:18 +0000"
author: ""
feed_url: "https://www.metabase.com/feed.xml"
---
<p>An independent security researcher Sho Odagiri from GMO Cybersecurity by Ierae submitted a severe issue with Metabase. We generally don’t blog about every bug, but this one is dangerous so we want to make sure that we reach out on all channels to our community to let them know that they should pay attention to this.</p>

<p>While we have no evidence that the vulnerability was ever exploited in the wild, and exploiting this vulnerability isn’t simple, <strong>if you are self-hosting Metabase, you should IMMEDIATELY update your Metabase instances (if you have not already).</strong></p>

<h2 id="the-vulnerability">The vulnerability</h2>

<p>The vulnerability allows an authenticated user (including embedding users) to retrieve sensitive information from a Metabase instance, including database access credentials. For more info, check out the <a href="https://github.com/metabase/metabase/security/advisories/GHSA-vcj8-rcm8-gfj9">security advisory</a>.</p>

<h2 id="are-you-affected">Are you affected?</h2>

<h3 id="metabase-cloud-customers-dont-need-to-upgrade">Metabase Cloud customers don’t need to upgrade</h3>

<p>No action needed. We’ve already upgraded your Metabase, and you’re no longer vulnerable.</p>

<h3 id="all-self-hosted-metabases-including-customers-should-upgrade-immediately">All self-hosted Metabases, including customers, should upgrade immediately</h3>

<p>IF you haven’t already, you should immediately upgrade to the latest point version of whichever Metabase version you’re running.</p>

<p>See the <a href="#minimum-safe-releases-for-each-metabase-version">list of minimum safe releases</a> below, and find the latest point version for the Metabase version you’re running. If you’re running a point version <em>below</em> that version, you’re still vulnerable and should upgrade immediately.</p>

<p>For example, if you are running 1.58.6, you should upgrade to 1.58.7 release or later. If you’re running a version of Metabase below version 55, you should upgrade to one of the versions listed below. You can find your current version by clicking on the “gear” icon in the upper right and selecting “About Metabase.”</p>

<h3 id="if-youre-running-a-custom-fork-of-metabase-reach-out-to-us-for-the-patches">If you’re running a custom fork of Metabase, reach out to us for the patches</h3>

<p>Email us at help@metabase.com so we can provide you the appropriate patches.</p>

<h2 id="minimum-safe-releases-for-each-metabase-version">Minimum safe releases for each Metabase version</h2>

<p>The downloads below include the minimum safe release for each Metabase version.</p>

<h3 id="55">55</h3>

<p>v0.55.20</p>

<ul>
  <li>Docker image: metabase/metabase:v0.55.20</li>
  <li>Download the JAR here: <a href="https://downloads.metabase.com/v0.55.20/metabase.jar">https://downloads.metabase.com/v0.55.20/metabase.jar</a></li>
</ul>

<p>v1.55.20</p>

<ul>
  <li>Docker image: metabase/metabase-enterprise:v1.55.20</li>
  <li>Download the JAR here: <a href="https://downloads.metabase.com/enterprise/v1.55.20/metabase.jar">https://downloads.metabase.com/enterprise/v1.55.20/metabase.jar</a></li>
</ul>

<h3 id="56">56</h3>

<p>v0.56.20</p>

<ul>
  <li>Docker image: metabase/metabase:v0.56.20</li>
  <li>Download the JAR here: <a href="https://downloads.metabase.com/v0.56.20/metabase.jar">https://downloads.metabase.com/v0.56.20/metabase.jar</a></li>
</ul>

<p>v1.56.20</p>

<ul>
  <li>Docker image: metabase/metabase-enterprise:v1.56.20</li>
  <li>Download the JAR here: <a href="https://downloads.metabase.com/enterprise/v1.56.20/metabase.jar">https://downloads.metabase.com/enterprise/v1.56.20/metabase.jar</a></li>
</ul>

<h3 id="57">57</h3>

<p>v0.57.13</p>

<ul>
  <li>Docker image: metabase/metabase:v0.57.13</li>
  <li>Download the JAR here: <a href="https://downloads.metabase.com/v0.57.13/metabase.jar">https://downloads.metabase.com/v0.57.13/metabase.jar</a></li>
</ul>

<p>v1.57.13</p>

<ul>
  <li>Docker image: metabase/metabase-enterprise:v1.57.13</li>
  <li>Download the JAR here: <a href="https://downloads.metabase.com/enterprise/v1.57.13/metabase.jar">https://downloads.metabase.com/enterprise/v1.57.13/metabase.jar</a></li>
</ul>

<h3 id="58">58</h3>

<p>v0.58.7</p>

<ul>
  <li>Docker image: metabase/metabase/v0.58.7</li>
  <li>Download the JAR here: <a href="https://downloads.metabase.com/v0.58.7/metabase.jar">https://downloads.metabase.com/v0.58.7/metabase.jar</a></li>
</ul>

<p>v1.58.7</p>

<ul>
  <li>Docker image: metabase/metabase-enterprise/v1.58.7</li>
  <li>Download the JAR here: <a href="https://downloads.metabase.com/enterprise/v1.58.7/metabase.jar">https://downloads.metabase.com/enterprise/v1.58.7/metabase.jar</a></li>
</ul>

<h2 id="credits">Credits</h2>

<p>We thank Sho Odagiri from GMO Cybersecurity by Ierae, Inc for discovering and disclosing this vulnerability.</p>
