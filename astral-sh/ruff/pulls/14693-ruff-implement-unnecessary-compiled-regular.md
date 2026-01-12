```yaml
number: 14693
title: "[`ruff`] Implement `unnecessary-compiled-regular-expression` (RUF056)"
type: pull_request
state: closed
author: sbrugman
labels:
  - rule
  - preview
assignees: []
base: main
head: rule-unncessary-regular-expression-compile
created_at: 2024-11-30T18:59:33Z
updated_at: 2025-01-03T12:39:42Z
url: https://github.com/astral-sh/ruff/pull/14693
synced_at: 2026-01-12T15:55:48Z
```

# [`ruff`] Implement `unnecessary-compiled-regular-expression` (RUF056)

---

_@sbrugman_

## Summary

Closes https://github.com/astral-sh/ruff/issues/14691.

Name suggestion: `unnecessary-compiled-regular-expression`, but open to suggestions.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Added test fixtures. Ecosystem results are correct.


---

_Renamed from "Implement `unnecessary-regular-expression-compile`" to "[`ruff`] Implement `unnecessary-regular-expression-compile` (RUF056)" by @sbrugman on 2024-11-30 18:59_

---

_Renamed from "[`ruff`] Implement `unnecessary-regular-expression-compile` (RUF056)" to "[`ruff`] Implement `unnecessary-compiled-regular-expression` (RUF056)" by @sbrugman on 2024-11-30 19:00_

---

_Comment by @github-actions[bot] on 2024-11-30 19:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/a4fc97e92ed938260728e3f6c2b92df5ffb57b7f/pandas/io/formats/style_render.py#L2670'>pandas/io/formats/style_render.py:2670:12:</a> RUF056 Use of `re.compile().search()`
+ <a href='https://github.com/pandas-dev/pandas/blob/a4fc97e92ed938260728e3f6c2b92df5ffb57b7f/pandas/io/formats/style_render.py#L2671'>pandas/io/formats/style_render.py:2671:12:</a> RUF056 Use of `re.compile().search()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/settings/skbuild_overrides.py#L59'>src/scikit_build_core/settings/skbuild_overrides.py:59:17:</a> RUF056 Use of `re.compile().search()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/01777d1ef1f1a492e7a690ff3d88cf180e058303/astropy/coordinates/solar_system.py#L178'>astropy/coordinates/solar_system.py:178:8:</a> RUF056 Use of `re.compile().match()`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF056 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-12-02 08:10_

---

_Label `preview` added by @MichaReiser on 2024-12-02 08:10_

---

_@MichaReiser reviewed on 2024-12-02 08:11_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression_compile.rs`:17 on 2024-12-02 08:11_

```suggestion
/// If the object is not stored, then the expression is functionally equivalent
```

---

_@MichaReiser reviewed on 2024-12-02 08:12_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression_compile.rs`:53 on 2024-12-02 08:12_

```suggestion
    const FIX_AVAILABILITY: FixAvailability = FixAvailability::None;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression_compile.rs`:63 on 2024-12-02 08:14_

```suggestion
        Some(format!("Replace with `re.{re_kind}()` or store the compiled pattern"))
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression_compile.rs`:92 on 2024-12-02 08:16_

Would it be difficult to extend the rule to support cases where the compiled pattern is read only once in a function?

```
def test(s):
	pattern = re.compile("a")
	m = pattern.match(s)
```





---

_@MichaReiser reviewed on 2024-12-02 08:16_

---

_Comment by @MichaReiser on 2025-01-03 12:39_

This PR seems to have gone stale. @sbrugman feel free to re-open or re-submit if you plan to continue working on it. If anyone else is interested to move the PR forward, feel free to submit it as a new PR and reference this PR in its summary. 

Thanks @sbrugman for your work.

---

_Closed by @MichaReiser on 2025-01-03 12:39_

---
