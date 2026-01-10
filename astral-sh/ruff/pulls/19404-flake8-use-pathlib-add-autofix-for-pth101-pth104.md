```yaml
number: 19404
title: "[`flake8-use-pathlib`] Add autofix for `PTH101`, `PTH104`, `PTH105`, `PTH121`"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: feat/autofixes
created_at: 2025-07-17T16:15:52Z
updated_at: 2025-07-23T16:14:03Z
url: https://github.com/astral-sh/ruff/pull/19404
synced_at: 2026-01-10T17:58:13Z
```

# [`flake8-use-pathlib`] Add autofix for `PTH101`, `PTH104`, `PTH105`, `PTH121`

---

_Pull request opened by @chirizxc on 2025-07-17 16:15_


## Summary

Part of https://github.com/astral-sh/ruff/issues/2331

## Test Plan

`cargo nextest run flake8_use_pathlib`


---

_Comment by @chirizxc on 2025-07-17 16:16_

sorry for #19298 😕

---

_Label `fixes` added by @ntBre on 2025-07-17 16:25_

---

_Label `preview` added by @ntBre on 2025-07-17 16:25_

---

_Comment by @ntBre on 2025-07-17 16:26_

Do you know what happened on the other PR? I just saw something similar on https://github.com/astral-sh/ruff/pull/19338.

I'll try to take a look soon, but it might not be until next week. Thanks for working on these!

---

_Comment by @chirizxc on 2025-07-17 16:27_

> Do you know what happened on the other PR? I just saw something similar on #19338.
> 
> I'll try to take a look soon, but it might not be until next week. Thanks for working on these!

it was my mistake

---

_Comment by @github-actions[bot] on 2025-07-17 16:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +48 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +30 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/src/airflow/configuration.py#L2131'>airflow-core/src/airflow/configuration.py:2131:9:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/src/airflow/configuration.py#L2131'>airflow-core/src/airflow/configuration.py:2131:9:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
+ <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/src/airflow/dag_processing/bundles/base.py#L391'>airflow-core/src/airflow/dag_processing/bundles/base.py:391:13:</a> PTH105 [*] `os.replace()` should be replaced by `Path.replace()`
- <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/src/airflow/dag_processing/bundles/base.py#L391'>airflow-core/src/airflow/dag_processing/bundles/base.py:391:13:</a> PTH105 `os.replace()` should be replaced by `Path.replace()`
+ <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/src/airflow/utils/log/file_task_handler.py#L841'>airflow-core/src/airflow/utils/log/file_task_handler.py:841:17:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/src/airflow/utils/log/file_task_handler.py#L841'>airflow-core/src/airflow/utils/log/file_task_handler.py:841:17:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
+ <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/tests/unit/core/test_impersonation_tests.py#L71'>airflow-core/tests/unit/core/test_impersonation_tests.py:71:17:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/tests/unit/core/test_impersonation_tests.py#L71'>airflow-core/tests/unit/core/test_impersonation_tests.py:71:17:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
+ <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/tests/unit/core/test_impersonation_tests.py#L80'>airflow-core/tests/unit/core/test_impersonation_tests.py:80:21:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/tests/unit/core/test_impersonation_tests.py#L80'>airflow-core/tests/unit/core/test_impersonation_tests.py:80:21:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
+ <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/tests/unit/core/test_impersonation_tests.py#L87'>airflow-core/tests/unit/core/test_impersonation_tests.py:87:13:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/airflow-core/tests/unit/core/test_impersonation_tests.py#L87'>airflow-core/tests/unit/core/test_impersonation_tests.py:87:13:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
+ <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L104'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:104:13:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
... 14 additional changes omitted for rule PTH101
+ <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/dev/breeze/src/airflow_breeze/utils/reproducible.py#L144'>dev/breeze/src/airflow_breeze/utils/reproducible.py:144:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/dev/breeze/src/airflow_breeze/utils/reproducible.py#L144'>dev/breeze/src/airflow_breeze/utils/reproducible.py:144:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/scripts/in_container/run_migration_reference.py#L212'>scripts/in_container/run_migration_reference.py:212:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/apache/airflow/blob/5db1b715f7dac87c543865d1aece135a70f1c719/scripts/in_container/run_migration_reference.py#L212'>scripts/in_container/run_migration_reference.py:212:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/70660e6a10ad548c66a32a09fe2498ebf371f3c1/src/latch_cli/utils/__init__.py#L278'>src/latch_cli/utils/__init__.py:278:9:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/latchbio/latch/blob/70660e6a10ad548c66a32a09fe2498ebf371f3c1/src/latch_cli/utils/__init__.py#L278'>src/latch_cli/utils/__init__.py:278:9:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +16 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/scripts/lib/zulip_tools.py#L60'>scripts/lib/zulip_tools.py:60:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/scripts/lib/zulip_tools.py#L60'>scripts/lib/zulip_tools.py:60:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/scripts/lib/zulip_tools.py#L730'>scripts/lib/zulip_tools.py:730:5:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/scripts/lib/zulip_tools.py#L730'>scripts/lib/zulip_tools.py:730:5:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/tools/lib/provision_inner.py#L158'>tools/lib/provision_inner.py:158:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/tools/lib/provision_inner.py#L158'>tools/lib/provision_inner.py:158:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/zerver/management/commands/export_usermessage_batch.py#L46'>zerver/management/commands/export_usermessage_batch.py:46:17:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/zerver/management/commands/export_usermessage_batch.py#L46'>zerver/management/commands/export_usermessage_batch.py:46:17:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/zerver/management/commands/export_usermessage_batch.py#L60'>zerver/management/commands/export_usermessage_batch.py:60:17:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/zerver/management/commands/export_usermessage_batch.py#L60'>zerver/management/commands/export_usermessage_batch.py:60:17:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/zerver/management/commands/fetch_tor_exit_nodes.py#L71'>zerver/management/commands/fetch_tor_exit_nodes.py:71:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/zerver/management/commands/fetch_tor_exit_nodes.py#L71'>zerver/management/commands/fetch_tor_exit_nodes.py:71:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/zerver/tornado/event_queue.py#L714'>zerver/tornado/event_queue.py:714:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/zerver/tornado/event_queue.py#L714'>zerver/tornado/event_queue.py:714:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/zerver/worker/base.py#L151'>zerver/worker/base.py:151:13:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/a4d70505ecd253bd79ba4077b3c42e085db006b1/zerver/worker/base.py#L151'>zerver/worker/base.py:151:13:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH101 | 26 | 0 | 0 | 26 | 0 |
| PTH104 | 20 | 0 | 0 | 20 | 0 |
| PTH105 | 2 | 0 | 0 | 2 | 0 |

</p>
</details>




---

_Comment by @StDymphna on 2025-07-18 14:20_

@chirizxc
Since you've been implementing autofixes for PTH* rules, are you planning on implementing autofixes for FURB101 and FURB103 (which are PTH related)?

---

_Comment by @chirizxc on 2025-07-18 14:41_

> @chirizxc
> Since you've been implementing autofixes for PTH* rules, are you planning on implementing autofixes for FURB101 and FURB103 (which are PTH related)?

I think so, but I won't promise😴

---

_Review requested from @ntBre by @MichaReiser on 2025-07-23 06:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__full_name.py.snap`:1 on 2025-07-23 14:20_

This is one of the files where it looks like we lost diagnostics.

---

_@ntBre requested changes on 2025-07-23 14:21_

The implementation looked good to me, but do you know what happened with the missing snapshots? It looks like we lost a bunch of violations even though their Python files aren't modified. It might be a minor issue with the `is_rule_enabled` checks, hopefully.

---

_Comment by @chirizxc on 2025-07-23 14:41_

> The implementation looked good to me, but do you know what happened with the missing snapshots? It looks like we lost a bunch of violations even though their Python files aren't modified. It might be a minor issue with the `is_rule_enabled` checks, hopefully.

Initial tests in the files are not correct

---

_Comment by @chirizxc on 2025-07-23 14:43_

These tests were taken from the original flake8-use-pathlib repository, now if you try to run these tests you will get Missing argument, since they require two arguments in general

---

_Comment by @ntBre on 2025-07-23 14:44_

Ohhh, they have the wrong number of arguments? Okay, this is probably good to go then. I'll take one more look at the snapshots. Thank you for clarifying and for fixing that pre-existing bug!

---

_Comment by @ntBre on 2025-07-23 14:46_

Hmm, on second thought, maybe we still want diagnostics in theses cases and just to suppress the fixes. What do you think about that? I think the diagnostics are still useful, but fixing could be dangerous since there's another error there already.

---

_Comment by @chirizxc on 2025-07-23 14:49_

I think it's a bit weird to suggest diagnostic when the code can't run due to an error

---

_Comment by @chirizxc on 2025-07-23 14:59_

> Hmm, on second thought, maybe we still want diagnostics in theses cases and just to suppress the fixes. What do you think about that? I think the diagnostics are still useful, but fixing could be dangerous since there's another error there already.

in some rules it already works like that, we can just take the behavior from them, if not, then we need to think🤔

---

_Comment by @ntBre on 2025-07-23 15:02_

> I think it's a bit weird to suggest diagnostic when the code can't run due to an error

I can see what you mean, but in general I think we try to show as many diagnostics as possible. The case I usually picture is someone getting a different diagnostic about the wrong number of arguments (maybe from a type checker or LSP), fixing that, and then a new Ruff diagnostic appears. Instead we'd prefer for the Ruff diagnostic to be there from the start. They _are_ using (or trying to use) an `os` method replaceable by `pathlib` too, so I think the diagnostic is still accurate. I momentarily forgot this rule of thumb in my initial comment :sweat_smile: 

In this case it also preserves the existing behavior, which is nice since adding an autofix usually shouldn't modify the other behavior of the rule.

> in some rules it already works like that, we can just take the behavior from them, if not, then we need to think🤔

Which rules do you mean here?

---

_Comment by @chirizxc on 2025-07-23 15:28_

> > I think it's a bit weird to suggest diagnostic when the code can't run due to an error
> 
> I can see what you mean, but in general I think we try to show as many diagnostics as possible. The case I usually picture is someone getting a different diagnostic about the wrong number of arguments (maybe from a type checker or LSP), fixing that, and then a new Ruff diagnostic appears. Instead we'd prefer for the Ruff diagnostic to be there from the start. They _are_ using (or trying to use) an `os` method replaceable by `pathlib` too, so I think the diagnostic is still accurate. I momentarily forgot this rule of thumb in my initial comment 😅
> 
> In this case it also preserves the existing behavior, which is nice since adding an autofix usually shouldn't modify the other behavior of the rule.
> 
> > in some rules it already works like that, we can just take the behavior from them, if not, then we need to think🤔
> 
> Which rules do you mean here?

Misspelled, meant IF this behavior is already used in other rules, we should do the same thing

---

_Comment by @chirizxc on 2025-07-23 15:43_

done

---

_Review requested from @ntBre by @chirizxc on 2025-07-23 15:53_

---

_@ntBre approved on 2025-07-23 16:12_

Thank you!

---

_Merged by @ntBre on 2025-07-23 16:13_

---

_Closed by @ntBre on 2025-07-23 16:13_

---

_Branch deleted on 2025-07-23 16:14_

---
