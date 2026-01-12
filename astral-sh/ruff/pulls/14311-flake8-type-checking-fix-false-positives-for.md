```yaml
number: 14311
title: "[`flake8-type-checking`] Fix false positives for `typing.Annotated`"
type: pull_request
state: merged
author: Daverball
labels:
  - bug
assignees: []
merged: true
base: main
head: bugfix/annotated-annotation-context
created_at: 2024-11-13T09:00:28Z
updated_at: 2024-11-14T10:13:00Z
url: https://github.com/astral-sh/ruff/pull/14311
synced_at: 2026-01-12T15:55:47Z
```

# [`flake8-type-checking`] Fix false positives for `typing.Annotated`

---

_@Daverball_

This partially addresses #13713

## Summary

Ruff properly clears  the `TYPE_DEFINITION` flag when visiting special parts of an annotation like `typing.Literal` and `typing.Annotated`. However there are other typing related semantic flags which aren't cleared. This is fine, however `is_typing_reference` only check for those higher level flags and completely ignores `TYPE_DEFINITION`, which can lead to false positives for TCH001-003.

## Test Plan

`cargo nextest run`


---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-13 09:07_

---

_Comment by @MichaReiser on 2024-11-13 09:08_

@AlexWaygood would you mind taking a look at this PR? I think you have a better understanding of our type checking flags

---

_Comment by @github-actions[bot] on 2024-11-13 09:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/3a1bb36441149282a305967ff810784099ec9a6a/ibis/expr/operations/generic.py#L16'>ibis/expr/operations/generic.py:16:44:</a> RUF100 Unused `noqa` directive (unused: `TCH001`)
+ <a href='https://github.com/ibis-project/ibis/blob/3a1bb36441149282a305967ff810784099ec9a6a/ibis/expr/operations/generic.py#L18'>ibis/expr/operations/generic.py:18:54:</a> RUF100 Unused `noqa` directive (unused: `TCH001`)
+ <a href='https://github.com/ibis-project/ibis/blob/3a1bb36441149282a305967ff810784099ec9a6a/ibis/expr/types/groupby.py#L28'>ibis/expr/types/groupby.py:28:42:</a> RUF100 Unused `noqa` directive (unused: `TCH001`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/3a1bb36441149282a305967ff810784099ec9a6a/ibis/expr/operations/generic.py#L16'>ibis/expr/operations/generic.py:16:44:</a> RUF100 Unused `noqa` directive (unused: `TCH001`)
+ <a href='https://github.com/ibis-project/ibis/blob/3a1bb36441149282a305967ff810784099ec9a6a/ibis/expr/operations/generic.py#L18'>ibis/expr/operations/generic.py:18:54:</a> RUF100 Unused `noqa` directive (unused: `TCH001`)
+ <a href='https://github.com/ibis-project/ibis/blob/3a1bb36441149282a305967ff810784099ec9a6a/ibis/expr/types/groupby.py#L28'>ibis/expr/types/groupby.py:28:42:</a> RUF100 Unused `noqa` directive (unused: `TCH001`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @Daverball on 2024-11-13 09:35_

Not quite sure what the new LOG015 violations are about and how they are related to this change, but they look correct to me.

The RUF100 violations are encouraging, since these are all due to previous false positives.

---

_Comment by @MichaReiser on 2024-11-13 10:43_

@Daverball can you try rebasing/merging main. I suspect that will help with the `LOG015` ecosystem changes

---

_@AlexWaygood approved on 2024-11-13 12:17_

Thank you! This LGTM.

---

_Renamed from "[flake8-type-checking] Fixes a false positive for `typing.Annotated`" to "[flake8-type-checking] Fix false positives for `typing.Annotated`" by @AlexWaygood on 2024-11-13 12:17_

---

_Merged by @AlexWaygood on 2024-11-13 12:17_

---

_Closed by @AlexWaygood on 2024-11-13 12:17_

---

_Branch deleted on 2024-11-13 12:30_

---

_Comment by @dhruvmanila on 2024-11-14 04:47_

@AlexWaygood I'm assuming this is a bugfix based on the PR title but feel free to update the label if you think it's a rule change.

---

_Label `bug` added by @dhruvmanila on 2024-11-14 04:47_

---

_Renamed from "[flake8-type-checking] Fix false positives for `typing.Annotated`" to "[`flake8-type-checking`] Fix false positives for `typing.Annotated`" by @dhruvmanila on 2024-11-14 04:47_

---

_Comment by @AlexWaygood on 2024-11-14 10:12_

> @AlexWaygood I'm assuming this is a bugfix based on the PR title but feel free to update the label if you think it's a rule change.

Yes, I think that's right! Sorry, I should have added the label.

---
