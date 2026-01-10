```yaml
number: 602
title: "Fix `fstring-type-annotation` example syntax"
type: pull_request
state: closed
author: niraj-khatiwada
labels:
  - documentation
assignees: []
base: main
head: patch-1
created_at: 2025-06-07T11:38:01Z
updated_at: 2025-06-07T12:38:40Z
url: https://github.com/astral-sh/ty/pull/602
synced_at: 2026-01-10T02:34:10Z
```

# Fix `fstring-type-annotation` example syntax

---

_Pull request opened by @niraj-khatiwada on 2025-06-07 11:38_

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
The example syntax shown in the documentation for `fstring-type-annotation` has an extra `:` after function declaration which I think is a mistake. There should only be one `:` after return type annotation.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
I installed `ty` using `uv` in a fresh install and tried to go through ty rules one by one. I encountered the syntax error on `fstring-type-annotation` example code.
<!-- How was it tested? -->


---

_Comment by @AlexWaygood on 2025-06-07 11:46_

Thank you! Unfortunately these docs are generated from the doc-comments in https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/diagnostic.rs. You'll need to edit that file in the Ruff repo, run `cargo dev generate-all` locally, and then make a PR to the Ruff repo. (If you don't have `cargo` installed locally, you should also just be able to manually make the changes to https://github.com/astral-sh/ruff/blob/main/crates/ty/docs/rules.md as well as to https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/diagnostic.rs.) When we do our next release here, the doc updates you make to the Ruff repo will be reflected in this repo as well.

Sorry about this -- I know it's sort-of confusing!

---

_Closed by @AlexWaygood on 2025-06-07 11:46_

---

_Label `documentation` added by @AlexWaygood on 2025-06-07 12:38_

---
