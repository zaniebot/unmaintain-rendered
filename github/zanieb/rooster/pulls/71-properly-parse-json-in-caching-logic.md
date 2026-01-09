---
number: 71
title: Properly parse JSON in caching logic
type: pull_request
state: closed
author: geofft
labels: []
assignees: []
base: main
head: cache-json
created_at: 2025-07-14T18:00:51Z
updated_at: 2025-07-14T19:27:21Z
url: https://github.com/zanieb/rooster/pull/71
synced_at: 2026-01-07T13:12:14-06:00
---

# Properly parse JSON in caching logic

---

_Pull request opened by @geofft on 2025-07-14 18:00_

Hishel works with httpcore.Response, not httpx.Response, which doesn't do e.g. gzip decoding. In order to correctly parse the JSON we have to construct an httpx.Response (which Hishel will do later but not at the point where we need it).

Without this, responses containing a JSON list of errors are incorrectly cached.

---

_@zanieb approved on 2025-07-14 18:09_

---

_Merged by @zanieb on 2025-07-14 19:27_

---

_Closed by @zanieb on 2025-07-14 19:27_

---
