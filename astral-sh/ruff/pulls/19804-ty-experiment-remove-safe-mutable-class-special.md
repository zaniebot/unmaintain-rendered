```yaml
number: 19804
title: "[ty] [experiment] remove \"safe mutable class\" special handling"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/safe-mutable
created_at: 2025-08-07T12:19:43Z
updated_at: 2025-08-12T17:51:25Z
url: https://github.com/astral-sh/ruff/pull/19804
synced_at: 2026-01-10T17:52:17Z
```

# [ty] [experiment] remove "safe mutable class" special handling

---

_Pull request opened by @AlexWaygood on 2025-08-07 12:19_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-08-07 12:21_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-07 12:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
twine (https://github.com/pypa/twine)
+ twine/commands/check.py:74:36: error[unresolved-attribute] Type `str` has no attribute `params`
- Found 12 diagnostics
+ Found 13 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/addons/test_modifyheaders.py:48:13: warning[possibly-unbound-attribute] Attribute `headers` on type `Response | None` is possibly unbound
- test/mitmproxy/addons/test_modifyheaders.py:50:20: warning[possibly-unbound-attribute] Attribute `headers` on type `Response | None` is possibly unbound
- test/mitmproxy/addons/test_modifyheaders.py:55:13: warning[possibly-unbound-attribute] Attribute `headers` on type `Response | None` is possibly unbound
- test/mitmproxy/addons/test_modifyheaders.py:57:20: warning[possibly-unbound-attribute] Attribute `headers` on type `Response | None` is possibly unbound
- test/mitmproxy/addons/test_modifyheaders.py:73:13: warning[possibly-unbound-attribute] Attribute `headers` on type `Response | None` is possibly unbound
- test/mitmproxy/addons/test_modifyheaders.py:75:33: warning[possibly-unbound-attribute] Attribute `headers` on type `Response | None` is possibly unbound
- test/mitmproxy/addons/test_modifyheaders.py:84:13: warning[possibly-unbound-attribute] Attribute `headers` on type `Response | None` is possibly unbound
- test/mitmproxy/addons/test_modifyheaders.py:86:33: warning[possibly-unbound-attribute] Attribute `headers` on type `Response | None` is possibly unbound
- Found 1821 diagnostics
+ Found 1813 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/test/unit/test_frame.py:2155:26: error[unresolved-attribute] Type `tuple[Literal[False], Literal[True]]` has no attribute `values`
+ static_frame/test/unit/test_frame.py:2177:26: error[unresolved-attribute] Type `list[Unknown]` has no attribute `to_pairs`
- Found 1771 diagnostics
+ Found 1773 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-08-07 12:27_

---

_Closed by @AlexWaygood on 2025-08-12 17:51_

---

_Branch deleted on 2025-08-12 17:51_

---
