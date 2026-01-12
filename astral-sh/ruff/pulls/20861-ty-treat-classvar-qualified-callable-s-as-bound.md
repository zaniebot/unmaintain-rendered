```yaml
number: 20861
title: "[ty] Treat `ClassVar`-qualified `Callable`s as bound-method descriptors"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: david/treat-classvar-callables-as-bound-method-descriptors
created_at: 2025-10-14T11:51:30Z
updated_at: 2025-10-14T11:57:19Z
url: https://github.com/astral-sh/ruff/pull/20861
synced_at: 2026-01-12T15:57:11Z
```

# [ty] Treat `ClassVar`-qualified `Callable`s as bound-method descriptors

---

_@sharkdp_

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

_Label `ty` added by @sharkdp on 2025-10-14 11:51_

---

_Renamed from "David/treat classvar callables as bound method descriptors" to "[ty] Treat `ClassVar`-qualified `Callable`s as bound-method descriptors" by @sharkdp on 2025-10-14 11:51_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-14 11:51_

---

_Comment by @github-actions[bot] on 2025-10-14 11:53_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-14 11:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/v1/dataclasses.py:315:17: error[missing-argument] No argument provided for required parameter 1
- pydantic/v1/dataclasses.py:332:17: error[missing-argument] No argument provided for required parameter 1
- Found 767 diagnostics
+ Found 765 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/testtools.py:1931:41: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__code__`
- hydpy/cythons/modelutils.py:2110:44: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__code__`
- Found 611 diagnostics
+ Found 609 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-14 11:57_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `missing-argument` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 2 | 0 |
| **Total** | **0** | **4** | **0** |

**[Full report with detailed diff](https://david-treat-classvar-callabl.ecosystem-663.pages.dev/diff)** ([timing results](https://david-treat-classvar-callabl.ecosystem-663.pages.dev/timing))


---

_Comment by @sharkdp on 2025-10-14 11:57_

The ecosystem impact is the same as on https://github.com/astral-sh/ruff/pull/20860 (and this PR includes the same changes). So the impact of also treating `ClassVar`s as bound-method descriptors is zero.

---

_Closed by @sharkdp on 2025-10-14 11:57_

---
