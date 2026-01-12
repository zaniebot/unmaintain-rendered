```yaml
number: 9020
title: Use double quotes for all docstrings, including single-quoted docstrings
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
assignees: []
merged: true
base: main
head: enforce-double-quotes-for-all-docstrings
created_at: 2023-12-06T06:36:22Z
updated_at: 2023-12-07T04:41:01Z
url: https://github.com/astral-sh/ruff/pull/9020
synced_at: 2026-01-10T23:40:55Z
```

# Use double quotes for all docstrings, including single-quoted docstrings

---

_Pull request opened by @MichaReiser on 2023-12-06 06:36_

## Summary

This PR changes our docstring formatting to enforce double quotes for single-quoted docstrings (and not just triple-quoted strings).

I see reasons for and against this.

* We enforce an empty line after all docstring, regardless if they are single or triple quoted. Meaning, we recognize single-quoted string literals as docstrings (according to PEP 257). This speaks in favor of enforcing double quotes
* The [quote-style documentation](https://docs.astral.sh/ruff/settings/#format-quote-style) mentions that it enforces double quotes for all docstring, but this isn't the case without this PR.
* PEP 257 recommends using triple double quotes for all docstrings, but this PR only enforces using double quotes, but not using triple quotes. Meaning, the result is only half-correct.

Alternatives:

* Leave as is
* Be even more opinionated and change quotes to triple quotes

I favor the consistency of what we consider docstrings and how we format them. 
 
## Test Plan

Added test


---

_Comment by @MichaReiser on 2023-12-06 06:36_

Current dependencies on/for this PR:
* main
  * **PR #9021** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9021?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #9020** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9020?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9020?utm_source=stack-comment).

---

_Label `formatter` added by @MichaReiser on 2023-12-06 06:42_

---

_Comment by @github-actions[bot] on 2023-12-06 06:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-06 07:07_

---

_Renamed from "Enforce double quotes for all docstrings" to "Reformat single-quoted docstrings with double quotes" by @MichaReiser on 2023-12-06 07:09_

---

_Renamed from "Reformat single-quoted docstrings with double quotes" to "Use double quotes for all docstrings, including single-quoted docstrings" by @MichaReiser on 2023-12-06 07:10_

---

_@BurntSushi approved on 2023-12-06 12:35_

---

_@konstin approved on 2023-12-06 14:31_

> Be even more opinionated and change quotes to triple quotes

I'd favor this, because single quotes obfuscate that the statement is a docstring

---

_@charliermarsh approved on 2023-12-07 03:43_

---

_Merged by @MichaReiser on 2023-12-07 04:41_

---

_Closed by @MichaReiser on 2023-12-07 04:41_

---

_Branch deleted on 2023-12-07 04:41_

---
