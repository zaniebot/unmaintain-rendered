```yaml
number: 21702
title: "parser: add `is_stub` to `ParseOptions` to fix false positive syntax error for stub files"
type: pull_request
state: closed
author: 11happy
labels:
  - parser
assignees: []
base: main
head: fix_stub_false_positive
created_at: 2025-11-30T12:28:54Z
updated_at: 2025-12-01T04:51:55Z
url: https://github.com/astral-sh/ruff/pull/21702
synced_at: 2026-01-12T15:57:31Z
```

# parser: add `is_stub` to `ParseOptions` to fix false positive syntax error for stub files

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

<!-- What's the purpose of the change? What does it do, and why? -->
This PR fixes #21677 

## Test Plan

<!-- How was it tested? -->
I have locally tested the working for main.pyi:
```
def foo[T](): ...

type Foo = int
```
<img width="733" height="56" alt="image" src="https://github.com/user-attachments/assets/f96e1772-e823-4841-b64e-c30f92e5e847" />



---

_Review requested from @MichaReiser by @11happy on 2025-11-30 12:28_

---

_Review requested from @dhruvmanila by @11happy on 2025-11-30 12:28_

---

_Comment by @astral-sh-bot[bot] on 2025-11-30 12:47_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @AlexWaygood on 2025-11-30 12:51_

I would personally want these syntax errors reported on my stub files if I specified a lower Python version (https://github.com/astral-sh/ruff/issues/21677#issuecomment-3592523385). Using new syntax makes stub files less portable, which isn't what I'd want if I was publishing library stubs. 

---

_Label `parser` added by @AlexWaygood on 2025-11-30 15:13_

---

_Comment by @11happy on 2025-12-01 04:51_

Closing this PR in support that these warnings can be opted out , which is much better than to remove them totally.
Thank you : ) 

---

_Closed by @11happy on 2025-12-01 04:51_

---
