```yaml
number: 8036
title: "[`pylint`] Implement `unnecessary-dict-index-lookup` (`PLR1733`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-R1733
created_at: 2023-10-18T04:11:56Z
updated_at: 2023-12-01T05:16:40Z
url: https://github.com/astral-sh/ruff/pull/8036
synced_at: 2026-01-12T15:55:25Z
```

# [`pylint`] Implement `unnecessary-dict-index-lookup` (`PLR1733`)

---

_@diceroll123_

## Summary

Add [R1733](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/unnecessary-dict-index-lookup.html) and autofix!

See #970 

## Test Plan

`cargo test` and manually

---

_Comment by @diceroll123 on 2023-10-18 04:17_

This PR is practically just #7999 in a trenchcoat

---

_Comment by @github-actions[bot] on 2023-10-18 04:31_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -1, 0 error(s))

<details><summary>rotki (+2, -1)</summary>
<p>

<pre>
- [*] 13 fixable with the `--fix` option (225 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 14 fixable with the `--fix` option (225 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/rotki/rotki/blob/e3f16072fa1e9bd28978c06ca0ff5556237a5b4f/rotkehlchen/tests/utils/checks.py#L68'>rotkehlchen/tests/utils/checks.py:68:53:</a> PLR1733 [*] Unnecessary dict index lookup
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLR1733 | 1 | 1 | 0 |



---

_Renamed from "[pylint] - add `unnecessary_dict_index_lookup` (`R1733`) + autofix" to "[pylint] - add `unnecessary_dict_index_lookup` (`PLR1733`) + autofix" by @diceroll123 on 2023-10-18 05:07_

---

_Comment by @Skylion007 on 2023-10-19 16:59_

Does this pylint rule cover dict comprehensions? or would that be covered by another rule? Or is it just not covered sadly?

---

_Comment by @diceroll123 on 2023-10-19 17:40_

> Does this pylint rule cover dict comprehensions? or would that be covered by another rule? Or is it just not covered sadly?

ooh, that was an oversight, I'll add all comprehensions later, thanks!

---

_Comment by @Skylion007 on 2023-10-29 18:39_

@charliermarsh Anything blocking these two PRs btw?

---

_Comment by @charliermarsh on 2023-10-29 18:42_

@Skylion007 - Just me finding time to review it, there's a lot of logic so I just need to read it closely.

---

_Comment by @Skylion007 on 2023-11-04 17:37_

I would add a test case for this bug: https://github.com/astral-sh/ruff/pull/7999#issuecomment-1793503531 . I suspect you would hit it with dict comprehensions as well.

---

_@Skylion007 reviewed on 2023-11-04 17:38_

---

_Review comment by @Skylion007 on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_dict_index_lookup.py`:32 on 2023-11-04 17:38_

What about accessing `_` after `FRUITS[fruit_name]` is edited? that's the main thing missing here.

---

_Comment by @codspeed-hq[bot] on 2023-11-04 18:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:add-R1733)

### Merging #8036 will **not alter performance**

<sub>Comparing <code>diceroll123:add-R1733</code> (92dc833) with <code>main</code> (69dfe0a)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-11-04 18:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/amazon/aws/executors/ecs/utils.py#L265'>airflow/providers/amazon/aws/executors/ecs/utils.py:265:31:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/amazon/aws/hooks/glue.py#L378'>airflow/providers/amazon/aws/hooks/glue.py:378:88:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/tests/utils/checks.py#L67'>rotkehlchen/tests/utils/checks.py:67:53:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1733 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_@Skylion007 reviewed on 2023-11-24 20:08_

---

_Review comment by @Skylion007 on `crates/ruff_linter/src/codes.rs`:257 on 2023-11-24 20:08_

Shouldn't this be 1 row down to maintain sorted order?

---

_@diceroll123 reviewed on 2023-11-24 20:14_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/codes.rs`:257 on 2023-11-24 20:14_

rebased and fixed!

---

_@charliermarsh reviewed on 2023-12-01 00:41_

@diceroll123 -- Thank you! Do you mind taking a look at some of the tweaks I made in https://github.com/astral-sh/ruff/pull/8932 and the comments I left to explain them, and applying those changes here?

---

_Comment by @diceroll123 on 2023-12-01 00:43_

Will do, I see you're still adding comments so I'll check back later!

---

_Review requested from @charliermarsh by @diceroll123 on 2023-12-01 04:42_

---

_@charliermarsh approved on 2023-12-01 05:04_

Thanks!

---

_Renamed from "[pylint] - add `unnecessary_dict_index_lookup` (`PLR1733`) + autofix" to "[`pylint`] Implement `unnecessary-dict-index-lookup` (`PLR1733`)" by @charliermarsh on 2023-12-01 05:04_

---

_Label `rule` added by @charliermarsh on 2023-12-01 05:04_

---

_Label `preview` added by @charliermarsh on 2023-12-01 05:04_

---

_Comment by @diceroll123 on 2023-12-01 05:05_

Thank YOU! 😄 

---

_Merged by @charliermarsh on 2023-12-01 05:09_

---

_Closed by @charliermarsh on 2023-12-01 05:09_

---
