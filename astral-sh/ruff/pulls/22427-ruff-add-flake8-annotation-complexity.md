```yaml
number: 22427
title: "[ruff]: add flake8-annotation-complexity"
type: pull_request
state: open
author: danjones1618
labels: []
assignees: []
base: main
head: feat-rule-flake8-annoation-complexity
created_at: 2026-01-06T22:57:30Z
updated_at: 2026-01-15T23:46:52Z
url: https://github.com/astral-sh/ruff/pull/22427
synced_at: 2026-01-16T00:03:07Z
```

# [ruff]: add flake8-annotation-complexity

---

_@danjones1618_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds support for [flake8-annotation-complexity](https://github.com/best-doctor/flake8-annotations-complexity) as requested by https://github.com/astral-sh/ruff/issues/2323.

As per the disucssion in #2323 the following has been implemented:

- Adds TAE002 to mirror `flake8-annotations-complexity`
- Ignores `typing.Annotated` for the purpose of annotation complexity
- Considers PEP-604 union syntax as complex as `Union` (+1)

Before merging, we should round-off any discussion over the formatting of the rules and associated codes. Currently it's adhering to the upstream however we may wish to split this into multiple rules like: `complex-argument-type-annotation`, `complex-return-type-annotation`, `complex-variable-annotation`, and so forth. In this case, we should move these to be under the `RUF` group instead of adding the `TAE` group.

## Test Plan

<!-- How was it tested? -->

- Added unit tests
- Added integration snapshot tests


---

_Renamed from "feat: implement initial complexity calculation" to "[ruff]: add flake8-annotation-complexity" by @danjones1618 on 2026-01-06 22:58_

---

_Marked ready for review by @danjones1618 on 2026-01-15 23:35_

---
