```yaml
number: 21849
title: "[ty] Make Python-version subdiagnostics less verbose"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/py-version-verbosity
created_at: 2025-12-08T15:46:10Z
updated_at: 2025-12-08T16:05:14Z
url: https://github.com/astral-sh/ruff/pull/21849
synced_at: 2026-01-12T15:57:35Z
```

# [ty] Make Python-version subdiagnostics less verbose

---

_@AlexWaygood_

## Summary

@MichaReiser mentioned in https://github.com/astral-sh/ty/issues/1582#issuecomment-3626991908 that these subdiagnostics currently repeat some information from the primary diagnostic, which feels unnecessary. I agree.

Before:

<details>
<summary>Before</summary>

<img width="1644" height="876" alt="image" src="https://github.com/user-attachments/assets/fb9dd3ed-ba0e-420c-9aab-5cbd1c04837d" />

</details>

<details>
<summary>After</summary>

<img width="1492" height="900" alt="image" src="https://github.com/user-attachments/assets/798ebe13-ece2-4d09-a4de-857f3650902d" />

</details>

## Test Plan

Snapshots updated


---

_Review requested from @carljm by @AlexWaygood on 2025-12-08 15:46_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-08 15:46_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-08 15:46_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-08 15:46_

---

_Label `ty` added by @AlexWaygood on 2025-12-08 15:46_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-08 15:46_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 15:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-08 15:50_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 38 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser approved on 2025-12-08 15:57_

Thank you

---

_Merged by @AlexWaygood on 2025-12-08 15:58_

---

_Closed by @AlexWaygood on 2025-12-08 15:58_

---

_Branch deleted on 2025-12-08 15:58_

---

_Comment by @codspeed-hq[bot] on 2025-12-08 16:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fpy-version-verbosity?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21849 will **degrade performances by 4.31%**

<sub>Comparing <code>alex/py-version-verbosity</code> (b14777c) with <code>main</code> (385dd27)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Fpy-version-verbosity?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fpy-version-verbosity?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 197.6 s | 206.5 s | -4.31% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Fpy-version-verbosity?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---
