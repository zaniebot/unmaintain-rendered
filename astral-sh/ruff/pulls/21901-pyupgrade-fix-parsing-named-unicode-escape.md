```yaml
number: 21901
title: "[`pyupgrade`] Fix parsing named Unicode escape sequences (`UP032`)"
type: pull_request
state: merged
author: phongddo
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: phongddo/fstring-escape-n
created_at: 2025-12-10T18:15:35Z
updated_at: 2025-12-16T21:33:39Z
url: https://github.com/astral-sh/ruff/pull/21901
synced_at: 2026-01-12T15:57:36Z
```

# [`pyupgrade`] Fix parsing named Unicode escape sequences (`UP032`)

---

_@phongddo_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/ruff/issues/19771

Fixes incorrect parsing of Unicode named escape sequences like `Hey \N{snowman}` in `FormatString`, which were being incorrectly split into separate literal and field parts instead of being treated as a single literal unit.

## Problem

The `FormatString` parser incorrectly handles Unicode named escape sequences:
- **Current**: `Hey \N{snowman}` is parsed into 2 parts `Literal("Hey \N")` & `Field("snowman")`
- **Expected**: `Hey \N{snowman}` should be parsed into 1 part  `Literal("Hey \N{snowman}")`

This affects f-string conversion rules when fixing `UP032` that rely on proper format string parsing.

## Solution

I modified `parse_literal` to detect and handle Unicode named escape sequences before parsing single characters:
- Introduced a flag to track when a backslash is "available" to escape something.
- When the flag is `true`, and the text starts with `N{`, try to parse the complete Unicode escape sequence as one unit, and set the flag to `false` after parsing successfully.
- Set the flag to `false` when the backslash is already consumed.

## Manual Verification

`"\N{angle}AOB = {angle}°".format(angle=180)` 

**Result**

```bash
 def foo():
-    "\N{angle}AOB = {angle}°".format(angle=180)
+    f"\N{angle}AOB = {180}°"

Would fix 1 error.
```

`"\N{snowman} {snowman}".format(snowman=1)`

**Result**
```bash
 def foo():
-    "\N{snowman} {snowman}".format(snowman=1)
+    f"\N{snowman} {1}"

Would fix 1 error.
```

`"\\N{snowman} {snowman}".format(snowman=1)`

**Result**
```bash
 def foo():
-    "\\N{snowman} {snowman}".format(snowman=1)
+    f"\\N{1} {1}"

Would fix 1 error.
```

## Test Plan

- Added test cases (happy case, invalid case, edge case) for `FormatString` when parsing Unicode escape sequence.
- Updated snapshots.


---

_Comment by @astral-sh-bot[bot] on 2025-12-10 18:24_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @phongddo on 2025-12-10 18:41_

---

_Review requested from @ntBre by @ntBre on 2025-12-10 20:20_

---

_Review comment by @ntBre on `crates/ruff_python_literal/src/format.rs`:1050 on 2025-12-10 23:43_

These might be nice as snapshot tests so that we don't have to update them manually in the future:


```suggestion
    fn test_format_unicode_escape() {
        assert_debug_snapshot!(FormatString::from_str("I am a \\N{snowman}"), @"<insta will update this>");
    }
```

in the off chance that the representation of `FormatString` changes.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP032_0.py.snap`:1 on 2025-12-10 23:56_

Could we add the test case from the issue to `UP032_0.py` too? I think the unit tests in this file make sense too, but it seems worth throwing in the rule test.

```py
"\N{angle}AOB = {angle}°".format(angle=180)
```

---

_@ntBre reviewed on 2025-12-11 00:00_

Thanks! This makes sense to me, I just had a couple of small suggestions about the tests.

I took a closer look at this code today, and I'm feeling much less wary than in https://github.com/astral-sh/ruff/pull/19774#pullrequestreview-3093439171. Our parser will have already flagged weird invalid cases like these that I brought up last time:

```py
"\N{{angle}}".format(angle="angle")
"\N{LATIN {SMALL} LETTER A}"
```

So we only need to handle valid cases, which this PR seems to do in a nice way. I also ran the fuzzer for a little while, just in case.

---

_Label `bug` added by @ntBre on 2025-12-11 00:00_

---

_Label `fixes` added by @ntBre on 2025-12-11 00:00_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 00:49_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@phongddo reviewed on 2025-12-11 00:50_

---

_Review comment by @phongddo on `crates/ruff_python_literal/src/format.rs`:1050 on 2025-12-11 00:50_

I don't have a strong opinion about using snapshot tests here, but this also makes sense to me 👍 I've updated the snapshots in https://github.com/astral-sh/ruff/pull/21901/commits/99a52eba908762420c0922ff89fcb2e17884a1c9, please let me know what you think (for example, if you'd prefer inline snapshots, sorry! I'm still learning the project and standards) 🙏 

---

_@phongddo reviewed on 2025-12-11 00:51_

---

_Review comment by @phongddo on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP032_0.py.snap`:1 on 2025-12-11 00:51_

Yeah, makes sense 👍 I added a test case in https://github.com/astral-sh/ruff/pull/21901/commits/99a52eba908762420c0922ff89fcb2e17884a1c9

---

_Review requested from @ntBre by @phongddo on 2025-12-11 00:52_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 00:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`
- WARN expected `heap_size` to be provided by Salsa query `class_based_items`

prefect (https://github.com/PrefectHQ/prefect)
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`


```

</details>




---

_Renamed from "Fix Unicode named escape squence parsing in `FormatString` when fixing `UP032`" to "[`pyupgrade`] Fix Unicode named escape squence parsing in `FormatString` when fixing `UP032`" by @phongddo on 2025-12-12 18:56_

---

_Comment by @phongddo on 2025-12-15 18:40_

Hey @ntBre 👋 just in case you missed the updates on this PR. I've addressed your comments about the tests. Please take a look when you have a moment and let me know if there's anything else I should adjust 🙏 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP032_0.py.snap`:916 on 2025-12-16 17:58_

Very small thing I missed before, but we should update this comment in the test file since it's not a false negative anymore 🎉 

---

_Review comment by @ntBre on `crates/ruff_python_literal/src/format.rs`:1050 on 2025-12-16 17:59_

Oh, I don't think it's worth adding a dependency to the crate anyway. Let's just revert to what you had initially, sorry for the trouble!

---

_@ntBre reviewed on 2025-12-16 18:03_

Thank you!

 This looks good to go, just one small test comment update, and I think I preferred your old tests (to avoid adding a dev-dependency to this crate). Sorry for the flip-flop and the delay.

---

_@phongddo reviewed on 2025-12-16 18:18_

---

_Review comment by @phongddo on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP032_0.py.snap`:916 on 2025-12-16 18:18_

ah, that's true, I missed it as well. Should we remove this comment entirely? I don't think it's relevant anymore.

---

_@phongddo reviewed on 2025-12-16 18:19_

---

_Review comment by @phongddo on `crates/ruff_python_literal/src/format.rs`:1050 on 2025-12-16 18:19_

no worries, I'm fine with both, I can revert it 👍 

---

_Review comment by @phongddo on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP032_0.py.snap`:916 on 2025-12-16 18:29_

I removed the comment in https://github.com/astral-sh/ruff/pull/21901/commits/bfbee7b0f163dd19470ecd11710a18842436b72e. This also caused many changes in the snapshot file due to the removal of one line. I hope that's fine 😅

---

_@phongddo reviewed on 2025-12-16 18:29_

---

_@ntBre approved on 2025-12-16 18:43_

Thank you!

---

_@ntBre reviewed on 2025-12-16 18:43_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP032_0.py.snap`:916 on 2025-12-16 18:43_

Yep, no worries!

---

_Renamed from "[`pyupgrade`] Fix Unicode named escape squence parsing in `FormatString` when fixing `UP032`" to "[`pyupgrade`] Fix parsing named Unicode escape sequences (`UP032`)" by @ntBre on 2025-12-16 18:45_

---

_Merged by @ntBre on 2025-12-16 21:33_

---

_Closed by @ntBre on 2025-12-16 21:33_

---
