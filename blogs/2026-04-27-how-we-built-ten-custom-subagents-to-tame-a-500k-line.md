---
title: "How we built ten custom subagents to tame a 500K-line Clojure codebase"
url: "https://www.metabase.com/blog/ten-custom-subagents"
date: "Mon, 27 Apr 2026 00:00:00 +0000"
author: ""
feed_url: "https://www.metabase.com/feed"
---
Metabase’s backend is big. We’re talking 500K lines of Clojure code spread across a query processor, permissions system, numerous database drivers, a notification pipeline, serialization layer, search engine, and more. And like all big codebases, each subsystem has its own idioms, gotchas, and “you just have to know” moments.
