```yaml
number: 11168
title: F401 - Distinguish between imports we wish to remove and those we wish to make explicit-exports
type: pull_request
state: merged
author: plredmond
labels:
  - rule
assignees: []
merged: true
base: main
head: ruff.F401
created_at: 2024-04-26T22:45:32Z
updated_at: 2024-05-03T05:15:12Z
url: https://github.com/astral-sh/ruff/pull/11168
synced_at: 2026-01-10T22:37:02Z
```

# F401 - Distinguish between imports we wish to remove and those we wish to make explicit-exports

---

_Pull request opened by @plredmond on 2024-04-26 22:45_

Resolves #10390 and starts to address #10391

# Changes to behavior

* In `__init__.py` we now offer some fixes for unused imports.
  * If the import binding is first-party this PR suggests a fix to turn it into a redundant alias.
  * If the import binding is not first-party, this PR suggests a fix to remove it from the `__init__.py`.
* The fix-titles are specific to these new suggested fixes.
* `checker.settings.ignore_init_module_imports` setting is deprecated/ignored. There is probably a documentation change to make that complete which I haven't done.

---

<details><summary>Old description of implementation changes</summary>

# Changes to the implementation

* In the body of the loop over import statements that contain unused bindings, the bindings are partitioned into `to_reexport` and `to_remove` (according to how we want to resolve the fact they're unused) with the following predicate:
  ```rust
  in_init && is_first_party(checker, &import.qualified_name().to_string()) // true means make it a reexport
  ```
* Instead of generating a single fix per import statement, we now generate up to two fixes per import statement:
  ```rust
  (fix_by_removing_imports(checker, node_id, &to_remove, in_init).ok(),
   fix_by_reexporting(checker, node_id, &to_reexport, dunder_all).ok())
  ```
* The `to_remove` fixes are unsafe when `in_init`.
* The `to_explicit` fixes are safe. Currently, until a future PR, we make them redundant aliases (e.g. `import a` would become `import a as a`).

## Other changes

* `checker.settings.ignore_init_module_imports` is deprecated/ignored. Instead, all fixes are gated on `checker.settings.preview.is_enabled()`.
* Got rid of the pattern match on the import-binding bound by the inner loop because it seemed less readable than referencing fields on the binding.
* [x] `// FIXME: rename "imports" to "bindings"` if reviewer agrees (see code)
* [x] `// FIXME: rename "node_id" to "import_statement"` if reviewer agrees (see code)

<details>
<summary><h2>Scope cut until a future PR</h2></summary>

* (Not implemented) The `to_explicit` fixes will be added to `__all__` unless it doesn't exist. When `__all__` doesn't exist they're resolved by converting to redundant aliases (e.g. `import a` would become `import a as a`).
 
---

</details>

# Test plan

* [x] `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_24` contains an `__init__.py` with*out* `__all__` that exercises the features in this PR, but it doesn't pass.
* [x] `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_25_dunder_all` contains an `__init__.py` *with* `__all__` that exercises the features in this PR, but it doesn't pass.
* [x] Write unit tests for the new edit functions in `fix::edits::make_redundant_alias`.

</details>

---

_Marked ready for review by @plredmond on 2024-04-29 21:12_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_24/__init__.py`:37 on 2024-04-29 21:13_

What's up with the crazy spacing in this file?

---

_@plredmond reviewed on 2024-04-29 21:13_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:132 on 2024-04-29 21:13_

saying `0` means 'absolute import' which might be wrong

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_25__all/__init__.py`:44 on 2024-04-29 21:14_

Should we add comments to all of the empty helper files saying what they're used for?

---

_@plredmond reviewed on 2024-04-29 21:14_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:205 on 2024-04-29 21:14_

reviewer, please approve or deny these renames? i think they'd improve the readability of the code

---

_@plredmond reviewed on 2024-04-29 21:14_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:208 on 2024-04-29 21:14_

i have work on this part, but it's been cut from this pr

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-04-29 21:15_

Should we just pass default values for these?

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:205 on 2024-04-29 21:16_

These are fine with me

---

_Comment by @github-actions[bot] on 2024-04-29 21:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +104 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:21:</a> F401 [*] `.backup.ScheduledBackup` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:21:</a> F401 `.backup.ScheduledBackup` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:38:</a> F401 [*] `.backup.ScheduledBackupRun` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:38:</a> F401 `.backup.ScheduledBackupRun` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +100 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L10'>latch/__init__.py:10:5:</a> F401 [*] `latch.functions.operators.group_tuple` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L10'>latch/__init__.py:10:5:</a> F401 `latch.functions.operators.group_tuple` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L11'>latch/__init__.py:11:5:</a> F401 [*] `latch.functions.operators.inner_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L11'>latch/__init__.py:11:5:</a> F401 `latch.functions.operators.inner_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L12'>latch/__init__.py:12:5:</a> F401 [*] `latch.functions.operators.latch_filter` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L12'>latch/__init__.py:12:5:</a> F401 `latch.functions.operators.latch_filter` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L13'>latch/__init__.py:13:5:</a> F401 [*] `latch.functions.operators.left_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L13'>latch/__init__.py:13:5:</a> F401 `latch.functions.operators.left_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L14'>latch/__init__.py:14:5:</a> F401 [*] `latch.functions.operators.outer_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L14'>latch/__init__.py:14:5:</a> F401 `latch.functions.operators.outer_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L15'>latch/__init__.py:15:5:</a> F401 [*] `latch.functions.operators.right_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L15'>latch/__init__.py:15:5:</a> F401 `latch.functions.operators.right_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L17'>latch/__init__.py:17:41:</a> F401 [*] `latch.resources.conditional.create_conditional_section` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L17'>latch/__init__.py:17:41:</a> F401 `latch.resources.conditional.create_conditional_section` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L18'>latch/__init__.py:18:39:</a> F401 [*] `latch.resources.map_tasks.map_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L18'>latch/__init__.py:18:39:</a> F401 `latch.resources.map_tasks.map_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L19'>latch/__init__.py:19:48:</a> F401 [*] `latch.resources.reference_workflow.workflow_reference` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L19'>latch/__init__.py:19:48:</a> F401 `latch.resources.reference_workflow.workflow_reference` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L21'>latch/__init__.py:21:5:</a> F401 [*] `latch.resources.tasks.custom_memory_optimized_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L21'>latch/__init__.py:21:5:</a> F401 `latch.resources.tasks.custom_memory_optimized_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L22'>latch/__init__.py:22:5:</a> F401 [*] `latch.resources.tasks.custom_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L22'>latch/__init__.py:22:5:</a> F401 `latch.resources.tasks.custom_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L23'>latch/__init__.py:23:5:</a> F401 [*] `latch.resources.tasks.large_gpu_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L23'>latch/__init__.py:23:5:</a> F401 `latch.resources.tasks.large_gpu_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L24'>latch/__init__.py:24:5:</a> F401 [*] `latch.resources.tasks.large_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L24'>latch/__init__.py:24:5:</a> F401 `latch.resources.tasks.large_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L25'>latch/__init__.py:25:5:</a> F401 [*] `latch.resources.tasks.medium_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L25'>latch/__init__.py:25:5:</a> F401 `latch.resources.tasks.medium_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L26'>latch/__init__.py:26:5:</a> F401 [*] `latch.resources.tasks.small_gpu_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L26'>latch/__init__.py:26:5:</a> F401 `latch.resources.tasks.small_gpu_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L27'>latch/__init__.py:27:5:</a> F401 [*] `latch.resources.tasks.small_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L27'>latch/__init__.py:27:5:</a> F401 `latch.resources.tasks.small_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L29'>latch/__init__.py:29:38:</a> F401 [*] `latch.resources.workflow.workflow` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L29'>latch/__init__.py:29:38:</a> F401 `latch.resources.workflow.workflow` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L7'>latch/__init__.py:7:38:</a> F401 [*] `latch.functions.messages.message` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L7'>latch/__init__.py:7:38:</a> F401 `latch.functions.messages.message` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L9'>latch/__init__.py:9:5:</a> F401 [*] `latch.functions.operators.combine` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/__init__.py#L9'>latch/__init__.py:9:5:</a> F401 `latch.functions.operators.combine` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/types/__init__.py#L10'>latch/types/__init__.py:10:5:</a> F401 [*] `latch.types.metadata.LatchMetadata` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/types/__init__.py#L10'>latch/types/__init__.py:10:5:</a> F401 `latch.types.metadata.LatchMetadata` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/types/__init__.py#L11'>latch/types/__init__.py:11:5:</a> F401 [*] `latch.types.metadata.LatchParameter` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/types/__init__.py#L11'>latch/types/__init__.py:11:5:</a> F401 `latch.types.metadata.LatchParameter` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/types/__init__.py#L12'>latch/types/__init__.py:12:5:</a> F401 [*] `latch.types.metadata.LatchRule` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/types/__init__.py#L12'>latch/types/__init__.py:12:5:</a> F401 `latch.types.metadata.LatchRule` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/types/__init__.py#L13'>latch/types/__init__.py:13:5:</a> F401 [*] `latch.types.metadata.Params` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/types/__init__.py#L13'>latch/types/__init__.py:13:5:</a> F401 `latch.types.metadata.Params` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
+ <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/types/__init__.py#L14'>latch/types/__init__.py:14:5:</a> F401 [*] `latch.types.metadata.Section` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/d6695988e6cc19196f39b4ca056814b24be51555/latch/types/__init__.py#L14'>latch/types/__init__.py:14:5:</a> F401 `latch.types.metadata.Section` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
... 52 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F401 | 104 | 0 | 0 | 104 | 0 |

</p>
</details>




---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:42 on 2024-04-29 21:17_

Can you file an issue to fix this underline and help message? We should probably highlight "renamed as bees" 

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:25 on 2024-04-29 21:18_

This help message is wrong, it should say something about exporting.

---

_@zanieb reviewed on 2024-04-29 21:20_

---

_Comment by @zanieb on 2024-04-29 21:21_

The ecosystem checks look concerning. We're losing fixes in preview? I think there should be no changes in stable?

---

_@zanieb reviewed on 2024-04-29 21:31_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_24/__init__.py`:37 on 2024-04-29 21:31_

Presuming this is to make the snapshots clearer, but I don't think we do this elsewhere I would stick with the existing style

---

_@plredmond reviewed on 2024-04-29 21:50_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_24/__init__.py`:37 on 2024-04-29 21:50_

Yeah, I was having trouble reading the snapshots. Willfix.

---

_@plredmond reviewed on 2024-04-29 21:51_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-04-29 21:51_

You mean `Default::default()` values? I don't know. I'm following the examples from the isort code that I saw elsewhere.

---

_@plredmond reviewed on 2024-04-29 21:51_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:205 on 2024-04-29 21:51_

Ok, I'll do a commit with these.

---

_@plredmond reviewed on 2024-04-29 21:52_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:42 on 2024-04-29 21:52_

Are you saying that the diagnostic should always apply to the whole alias (name and asname) or are you saying that we should special-case the the renamed-but-unused case?

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:25 on 2024-04-29 21:53_

Yeah, I didn't touch the diagnostic generation code. My bad. I'll take a look at improving that.

---

_@plredmond reviewed on 2024-04-29 21:53_

---

_Comment by @plredmond on 2024-04-29 21:53_

Yes, yes those do look concerning. I'll look into it.

---

_@zanieb reviewed on 2024-04-29 21:58_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-04-29 21:58_

Well, we're not sorting imports so I feel like it's weird to use these import sort section flags from the settings for categorization here. Maybe it's not... but I'd imagine setting values that make sense for this rule e.g. `no_sections` should be `false`, `section_order` should be an empty slice, and the `default_section` should be.. first party? so we don't remove things that we're not sure about? not sure.

---

_@zanieb reviewed on 2024-04-29 22:00_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:42 on 2024-04-29 22:00_

I think that "Remove unused import: `.renamed`" with only "bees" highlighted is very confusing. I'd need to look at more cases to determine what we should change it to — that seems out of scope here.

---

_@zanieb reviewed on 2024-04-29 22:01_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:25 on 2024-04-29 22:01_

Usually you have the violation take an enum then use that to generate the proper message

---

_@zanieb reviewed on 2024-04-29 22:02_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:25 on 2024-04-29 22:02_

e.g. https://github.com/astral-sh/ruff/blob/a6d892b1f4b45bfed918f3240218d5fcd39d3099/crates/ruff_linter/src/rules/flake8_bandit/rules/bad_file_permissions.rs#L43-L52

---

_@plredmond reviewed on 2024-04-29 22:21_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_25__all/__init__.py`:44 on 2024-04-29 22:21_

I can do that.

---

_@plredmond reviewed on 2024-04-29 22:26_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-04-29 22:26_

This function has `#[allow(clippy::too_many_arguments)]` on it and no docstring. I wasn't about to dig into its implementation to figure out the meaning of all the arguments, since the tests are passing and the behavior is correct. I'll do as you recommend and ensure the tests work. Thanks for looking.

---

_@plredmond reviewed on 2024-04-29 22:27_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:42 on 2024-04-29 22:27_

Ok, I'll file an issue to point out that this rule only highlights the as-name part of the alias and that's confusing. :+1: 

---

_@plredmond reviewed on 2024-04-30 00:05_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:25 on 2024-04-30 00:05_

Thanks! I hadn't picked up on this pattern.

---

_Review requested from @MichaReiser by @zanieb on 2024-04-30 15:31_

---

_@plredmond reviewed on 2024-04-30 20:44_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-04-30 20:44_

Oof, I tried those arguments and it starts miscategorizing stdin imports as first party. Hmm. More investigation is required.

I read the function body a bit, and I think the problem is probably just changing the default_section argument.

---

_@plredmond reviewed on 2024-04-30 21:07_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-04-30 21:07_

Ok, I tried various combinations of these arguments and got various incorrect categorization results, so I'm going to leave this as-is.

---

_@plredmond reviewed on 2024-04-30 23:43_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:25 on 2024-04-30 23:43_

I changed it so that when in `__init__.py` and the import is first-party and there's no `__all__` it says to "use a redundant alias" and if there is a `__all__` it says "add to `__all__`"; otherwise it falls back to the text "remove unused import". This isn't perfect, because it means in the `import foo as bar` case it recommends one of the export texts, not "remove unused import".

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_24____init__.py.snap`:42 on 2024-04-30 23:48_

Filed https://github.com/astral-sh/ruff/issues/11223

---

_@plredmond reviewed on 2024-04-30 23:48_

---

_@zanieb reviewed on 2024-04-30 23:49_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-04-30 23:49_

I'm confused. It seems like using whatever our defaults are for the settings should give the correct behavior?

If a user changes these import sort settings, we shouldn't see a change of behavior in this rule — that sounds incorrect.

---

_@charliermarsh reviewed on 2024-05-01 04:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:132 on 2024-05-01 04:59_

Yeah we can't do this here -- we need to know the level. This is the cause of the PostHog regressions in the ecosystem checks.

---

_@charliermarsh reviewed on 2024-05-01 04:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-05-01 04:59_

We should use the user-defined settings IMO. I think what we have here is correct.

---

_@charliermarsh reviewed on 2024-05-01 05:00_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:208 on 2024-05-01 05:00_

I think best-practice would then be to cut all the other `dunder_all`-related code from this PR. Otherwise, it's not really being tested in the PR that introduces it. And who knows -- maybe we never address this TODO, and that code remains dead in the codebase forever.

---

_@charliermarsh reviewed on 2024-05-01 05:05_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:129 on 2024-05-01 05:05_

Can we import these at the top-level, rather than in here?

---

_@charliermarsh reviewed on 2024-05-01 05:05_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:128 on 2024-05-01 05:05_

We would typically pass checker as the second argument. We tend to lead with the data that the function is about, and pass extra structs to query as secondary arguments.

---

_@charliermarsh reviewed on 2024-05-01 05:07_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:117 on 2024-05-01 05:07_

I guess it's not new, but I'm finding the selective inclusion of `name` here to be confusing as a user, since we show one diagnostic at a time that underlines the import, but then _sometimes_ include the name and sometimes don't. I'd vote to just remove `name` here while we're changing it.

---

_Comment by @charliermarsh on 2024-05-01 05:08_

Not sure what's up with the Latch changes. I ran it locally and they are indeed classified as first-party (at least by the isort rule).

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/edits.rs`:127 on 2024-05-01 08:08_

Nit: Accepting an `iter` gives us a lot of flexibility but it seems that we have a `Vec` on all call sites. That's why I would change the signature to accept a `&'a [str]`. It has the added benefit that it avoids monomorphizing the function (not a huge problem here because the function itself isn't large).

If you want to keep it an iterator, than I suggest considering to accept a `IntoIterator` because it makes calling the function more convenient. E.g. `make_redundant_alias(vec!["x"].into_iter(), stmt),` becomes `make_redundant_alias(vec!["x"], stmt)`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/edits.rs`:128 on 2024-05-01 08:09_

Nit: You can consider using `AnyImport` here to get this type checked 

```suggestion
		import: AnyImport
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/edits.rs`:141 on 2024-05-01 08:10_

Nit Nit: You could consider inversing the order here because testing if something is `None` is significantly cheaper than comparing two names
```suggestion
                .find(|alias| alias.asname.is_none() && name == alias.name.id)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/edits.rs`:565 on 2024-05-01 08:11_

Nit: You don't need a vec here, you can pass an array

```suggestion
            make_redundant_alias(["x"].into_iter(), stmt),
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:117 on 2024-05-01 08:15_

I think we should do this as a separate PR or we'll get flooded by ecosystem changes, making it harder to spot real changes.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:123 on 2024-05-01 08:18_

Nit: Could be a follow up PR or a preparation PR (or you decide not to do it at all) 

I think it would be nice if `categorize` would either

* accept `&Checker` as an argument and extract the relevant parts itself
* accept a `IsortContext` that stores all the data. `IsortContext` could then offer a `::from_checker` method that removes a lot of the complexity here

---

_@MichaReiser reviewed on 2024-05-01 08:22_

---

_@zanieb reviewed on 2024-05-01 15:59_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-05-01 15:59_

I think we should use the user-settings for some parts, but why should changing your isort section order or default section affect this rule?

---

_@charliermarsh reviewed on 2024-05-01 16:20_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-05-01 16:20_

I think including the default section _does_ make sense. From the perspective of this rule, these are opaque settings that the user provides to tell us how imports should be categorized. This rule shouldn't need to keep track of which settings _might_ matter based on details of its own logic. Imagine instead that this method just takes `settings.isort`. Re-creating that object to omit some values would be breaking the API contract. (And I don't really see any benefit -- it's actually _more_ work in the future, if we add arguments, to try and keep track of which ones should be provided and which should be intentionally omitted.)

---

_@charliermarsh reviewed on 2024-05-01 16:21_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:117 on 2024-05-01 16:21_

(Though this only shows up in the fix message?)

---

_@zanieb reviewed on 2024-05-01 16:37_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-05-01 16:37_

I mean.. the method doesn't take `settings.isort` but okay. It sounds like what I want is a different categorization method that doesn't rely on `isort` sections to determine if something is first-party or not — it's a weird mix of concerns. Perhaps I'm just misunderstanding the intent of these settings but, as a user, I would be very surprised that import-sorting options affect fixes for unused imports.

---

_@zanieb reviewed on 2024-05-01 16:37_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-05-01 16:37_

It sounds like we should leave this as-is in this pull request though.

---

_@charliermarsh reviewed on 2024-05-01 16:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:133 on 2024-05-01 16:40_

Yes, please leave it as-is.

---

_@plredmond reviewed on 2024-05-01 18:43_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:132 on 2024-05-01 18:43_

How do I determine the correct level? I'll look at existing examples, but I'm not 100% clear on the meaning of this argument. I saw carl's PR related to it though, so I'll look around.

---

_@plredmond reviewed on 2024-05-01 18:46_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:208 on 2024-05-01 18:46_

A lot of the design for how I implemented the current PR is predicated on doing the `__all__` stuff next. The only reason I didn't do it in this PR was because of @zanieb's instruction. I'm getting a bit of whiplash here.

---

_@plredmond reviewed on 2024-05-01 18:52_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:117 on 2024-05-01 18:52_

Sounds like no change is needed here.

---

_@plredmond reviewed on 2024-05-01 18:53_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:128 on 2024-05-01 18:53_

Willfix

---

_@plredmond reviewed on 2024-05-01 18:53_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:129 on 2024-05-01 18:53_

Willfix

---

_@plredmond reviewed on 2024-05-01 18:57_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:123 on 2024-05-01 18:57_

This seems out of context for this PR, but I agree that `categorize` probably shouldn't have all these domain-specific arguments exposed.

---

_@plredmond reviewed on 2024-05-01 19:09_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:128 on 2024-05-01 19:09_

I'm not actually sure what your suggestion is here, probably because I don't know how to get from what I have in the caller (a `NodeId`) to an `AnyImport`. I was following existing code nearby which called `Semantic::statement` to convert `NodeId` to `Stmt`. What is the benefit of using `AnyImport`?

---

_@plredmond reviewed on 2024-05-01 19:11_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:127 on 2024-05-01 19:11_

Yep, again, I was following the example set by `fix::edit::remove_unused_imports`, which is the only other `Edit`-producing function used by this rule. I assume we want code to be consistent. Happy to try out your suggestion, though. Thanks.

---

_@charliermarsh reviewed on 2024-05-01 19:16_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/fix/edits.rs`:128 on 2024-05-01 19:16_

(Ah yeah, I don't think `AnyImport` does what you want. `AnyImport` is for a binding, but you're operating on an entire statement here. You actually have an `AnyImport` already via `let Some(import) = binding.as_any_import()`.)

---

_@charliermarsh reviewed on 2024-05-01 19:17_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:224 on 2024-05-01 19:17_

Just trying to understand what's happening in Latch, at least one thing is that I think `isort::categorize` expects `.module_name()` rather than `.qualified_name()`, so this should probably be `binding.import.module_name()` (although I wouldn't expect it to change the outcome -- the module name is generally a prefix of the qualified name, and the import categorization just operates on the first segment).

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:224 on 2024-05-01 19:20_

Oh interesting, but `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs` uses the `.qualified_name` with `level=0`. Interesting... I mean, on further review, it's possible that `level=0` _should_ work here, because the binding name is meant to be the fully-resolved name.

Like, if the user does this from a module named `foo`:

```python
from . import bar
```

Then the qualified name on the binding should be `["foo", "bar"]` -- and so we should be able to resolve it without taking into account the dots. I'm surprised it's not working for those projects. It's probably worth checking them out, running them locally, and looking at what `binding.import.qualified_name` is here.


---

_@charliermarsh reviewed on 2024-05-01 19:20_

---

_@charliermarsh reviewed on 2024-05-01 19:21_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:132 on 2024-05-01 19:21_

The correct level needs to come from the import statement, rather than _binding_. We don't store it on the binding at all, but it's a field on `Stmt::ImportFrom`. You could look it up using `import_statement` below, by grabbing the `Stmt`?

---

_@plredmond reviewed on 2024-05-01 20:42_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:127 on 2024-05-01 20:42_

I tried this a couple different ways and I don't think it ended up simplifying things. I don't know the rust patterns here, but it seems like taking a slice ends up requiring a lot more collections because you can't map down to subfields.

---

_@plredmond reviewed on 2024-05-01 20:51_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:128 on 2024-05-01 20:51_

I just saw that in the caller I have `ImportBinding.import : AnyImport`.

And yes, this code is producing an edit for a statement that incises one or more bindings, so the comment isn't applicable. 

---

_@plredmond reviewed on 2024-05-01 21:02_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:132 on 2024-05-01 21:02_

Would it be appropriate to add a `level` method to `impl Imported AnyImport` for this, or better to just pattern match on the `Stmt`?

---

_Comment by @plredmond on 2024-05-01 21:24_

Done with this round of review. I think the outstanding issues (in order of importance) are:

(1) Incorrectly categorizing many imports as not first-party in ecosystem results.

  * Latest change might address this by setting level correctly

<details><summary>(2) Dangling `__all__` code</summary><ol><li>`let dunder_all = None;`<li>`dunder_all: bool` field of `UnusedImportContext::init`<li>`dunder_all: Option<NodeId>` argument of `fix_by_reexporting` <li> `match dunder_all` in `fix_by_reexporting` which has a "not implemented" branch <li> branch in `fix_title` of `impl Violation for UnusedImport` which handles the case for no2</ol></details>

  * To complete this feature requires adding a new edit function and calling it at no4.

<details><summary>(3) Some functions still take `impl Iterator<...>` but might be better to take `&'a [...]`</summary><ol><li>`fix::edits::make_redundant_alias` <li> `fix_by_reexporting` <li> `fix_by_removing_imports`</ol></details>


---

_Comment by @plredmond on 2024-05-01 21:37_

`latch/latchbio` is still getting many imports incorrectly categorized, it seems

---

_Comment by @plredmond on 2024-05-01 21:39_

I think there might just be a bug in my logic somewhere, since it seems that the only things being miscategorized are bindings in the syntax
```
from foo.bar.baz import (bind1,bind2)
```
So I'll write a test for that syntax and see what happens with it.

---

_Comment by @charliermarsh on 2024-05-01 22:10_

Cool, I can help take a look at the remaining Latch categorizations tomorrow if it’s helpful!

---

_Comment by @charliermarsh on 2024-05-02 16:28_

On second glance, aren't the ecosystem changes _correct_? We're now offering fixes for first-party imports in `__init__.py` files, whereas we weren't before?

---

_@charliermarsh reviewed on 2024-05-02 16:32_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:132 on 2024-05-02 16:32_

On review, I think what you have here actually _is_ fine, because we're using `qualified_name`... Imagine you have `from . import bar` in a module named `foo`. `qualified_name` will get resolved to `foo.bar`, so it should correctly be marked as first-party.

(In the `isort` rules, we don't have `qualified_name`, but rather, an import with a level (the dot) and member (`bar`), which is why we need to pass the level in. Also, the `isort` rules care about differentiating between relative and absolute imports -- they get rendered as separate sections -- but that doesn't matter here.)

---

_@charliermarsh approved on 2024-05-02 16:36_

The behavior here looks correct to me. Would you mind adding a brief summary of the _behavior_ changes to the PR body, so that users that click through in the changelog can understand what changed, rather than being limited to the implementation details?

---

_@charliermarsh approved on 2024-05-02 16:37_

The behavior here looks correct to me. Would you mind adding a brief summary of the _behavior_ changes to the PR body, so that users that click through in the changelog can understand what changed, rather than being limited to the implementation details?

---

_@charliermarsh reviewed on 2024-05-02 16:54_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:208 on 2024-05-02 16:54_

Here's the entirety of what's being requested here:

```diff
diff --git a/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs b/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs
index 3a3302e8c..6dda41f8e 100644
--- a/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs
+++ b/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs
@@ -18,7 +18,7 @@ use crate::rules::{isort, isort::ImportSection, isort::ImportType};
 #[derive(Debug, Copy, Clone, Eq, PartialEq)]
 enum UnusedImportContext {
     ExceptHandler,
-    Init { first_party: bool, dunder_all: bool },
+    Init { first_party: bool },
 }
 
 /// ## What it does
@@ -108,14 +108,7 @@ impl Violation for UnusedImport {
     fn fix_title(&self) -> Option<String> {
         let UnusedImport { name, multiple, .. } = self;
         let resolution = match self.context {
-            Some(UnusedImportContext::Init {
-                first_party: true,
-                dunder_all: true,
-            }) => "Add unused import to __all__",
-            Some(UnusedImportContext::Init {
-                first_party: true,
-                dunder_all: false,
-            }) => "Use a redundant alias",
+            Some(UnusedImportContext::Init { first_party: true }) => "Use a redundant alias",
             _ => "Remove unused import",
         };
         Some(if *multiple {
@@ -147,7 +140,7 @@ fn is_first_party(qualified_name: &str, level: u32, checker: &Checker) -> bool {
 
 /// For some unused binding in an import statement...
 ///
-///  __init__.py ∧ 1stpty → safe,   move to __all__ or convert to explicit-import
+///  __init__.py ∧ 1stpty → safe,   convert to explicit-import
 ///  __init__.py ∧ stdlib → unsafe, remove
 ///  __init__.py ∧ 3rdpty → unsafe, remove
 ///
@@ -204,8 +197,6 @@ pub(crate) fn unused_import(checker: &Checker, scope: &Scope, diagnostics: &mut
 
     let in_init = checker.path().ends_with("__init__.py");
     let fix_init = checker.settings.preview.is_enabled();
-    // TODO: find the `__all__` node
-    let dunder_all = None;
 
     // Generate a diagnostic for every import, but share fixes across all imports within the same
     // statement (excluding those that are ignored).
@@ -234,7 +225,6 @@ pub(crate) fn unused_import(checker: &Checker, scope: &Scope, diagnostics: &mut
                             level,
                             checker,
                         ),
-                        dunder_all: dunder_all.is_some(),
                     })
                 } else {
                     None
@@ -265,7 +255,6 @@ pub(crate) fn unused_import(checker: &Checker, scope: &Scope, diagnostics: &mut
                     checker,
                     import_statement,
                     to_reexport.iter().map(|(binding, _)| binding),
-                    dunder_all,
                 )
                 .ok(),
             )
@@ -378,13 +367,12 @@ fn fix_by_removing_imports<'a>(
     )
 }
 
-/// Generate a [`Fix`] to make bindings in a statement explicit, either by adding them to `__all__`
-/// or changing them from `import a` to `import a as a`.
+/// Generate a [`Fix`] to make bindings in a statement explicit by adding a redundant alias (e.g.,
+/// converting `import a` to `import a as a`).
 fn fix_by_reexporting<'a>(
     checker: &Checker,
     node_id: NodeId,
     imports: impl Iterator<Item = &'a ImportBinding<'a>>,
-    dunder_all: Option<NodeId>,
 ) -> Result<Fix> {
     let statement = checker.semantic().statement(node_id);
 
@@ -395,10 +383,7 @@ fn fix_by_reexporting<'a>(
         bail!("Expected import bindings");
     }
 
-    let edits = match dunder_all {
-        Some(_dunder_all) => bail!("Not implemented: add to dunder_all"),
-        None => fix::edits::make_redundant_alias(member_names.iter().map(AsRef::as_ref), statement),
-    };
+    let edits = fix::edits::make_redundant_alias(member_names.iter().map(AsRef::as_ref), statement);
 
     // Only emit a fix if there are edits
     let mut tail = edits.into_iter();
```

I leave it to you as to whether you apply the patch.

---

_@plredmond reviewed on 2024-05-02 17:13_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:132 on 2024-05-02 17:13_

No longer hardcodes a zero argument in all cases. In the `FromImport` case we pass the `level` field in.

---

_@plredmond reviewed on 2024-05-02 17:32_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:208 on 2024-05-02 17:32_

Applied.

---

_Merged by @plredmond on 2024-05-02 23:10_

---

_Closed by @plredmond on 2024-05-02 23:10_

---

_Branch deleted on 2024-05-02 23:10_

---

_Comment by @charliermarsh on 2024-05-02 23:11_

Congrats on the merge :)

---

_Label `rule` added by @dhruvmanila on 2024-05-03 05:15_

---
