```yaml
number: 22645
title: "[ty]: Add Sarif Output Format"
type: pull_request
state: open
author: 11happy
labels:
  - cli
  - ty
assignees: []
base: main
head: sarif_output
created_at: 2026-01-17T10:23:58Z
updated_at: 2026-01-17T19:31:47Z
url: https://github.com/astral-sh/ruff/pull/22645
synced_at: 2026-01-17T20:10:51Z
```

# [ty]: Add Sarif Output Format

---

_@11happy_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR fixes #https://github.com/astral-sh/ty/issues/2484

## Test Plan

<!-- How was it tested? -->
I have added new tests referencing from https://github.com/astral-sh/ruff/pull/20155 & updated the respective snapshots.


---

_Review requested from @carljm by @11happy on 2026-01-17 10:23_

---

_Review requested from @MichaReiser by @11happy on 2026-01-17 10:23_

---

_Review requested from @sharkdp by @11happy on 2026-01-17 10:23_

---

_Review requested from @dcreager by @11happy on 2026-01-17 10:23_

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 10:34_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `cli` added by @AlexWaygood on 2026-01-17 11:42_

---

_Label `ty` added by @AlexWaygood on 2026-01-17 11:42_

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/snapshots/cli__lint__output_format_sarif.snap`:121 on 2026-01-17 19:31_

We should make sure not to regress the sarif output format of Ruff. 

I see two options:

* Duplicate the implementations and have one implementation in Ruff and another. This way, both have access to the rule registry
* Add a new `RuleMetadata` trait or similar where the Sarif emitter is either generic over `RuleMetadata` or takes a `&dyn RuleMetadata` (it can store it in a `Box<dyn RuleMetadata>` I would probably do this, given that this code isn't performance sensitive. 



---

_@MichaReiser reviewed on 2026-01-17 19:31_

---
