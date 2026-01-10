```yaml
number: 14270
title: "[`flake8-pyi`] Always autofix `duplicate-union-members` (`PYI016`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: pyi016-always-autofix
created_at: 2024-11-11T12:05:24Z
updated_at: 2024-11-13T16:51:19Z
url: https://github.com/astral-sh/ruff/pull/14270
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-pyi`] Always autofix `duplicate-union-members` (`PYI016`)

---

_Pull request opened by @sbrugman on 2024-11-11 12:05_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR extends the autofix for `PYI016` so that it's always available.

It also improves the fix for a few cases where the previous fix would give awkward (but valid) results:
 
 `int | (str | int)`
```diff
-`int | (str)`
+`int | str`
```

The fix now also marks as unsafe when comments are present.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-11 12:05_

---

_Comment by @github-actions[bot] on 2024-11-11 12:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -2 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1bd061df6e4409c5330d258d11fb7633e861a622/providers/src/airflow/providers/microsoft/azure/operators/container_instances.py#L253'>providers/src/airflow/providers/microsoft/azure/operators/container_instances.py:253:43:</a> PYI016 Duplicate union member `VolumeMount`
- <a href='https://github.com/apache/airflow/blob/1bd061df6e4409c5330d258d11fb7633e861a622/providers/src/airflow/providers/microsoft/azure/operators/container_instances.py#L253'>providers/src/airflow/providers/microsoft/azure/operators/container_instances.py:253:43:</a> PYI016 [*] Duplicate union member `VolumeMount`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI016 | 2 | 0 | 0 | 0 | 2 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +8 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/models/helpers.py#L900'>superset/models/helpers.py:900:39:</a> PYI016 Duplicate union member `str`
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/models/helpers.py#L900'>superset/models/helpers.py:900:39:</a> PYI016 [*] Duplicate union member `str`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L51'>src/bokeh/core/property/factors.py:51:56:</a> PYI016 Duplicate union member `tuple[str, str]`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L51'>src/bokeh/core/property/factors.py:51:56:</a> PYI016 [*] Duplicate union member `tuple[str, str]`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L52'>src/bokeh/core/property/factors.py:52:85:</a> PYI016 Duplicate union member `tp.Sequence[tuple[str, str]]`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L52'>src/bokeh/core/property/factors.py:52:85:</a> PYI016 [*] Duplicate union member `tp.Sequence[tuple[str, str]]`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:88:</a> PYI016 Duplicate union member `ContourColor`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:88:</a> PYI016 [*] Duplicate union member `ContourColor`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI016 | 8 | 0 | 0 | 8 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:35 on 2024-11-11 12:58_

Is the reason to mark the fix as unsafe because it could remove comments or is it because it could lead to invalid syntax?

---

_Label `rule` added by @AlexWaygood on 2024-11-11 13:58_

---

_Label `fixes` added by @AlexWaygood on 2024-11-11 13:58_

---

_@sbrugman reviewed on 2024-11-11 16:46_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:111 on 2024-11-11 16:46_

I haven't yet looked into in which cases the import request errors (and probably should handle it differently)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:38 on 2024-11-12 09:59_

It's not clear to me if we rather want to have a dedicate rule that detects and fixes unnecessarily nested unions but I'm fine keeping it in here for now

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:111 on 2024-11-12 10:00_

Yes, we can't `unwrap` here and it will mean that we can't mark this rule as always fixable.

---

_@MichaReiser reviewed on 2024-11-12 10:02_

This is great. I have about the same comments as on the other PR:

* Using text-edits over generator has the advantage that the fix preserves (more) comments and the formatting. Is there a specific reason why you chose to rewrite the fix to being generator based
* I need to go through some other rules but I think we decided not to mark fixes as unsafe just because they could drop comments.

---

_@sbrugman reviewed on 2024-11-12 10:23_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:35 on 2024-11-12 10:23_

The motivation is that comments might cause information loss (e.g. link to an issue, why a certain annotation was chosen) and the developer should be able to review this as possible. Marking comments as unsafe, users are able to automatically fix all other cases (comments inside type annotations are rare), and carefully review unsafe fixes. 

Syntactically all is fine (edit: unless comments have semantics attached, e.g. `type: ignore`). 

If there was a decision to mark comments as safe and instead document this in the rule, I can change it to that. If so, shall we then draw that as a conclusion on https://github.com/astral-sh/ruff/issues/9790 and document it?

I see the point that removing comments is not "unsafe" in terms of possibly generating invalid syntax, but it is a way for users to automatically disambiguate between fixes that need careful review and which could be always applied instead of having to go through each "Fix safety" section in the docs.

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:38 on 2024-11-12 10:30_

The reasons for removing the item are different: one is because of exact duplicates, the other because `complex` implies `float` and `int`. There are cases where libs prefer being explicit and disable the second rule (stdlib random is an example). 

A dedicated rule for flatten a nested union could also work, but then should always be enabled together with these rules to clean up the fix result.

---

_@sbrugman reviewed on 2024-11-12 10:30_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:35 on 2024-11-12 10:36_

In the other thread I see the suggestion to not offer a fix when comments are present, shall I continue in that direction?

---

_@sbrugman reviewed on 2024-11-12 10:36_

---

_@sbrugman reviewed on 2024-11-12 10:41_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:35 on 2024-11-12 10:41_

@MichaReiser I recall I saw this as tweaks one previous PR, so that's why I took it as policy:
https://github.com/astral-sh/ruff/pull/14008/commits/f099b0d371e25f4c87ae881309bf853931b18fc1


---

_@MichaReiser reviewed on 2024-11-12 10:43_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:35 on 2024-11-12 10:43_

>In the other thread I see the suggestion to not offer a fix when comments are present, shall I continue in that direction?

Yes. I spent some more time in that review to see what we do in other places and not offering a fix is the most common. I suggest that we align the behavior here and revisit the decision holistically if we see need for it.

---

_@MichaReiser reviewed on 2024-11-13 09:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:35 on 2024-11-13 09:06_

It's good that you waited with changing the applicability. We discussed it internally thanks to you and we agree that we should mark such rules as unsafe. See https://github.com/astral-sh/ruff/pull/14300


---

_@MichaReiser reviewed on 2024-11-13 09:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:123 on 2024-11-13 09:06_

Let's gate the fix behind preview for now to get some testing in

---

_@MichaReiser approved on 2024-11-13 09:07_

---

_Merged by @MichaReiser on 2024-11-13 16:42_

---

_Closed by @MichaReiser on 2024-11-13 16:42_

---

_Branch deleted on 2024-11-13 16:44_

---
