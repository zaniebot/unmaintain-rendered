```yaml
number: 17656
title: "[`flake8-return`] Fix false-positive for variables used inside nested functions in `RET504`"
type: pull_request
state: closed
author: LaBatata101
labels:
  - bug
assignees: []
base: main
head: fix-RET504
created_at: 2025-04-27T20:41:41Z
updated_at: 2025-06-02T19:51:43Z
url: https://github.com/astral-sh/ruff/pull/17656
synced_at: 2026-01-10T18:45:04Z
```

# [`flake8-return`] Fix false-positive for variables used inside nested functions in `RET504`

---

_Pull request opened by @LaBatata101 on 2025-04-27 20:41_


<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Fixes #14052.

## Test Plan

<!-- How was it tested? -->
Snapshot tests.

---

_Comment by @github-actions[bot] on 2025-04-27 20:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -8 violations, +0 -0 fixes in 3 projects; 1 project error; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/407ef720f43882aca9331480affe5fcbcd310249/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L464'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:464:12:</a> RET504 Unnecessary assignment to `safe_dag_prefix` before `return` statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/871cfe0c78686867b79c0476793daa208d060ec4/scripts/erd/erd.py#L195'>scripts/erd/erd.py:195:5:</a> D413 [*] Missing blank line after last section ("Parameters")
- <a href='https://github.com/apache/superset/blob/871cfe0c78686867b79c0476793daa208d060ec4/scripts/erd/erd.py#L195'>scripts/erd/erd.py:195:5:</a> D413 [*] Missing blank line after last section ("Parameters")
+ <a href='https://github.com/apache/superset/blob/871cfe0c78686867b79c0476793daa208d060ec4/tests/integration_tests/base_tests.py#L553'>tests/integration_tests/base_tests.py:553:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
- <a href='https://github.com/apache/superset/blob/871cfe0c78686867b79c0476793daa208d060ec4/tests/integration_tests/base_tests.py#L553'>tests/integration_tests/base_tests.py:553:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
+ <a href='https://github.com/apache/superset/blob/871cfe0c78686867b79c0476793daa208d060ec4/tests/integration_tests/reports/utils.py#L194'>tests/integration_tests/reports/utils.py:194:5:</a> ANN201 Missing return type annotation for public function `create_dashboard_report`
- <a href='https://github.com/apache/superset/blob/871cfe0c78686867b79c0476793daa208d060ec4/tests/integration_tests/reports/utils.py#L194'>tests/integration_tests/reports/utils.py:194:5:</a> ANN201 Missing return type annotation for public function `create_dashboard_report`
+ <a href='https://github.com/apache/superset/blob/871cfe0c78686867b79c0476793daa208d060ec4/tests/unit_tests/fixtures/bash_mock.py#L23'>tests/unit_tests/fixtures/bash_mock.py:23:9:</a> ANN205 Missing return type annotation for staticmethod `tag_latest_release`
- <a href='https://github.com/apache/superset/blob/871cfe0c78686867b79c0476793daa208d060ec4/tests/unit_tests/fixtures/bash_mock.py#L23'>tests/unit_tests/fixtures/bash_mock.py:23:9:</a> ANN205 Missing return type annotation for staticmethod `tag_latest_release`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/2b089c7e98149a7916b25efaadb5f0cbd3cc5ae9/zerver/lib/generate_test_data.py#L16'>zerver/lib/generate_test_data.py:16:12:</a> RET504 Unnecessary assignment to `config` before `return` statement
- <a href='https://github.com/zulip/zulip/blob/2b089c7e98149a7916b25efaadb5f0cbd3cc5ae9/zerver/lib/query_helpers.py#L15'>zerver/lib/query_helpers.py:15:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/zulip/zulip/blob/2b089c7e98149a7916b25efaadb5f0cbd3cc5ae9/zerver/lib/query_helpers.py#L15'>zerver/lib/query_helpers.py:15:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/zulip/zulip/blob/2b089c7e98149a7916b25efaadb5f0cbd3cc5ae9/zerver/lib/test_helpers.py#L784'>zerver/lib/test_helpers.py:784:5:</a> D400 First line should end with a period
- <a href='https://github.com/zulip/zulip/blob/2b089c7e98149a7916b25efaadb5f0cbd3cc5ae9/zerver/lib/test_helpers.py#L784'>zerver/lib/test_helpers.py:784:5:</a> D400 First line should end with a period
</pre>

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
Failed to clone openai/openai-cookbook: error: 6635 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>
<details><summary>Changes by rule (7 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D413 | 2 | 1 | 1 | 0 | 0 |
| PLR0913 | 2 | 1 | 1 | 0 | 0 |
| ANN201 | 2 | 1 | 1 | 0 | 0 |
| ANN205 | 2 | 1 | 1 | 0 | 0 |
| D404 | 2 | 1 | 1 | 0 | 0 |
| D400 | 2 | 1 | 1 | 0 | 0 |
| RET504 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -3 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/407ef720f43882aca9331480affe5fcbcd310249/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L464'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:464:12:</a> RET504 Unnecessary assignment to `safe_dag_prefix` before `return` statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/871cfe0c78686867b79c0476793daa208d060ec4/superset/sql_lab.py#L680'>superset/sql_lab.py:680:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/871cfe0c78686867b79c0476793daa208d060ec4/superset/sql_lab.py#L680'>superset/sql_lab.py:680:5:</a> DOC201 `return` is not documented in docstring
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/2b089c7e98149a7916b25efaadb5f0cbd3cc5ae9/zerver/lib/generate_test_data.py#L16'>zerver/lib/generate_test_data.py:16:12:</a> RET504 Unnecessary assignment to `config` before `return` statement
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC201 | 2 | 1 | 1 | 0 | 0 |
| RET504 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @ntBre on 2025-04-28 20:39_

---

_Renamed from "[`flake8_return`] Fix false-positive for variables used inside nested functions in `RET504`" to "[`flake8-return`] Fix false-positive for variables used inside nested functions in `RET504`" by @LaBatata101 on 2025-05-01 13:39_

---

_@MichaReiser reviewed on 2025-05-03 14:04_

I'm not sure this is the right fix because it only works for directly nested functions but not functions e.g. nested inside a `with` statement. 

Recursively visiting nested functions is also somewhat expensive because it requires traversing the entire body of the function (deep!) and we even do this multiple times if functions are nested, even if there are no violations. 

I'd prefer if we could make use of the information already present in the `SemanticModel` (binding, scopes). However, this may require making this a deferred rule that runs after the entire semantic model is fully built.

---

_Comment by @LaBatata101 on 2025-05-03 14:20_

> I'd prefer if we could make use of the information already present in the SemanticModel (binding, scopes). However, this may require making this a deferred rule that runs after the entire semantic model is fully built.

Alright!

---

_Comment by @LaBatata101 on 2025-05-03 22:01_

I've put the rule to execute in [bindings.rs](https://github.com/astral-sh/ruff/pull/17656/files#diff-05a851ce3de5468f8c436e034f9c1485d7d34dac4f245d26a683b636ed422f59) not sure if there's a better place to do it. 

I didn't get it why the snapshot changed, but it seems correct??

---

_@LaBatata101 reviewed on 2025-05-03 22:02_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:628 on 2025-05-03 22:02_

Is there a better way of getting the binding for the `assigned_id`?

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-05-03 22:02_

---

_Comment by @ntBre on 2025-05-05 20:03_

> I've put the rule to execute in [bindings.rs](https://github.com/astral-sh/ruff/pull/17656/files#diff-05a851ce3de5468f8c436e034f9c1485d7d34dac4f245d26a683b636ed422f59) not sure if there's a better place to do it.
> 
> I didn't get it why the snapshot changed, but it seems correct??

It looks like there is a duplicate diagnostic. If you view the whole file it's more obvious than in the diff:

https://github.com/astral-sh/ruff/blob/16c055b0bc2720334694df2e28514ed7f7882dc2/crates/ruff_linter/src/rules/flake8_return/snapshots/ruff_linter__rules__flake8_return__tests__RET504_RET504.py.snap#L84-L122

These are both emitted on line 269. I'm guessing that's the cause of the numerous new ecosystem hits too.

---

_Comment by @LaBatata101 on 2025-05-05 20:08_

> I'm guessing that's the cause of the numerous new ecosystem hits too.

Wow, didn't see that until you mentioned. `github-actions` doesn't notify me when it edits the ecosystem comment.

---

_Comment by @LaBatata101 on 2025-05-06 20:55_

@ntBre I fixed the duplicated diagnostics issue, but the ecosystem changes still looks weird.

---

_Comment by @ntBre on 2025-05-07 20:34_

> @ntBre I fixed the duplicated diagnostics issue, but the ecosystem changes still looks weird.

I _think_ you can safely ignore the changes with non-`RET504` codes because I don't think any of your changes should affect other rules. The two ecosystem hits for `RET504` itself look correct to me. Is there anything else that looks weird?

I'll let Micha re-review this, but we might want to add a test for his suggestion about nested scopes to show off your binding-based implementation. Otherwise this looks reasonable to me based on a quick look. Happy to review in more detail if y'all want!

---

_Comment by @LaBatata101 on 2025-05-07 21:01_

> I think you can safely ignore the changes with non-RET504 codes because I don't think any of your changes should affect other rules. The two ecosystem hits for RET504 itself look correct to me. Is there anything else that looks weird?

Nope, not really.

I have this other open PR #17692 (which is not related to this one) that I think you guys may have missed, could you take a look? 

Anyway, thanks for the help!



---

_Review requested from @ntBre by @MichaReiser on 2025-05-26 09:37_

---

_Closed by @LaBatata101 on 2025-06-02 14:05_

---

_Branch deleted on 2025-06-02 14:05_

---

_Comment by @ntBre on 2025-06-02 16:25_

Sorry for the delay reviewing this. Are you planning to reopen, or was there something wrong with the PR? Just checking before I clear it from my notifications.

---

_Comment by @LaBatata101 on 2025-06-02 16:53_

> Sorry for the delay reviewing this. Are you planning to reopen, or was there something wrong with the PR? Just checking before I clear it from my notifications.

I was fixing another issue for RET-504 and accidentally made a branch with the same name as this one. Git told me the branch already existed, so I just deleted it. I didn’t realize this PR was still open though 😅. That's why it got closed. Not sure how to fix this.

---

_Comment by @ntBre on 2025-06-02 17:30_

Ahh, I see. I was going to say I'd just review the other PR and then we can try to resurrect this branch afterwards, but I wonder if it might resurrect the second one instead. It might be easiest to open a new PR if you can recover this branch locally. I think you can probably do

```shell
git branch new-branch-name 31f62a1eca7a4b9e17f75231510dd1e5db54c1a4  # looks like the most recent hash above
```

or something else with `git reflog`, if you need it. I usually have to double check the git commands I use less frequently :laughing:

https://stackoverflow.com/questions/3640764/can-i-recover-a-branch-after-its-deletion-in-git 


---

_Comment by @LaBatata101 on 2025-06-02 19:43_

> `git branch new-branch-name 31f62a1eca7a4b9e17f75231510dd1e5db54c1a4 `

This command worked like a charm. Here's the new [PR](https://github.com/astral-sh/ruff/pull/18433/).

Thanks!

---
