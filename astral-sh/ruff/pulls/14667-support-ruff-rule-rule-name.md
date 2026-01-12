```yaml
number: 14667
title: "Support `ruff rule rule-name`"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - cli
  - needs-decision
assignees: []
base: main
head: cli-rule
created_at: 2024-11-29T05:18:38Z
updated_at: 2024-11-29T12:19:54Z
url: https://github.com/astral-sh/ruff/pull/14667
synced_at: 2026-01-12T15:55:48Z
```

# Support `ruff rule rule-name`

---

_@InSyncWithFoo_

## Summary

Resolves #14666.

## Test Plan

Added a round-trip unit test.


---

_Comment by @github-actions[bot] on 2024-11-29 05:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-11-29 07:11_

Thanks. I've to think about this. For future requests, please first open an issue and then give us some time to triage it before opening a PR because 

* We want to be mindful of your time and avoid that you spend time implementing a feature that won't make it into ruff
* We now have two places to discuss the feature requests, and two notification for everyone subscribed to the Ruff repo


---

_Comment by @InSyncWithFoo on 2024-11-29 07:25_

I will do so for future feature requests. As for this one, it is both so important and simple that I had already finished all the implementation by the time I knew it (client-side too, by the way).

In hindsight, creating #14666 was unnecessary (it is practically a duplicate of #14491, which I knew existed), for which I apologize.


---

_Label `cli` added by @MichaReiser on 2024-11-29 09:42_

---

_Label `needs-decision` added by @MichaReiser on 2024-11-29 09:42_

---

_Comment by @MichaReiser on 2024-11-29 12:11_

I'll close this considering that there are reasonable alternatives to get the rule -> code metadata, that supporting rule names for `rule` only results in an inconsistent CLI experience, and it means that rule names require versioning.

I do like to keep the issue open because we want to support rule names for the `rule` command long-term.

---

_Closed by @MichaReiser on 2024-11-29 12:11_

---

_Branch deleted on 2024-11-29 12:19_

---
