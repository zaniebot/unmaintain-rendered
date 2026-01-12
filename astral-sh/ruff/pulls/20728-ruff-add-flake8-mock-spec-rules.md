```yaml
number: 20728
title: "[ruff] Add flake8-mock-spec rules"
type: pull_request
state: open
author: Tenzer
labels: []
assignees: []
draft: true
base: main
head: flake8-mock-spec
created_at: 2025-10-06T20:20:38Z
updated_at: 2025-10-07T07:58:27Z
url: https://github.com/astral-sh/ruff/pull/20728
synced_at: 2026-01-12T15:57:08Z
```

# [ruff] Add flake8-mock-spec rules

---

_@Tenzer_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This adds rules from [flake8-mock-spec](https://pypi.org/project/flake8-mock-spec/), as requested in #5734.

## Test Plan

<!-- How was it tested? -->
This isn't currently tested, as it isn't finished yet.

Feed back would be appreciated about the current direction of this before I go further.

---

_@Tenzer reviewed on 2025-10-06 20:23_

---

_Review comment by @Tenzer on `crates/ruff_linter/src/codes.rs`:1192 on 2025-10-06 20:23_

This rule doesn't exist in flake8-mock-spec - probably because `ThreadingMock` only was added in Python 3.13 - so I have taken the liberty of using the next rule number for it.

---

_Comment by @MichaReiser on 2025-10-07 07:46_

Thank you for working on this. I think we should first come to a decision on the issue before working on a PR.

---

_Comment by @Tenzer on 2025-10-07 07:49_

It's not clear to me what outstanding decision there is to be made from the issue. As far as I can see there hasn't been any activity towards this for the last two years, so I thought it might be easiest to try to work a bit towards it and gather feedback on that - hence this PR.

---

_Comment by @MichaReiser on 2025-10-07 07:53_

The outstanding decision is whether we want to add the new mockspec plugin (does it fit into the rules we think should be part of Ruff. Every rule also adds a fair amount of maintenance burden)

---

_Comment by @Tenzer on 2025-10-07 07:58_

Ah, okay. Is there anything I can do to help getting a decision made on that?

---
