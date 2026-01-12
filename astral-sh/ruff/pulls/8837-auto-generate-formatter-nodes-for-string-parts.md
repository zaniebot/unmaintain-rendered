```yaml
number: 8837
title: Auto-generate formatter nodes for string parts
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
assignees: []
merged: true
base: main
head: dhruv/formatted-nodes
created_at: 2023-11-25T12:52:16Z
updated_at: 2023-11-27T15:28:41Z
url: https://github.com/astral-sh/ruff/pull/8837
synced_at: 2026-01-10T23:40:55Z
```

# Auto-generate formatter nodes for string parts

---

_Pull request opened by @dhruvmanila on 2023-11-25 12:52_

A follow-up to auto-generate the `FormatNodeRule` implementation for the string part nodes. This is just a dummy implementation that is unreachable because it's handled by the parent nodes.


---

_Comment by @dhruvmanila on 2023-11-25 12:52_

Current dependencies on/for this PR:
* main
  * **PR #8837** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8837?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8837?utm_source=stack-comment).

---

_Label `formatter` added by @dhruvmanila on 2023-11-25 12:52_

---

_Comment by @dhruvmanila on 2023-11-25 12:56_

I forgot to run this script before. \cc @konstin should we create a CI step to make sure this is checked?

---

_Merged by @dhruvmanila on 2023-11-25 13:00_

---

_Closed by @dhruvmanila on 2023-11-25 13:00_

---

_Branch deleted on 2023-11-25 13:00_

---

_Comment by @github-actions[bot] on 2023-11-25 13:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2023-11-27 02:36_

Is the idea to move the formatting implementations to these nodes later? If not, then I don't see the value of having the format implementations (that can't format anything). Should we instead exclude the nodes in the generate script?

---

_Comment by @dhruvmanila on 2023-11-27 15:28_

> Is the idea to move the formatting implementations to these nodes later? If not, then I don't see the value of having the format implementations (that can't format anything). Should we instead exclude the nodes in the generate script?

That's a good point. I haven't spent much time on it but I think it would be beneficial to move the formatting implementations to those nodes. I can revert the change later if it's not useful.

---
