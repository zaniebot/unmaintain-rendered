```yaml
number: 14286
title: use a laughably large test timeout (10 min) to unbreak CI
type: pull_request
state: closed
author: oconnor663
labels: []
assignees: []
draft: true
base: main
head: jack/laughably_large_test_timeout
created_at: 2025-06-26T17:35:26Z
updated_at: 2025-06-26T17:53:30Z
url: https://github.com/astral-sh/uv/pull/14286
synced_at: 2026-01-10T11:10:43Z
```

# use a laughably large test timeout (10 min) to unbreak CI

---

_Pull request opened by @oconnor663 on 2025-06-26 17:35_

Two minutes wasn't enough here: https://github.com/astral-sh/uv/pull/14170

It sounds like the real fix will have something to do with our Depot setup, but in the meantime we might want to tolerate excessively slow tests to unbreak CI? Eager to get other people's thoughts here. (Will wait to see if CI even passes before tagging anyone.)

---

_Comment by @zanieb on 2025-06-26 17:42_

We'll lose metrics on how often these are failing, and could have other regressions. I think I'd rather rely on one or more of

- #14285 
- https://github.com/astral-sh/uv/pull/14284
- #14281

---

_Closed by @oconnor663 on 2025-06-26 17:53_

---
