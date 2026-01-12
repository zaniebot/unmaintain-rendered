```yaml
number: 16230
title: Add technique for offline generation of autocompletion files to docs
type: pull_request
state: open
author: morags
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-10-10T14:46:35Z
updated_at: 2025-10-15T14:07:11Z
url: https://github.com/astral-sh/uv/pull/16230
synced_at: 2026-01-12T16:12:10Z
```

# Add technique for offline generation of autocompletion files to docs

---

_@morags_

## Summary

Generating uv and uvx's autocompletions can take over half a second on even a moderately fast system, while loading pre-prepared files can be two orders of magnitude faster. This commit provides users with a general-purpose technique for generating autocompletion files offline, which could contribute to significantly faster shell startup times.

## Test Plan

Documentation change - manual testing


---

_Comment by @zanieb on 2025-10-10 15:50_

Hm it only takes 5ms on my machine, you're seeing half a second? That seems way too slow.

Separately, this might be nice to have but doesn't seem appropriate for our installation guide — it's too detailed. We'll want to find somewhere else for it, e.g., in the Reference, but I'm not sure where would be best yet.

---

_Comment by @morags on 2025-10-15 14:07_

~300ms after some upgrades, yes. Then the cache kicks in and subsequent runs are much faster, but obviously only the first one matters.

I know it's general-purpose, but uv is the main culprit in my scenario, hence the uv docs. In the meanwhile I put it in its own [gist](https://gist.github.com/morags/31ef8d0b1b8da7f8b55f8380c904e6fa), in case it doesn't fit here.

---
