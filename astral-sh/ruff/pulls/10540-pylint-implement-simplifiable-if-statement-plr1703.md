```yaml
number: 10540
title: "[`pylint`] Implement `simplifiable-if-statement` (`PLR1703`)"
type: pull_request
state: closed
author: augustelalande
labels: []
assignees: []
base: main
head: simplifiable-if-statement
created_at: 2024-03-24T00:39:04Z
updated_at: 2024-04-22T18:02:07Z
url: https://github.com/astral-sh/ruff/pull/10540
synced_at: 2026-01-10T22:37:01Z
```

# [`pylint`] Implement `simplifiable-if-statement` (`PLR1703`)

---

_Pull request opened by @augustelalande on 2024-03-24 00:39_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement `simplifiable-if-statement` from pylint, part of #970.

The implementation is largely derived from the `needless-bool` implementation in `flake8-simplify`.

## Test Plan

Text fixture added.


---

_Comment by @augustelalande on 2024-03-24 00:44_

I now see this is a duplicate of #9656 ugh

---

_Comment by @augustelalande on 2024-03-24 00:54_

I'll close this in favor of #9656 if requested, but I think this PR should be easier to merge considering it largely mirrors `needless-bool` which has been in the code base for a while and is stable.

Anyway I leave it up to you guys.

---

_Comment by @github-actions[bot] on 2024-03-24 01:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/25eda43904cdfb481759a7c497716b4839956014/securedrop/journalist_app/admin.py#L49'>securedrop/journalist_app/admin.py:49:9:</a> PLR1703 Assign the condition directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/25eda43904cdfb481759a7c497716b4839956014/securedrop/models.py#L148'>securedrop/models.py:148:9:</a> PLR1703 Assign the condition `self.star and self.star.starred` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/669ddfb343bfc7f32f30ba14e2369ff2f3ebbc12/pandas/plotting/_matplotlib/core.py#L300'>pandas/plotting/_matplotlib/core.py:300:13:</a> PLR1703 Assign the condition `ax is None and by is None` directly
+ <a href='https://github.com/pandas-dev/pandas/blob/669ddfb343bfc7f32f30ba14e2369ff2f3ebbc12/pandas/tests/groupby/aggregate/test_cython.py#L323'>pandas/tests/groupby/aggregate/test_cython.py:323:5:</a> PLR1703 Assign the condition `op_name in ("mean", "median")` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/e5d50d9787f735a1fe5e9c8ff080ccd8b54d35f3/zproject/computed_settings.py#L97'>zproject/computed_settings.py:97:1:</a> PLR1703 Assign the condition directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1703 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-04-07 03:01_

Unfortunately I think this is also a duplicate of [`if-else-block-instead-of-if-exp`](https://docs.astral.sh/ruff/rules/if-else-block-instead-of-if-exp/) based on playing around in the playground and looking through the examples -- sorry about that. I'm gonna close for now, but let me know if I'm missing any differences between the two rules.

---

_Closed by @charliermarsh on 2024-04-07 03:01_

---

_Branch deleted on 2024-04-22 18:02_

---
