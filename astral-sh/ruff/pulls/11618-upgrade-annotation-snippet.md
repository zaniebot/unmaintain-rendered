```yaml
number: 11618
title: Upgrade annotation snippet
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: upgrade-annotation-snippet
created_at: 2024-05-30T10:38:34Z
updated_at: 2024-11-26T09:08:45Z
url: https://github.com/astral-sh/ruff/pull/11618
synced_at: 2026-01-10T20:50:57Z
```

# Upgrade annotation snippet

---

_Pull request opened by @MichaReiser on 2024-05-30 10:38_

## Summary

I tried in this PR to upgrade annotation snippet but I hit a blocker because annotation snippet has now become more opinionated. It now prefixes every message with a level and requires a title. 
That means, it is no longer possible to have our custom message headers of the form `path:row:column [CODE] message` and instead it must be `error[code]: <title>` and it is unclear what message format we want. 

TODO: 

* Updated `grouped` emitter
* Decide on new message format

## Test Plan
See updated snapshots

Fixes #10832


---

_@MichaReiser reviewed on 2024-05-30 10:39_

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:48 on 2024-05-30 10:39_

It's not clear to me where and how we want to show the file name, location, and fixability

---

_Review requested from @charliermarsh by @MichaReiser on 2024-05-30 10:39_

---

_Closed by @MichaReiser on 2024-11-26 09:08_

---
