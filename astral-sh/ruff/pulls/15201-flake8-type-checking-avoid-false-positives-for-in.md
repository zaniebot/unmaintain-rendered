```yaml
number: 15201
title: "[`flake8-type-checking`] Avoid false positives for `|` in `TC008`"
type: pull_request
state: merged
author: Daverball
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: bugfix/tc008-union-syntax-pre-py310
created_at: 2024-12-30T10:54:43Z
updated_at: 2025-01-15T13:31:16Z
url: https://github.com/astral-sh/ruff/pull/15201
synced_at: 2026-01-10T20:34:00Z
```

# [`flake8-type-checking`] Avoid false positives for `|` in `TC008`

---

_Pull request opened by @Daverball on 2024-12-30 10:54_

This is a follow-up to #15180

## Summary

`|` is not a valid runtime operation between types pre Python 3.10, so we should avoid unquoting the type expression.

Additionally most type checkers don't care about the execution context, so while they will allow `|` between types in stub files for convenience, they will still disallow it in type checking blocks, since those are treated as regular runtime code.

## Test Plan

`cargo nextest run`


---

_Comment by @Daverball on 2024-12-30 10:56_

I've based this on the same branch as #15180 in order to reduce churn between the two changes, so there are some unrelated changes here until that has been merged.

---

_Comment by @github-actions[bot] on 2024-12-30 11:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/decorators/condition.py#L32'>airflow/decorators/condition.py:32:35:</a> TC008 [*] Remove quotes from type alias
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/decorators/condition.py#L33'>airflow/decorators/condition.py:33:35:</a> TC008 [*] Remove quotes from type alias
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC008 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Comment by @Daverball on 2024-12-30 11:46_

Not quite sure why https://github.com/apache/airflow/blob/e9412bf69e5f25a815b660e6f06fa1aeec6b062a/airflow/decorators/condition.py#L33 disappeared from the ecosystem hits

Maybe it'll become more obvious once the other PR has been merged and we get the real diff.

---

_Comment by @MichaReiser on 2025-01-03 09:12_

Reviewing this PR is a bit challenging right now because it's based on https://github.com/astral-sh/ruff/pull/15180 and I can't change the base because your an external contributor. I suggest we wait with reviewing until the base PR is merged. Please ping us if we happen to forget to review the PR after #15180 is merged.

---

_Comment by @Daverball on 2025-01-08 12:46_

@MichaReiser @AlexWaygood You should be able to review this now

---

_Label `rule` added by @dhruvmanila on 2025-01-08 13:00_

---

_Label `preview` added by @dhruvmanila on 2025-01-08 13:01_

---

_Comment by @Daverball on 2025-01-08 13:38_

The ecosystem results make sense to me now. Since airflow targets Python 3.9+ mypy would flag those type aliases if they were unquoted. So a TC008 shouldn't trigger, despite there being no runtime effects.

---

_Comment by @dangotbanned on 2025-01-10 11:01_

@Daverball 

I've got some false positives that seem to be this, in combination with being in a `TYPE_CHECKING` block:
- [workflow failure](https://github.com/vega/altair/actions/runs/12698005027/job/35395529522?pr=3747)
- Source
	- [1 & 3](https://github.com/vega/altair/blob/23b189103b8c5ac38928876cdde5ec5f71fcadc3/tools/schemapi/schemapi.py#L59)
	- [2](https://github.com/vega/altair/blob/23b189103b8c5ac38928876cdde5ec5f71fcadc3/tests/__init__.py#L24-L26)
- [`tool.ruff.target-version = "py39"`](https://github.com/vega/altair/blob/23b189103b8c5ac38928876cdde5ec5f71fcadc3/pyproject.toml#L206)

Should I open an issue for this?

---

_Comment by @Daverball on 2025-01-10 12:11_

@dangotbanned Yes, that's this bug, it can also be hit in different contexts, it just previously wasn't as likely to be hit in `TYPE_CHECKING` blocks, because those also weren't handled properly yet.

Feel free to open an issue, if you think it makes it easier for others to discover that this issue is known and will be fixed.

---

_Comment by @dangotbanned on 2025-01-10 12:24_

All good, thanks for clarifying @Daverball 🙂 

---

_Comment by @Daverball on 2025-01-14 07:28_

@MichaReiser @AlexWaygood Bumping this again, now that the 0.9 rush is over.

---

_Comment by @MichaReiser on 2025-01-15 09:27_

Thanks for the ping. We decided that Alex should spend more time on Red Knot which leaves me as the main reviewer. I'll try to take a look but I'm not sure when I'll get to it because it also requires familiarizing myself with the type checking rules. Sorry for the long wait.

---

_Comment by @Daverball on 2025-01-15 10:22_

@MichaReiser Alright, no worries. This PR should be fairly easy to review, even with surface knowledge, the only thing you need to know is that the union syntax `|` is only allowed to be used at runtime starting with Python 3.10, prior to that it will raise a `TypeError`, since `type` did not provide a `__or__` method that returns `UnionType`.

However type checkers still allow you to use the new syntax in older Python versions in non-runtime contexts like implicit/explicit forward references. But type checkers generally don't consider statements in `TYPE_CHECKING` blocks to be non-runtime. It is treated like any other code block, the only thing they do, is statically evaluate `TYPE_CHECKING` to `True` (and entirely skip analysis of code that isn't reachable this way).

There is a tiny bit of extra complexity for avoiding false negatives for `typing.Annotated`. This part should probably have been changed in #15180 in hindsight. All you need to know here is, that only the first argument to `typing.Annotated` is a type expression, so the rest of the arguments shouldn't influence our decision whether or not we can unquote.

TLDR: Annotated type alias value expressions always need to be considered in runtime context, unless they're explicitly wrapped in quotes. As a consequence, if a forward reference in the value expression uses `|`, we're not allowed to remove the quotes if the target python version is 3.9 or below.

---

_@MichaReiser approved on 2025-01-15 13:27_

Thanks for the detailed explanation! It helped me to build the necessary context quickly to review the PR. I really appreciate it. 

Looks good and thanks again for taking the time to explain the changes in detail to me.

---

_Merged by @MichaReiser on 2025-01-15 13:27_

---

_Closed by @MichaReiser on 2025-01-15 13:27_

---

_Branch deleted on 2025-01-15 13:31_

---
