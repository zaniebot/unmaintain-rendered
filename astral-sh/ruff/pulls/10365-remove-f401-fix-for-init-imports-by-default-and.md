```yaml
number: 10365
title: "Remove `F401` fix for `__init__` imports by default and allow opt-in to unsafe fix"
type: pull_request
state: merged
author: zanieb
labels:
  - rule
assignees: []
merged: true
base: main
head: zb/F401-init
created_at: 2024-03-12T17:33:38Z
updated_at: 2024-04-19T21:11:07Z
url: https://github.com/astral-sh/ruff/pull/10365
synced_at: 2026-01-12T15:55:32Z
```

# Remove `F401` fix for `__init__` imports by default and allow opt-in to unsafe fix

---

_@zanieb_

Re-implementation of https://github.com/astral-sh/ruff/pull/5845 but instead of deprecating the option I toggle the default. Now users can _opt-in_ via the setting which will give them an unsafe fix to remove the import. Otherwise, we raise violations but do not offer a fix. The setting is a bit of a misnomer in either case, maybe we'll want to remove it still someday.

As discussed there, I think the safe fix should be to import it as an alias. I'm not sure. We need support for offering multiple fixes for ideal behavior though? I think we should remove the fix entirely and consider it separately.

Closes https://github.com/astral-sh/ruff/issues/5697
Supersedes https://github.com/astral-sh/ruff/pull/5845

---

_Label `rule` added by @zanieb on 2024-03-12 17:33_

---

_Renamed from "Deprecate F401 `ignore-init-module-imports` and make default behavior" to "Remove `F401` fix for `__init__` imports by default and allow opt-in to unsafe fix" by @zanieb on 2024-03-12 18:01_

---

_Comment by @github-actions[bot] on 2024-03-12 18:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+52 -52 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:21:</a> F401 [*] `.backup.ScheduledBackup` imported but unused
+ <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:21:</a> F401 `.backup.ScheduledBackup` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:38:</a> F401 [*] `.backup.ScheduledBackupRun` imported but unused
+ <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:38:</a> F401 `.backup.ScheduledBackupRun` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+50 -50 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L10'>latch/__init__.py:10:5:</a> F401 [*] `latch.functions.operators.group_tuple` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L10'>latch/__init__.py:10:5:</a> F401 `latch.functions.operators.group_tuple` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L11'>latch/__init__.py:11:5:</a> F401 [*] `latch.functions.operators.inner_join` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L11'>latch/__init__.py:11:5:</a> F401 `latch.functions.operators.inner_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L12'>latch/__init__.py:12:5:</a> F401 [*] `latch.functions.operators.latch_filter` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L12'>latch/__init__.py:12:5:</a> F401 `latch.functions.operators.latch_filter` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L13'>latch/__init__.py:13:5:</a> F401 [*] `latch.functions.operators.left_join` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L13'>latch/__init__.py:13:5:</a> F401 `latch.functions.operators.left_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L14'>latch/__init__.py:14:5:</a> F401 [*] `latch.functions.operators.outer_join` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L14'>latch/__init__.py:14:5:</a> F401 `latch.functions.operators.outer_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L15'>latch/__init__.py:15:5:</a> F401 [*] `latch.functions.operators.right_join` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L15'>latch/__init__.py:15:5:</a> F401 `latch.functions.operators.right_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L17'>latch/__init__.py:17:41:</a> F401 [*] `latch.resources.conditional.create_conditional_section` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L17'>latch/__init__.py:17:41:</a> F401 `latch.resources.conditional.create_conditional_section` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L18'>latch/__init__.py:18:39:</a> F401 [*] `latch.resources.map_tasks.map_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L18'>latch/__init__.py:18:39:</a> F401 `latch.resources.map_tasks.map_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L19'>latch/__init__.py:19:48:</a> F401 [*] `latch.resources.reference_workflow.workflow_reference` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L19'>latch/__init__.py:19:48:</a> F401 `latch.resources.reference_workflow.workflow_reference` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L21'>latch/__init__.py:21:5:</a> F401 [*] `latch.resources.tasks.custom_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L21'>latch/__init__.py:21:5:</a> F401 `latch.resources.tasks.custom_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L22'>latch/__init__.py:22:5:</a> F401 [*] `latch.resources.tasks.custom_memory_optimized_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L22'>latch/__init__.py:22:5:</a> F401 `latch.resources.tasks.custom_memory_optimized_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L23'>latch/__init__.py:23:5:</a> F401 [*] `latch.resources.tasks.large_gpu_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L23'>latch/__init__.py:23:5:</a> F401 `latch.resources.tasks.large_gpu_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L24'>latch/__init__.py:24:5:</a> F401 [*] `latch.resources.tasks.large_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L24'>latch/__init__.py:24:5:</a> F401 `latch.resources.tasks.large_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L25'>latch/__init__.py:25:5:</a> F401 [*] `latch.resources.tasks.medium_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L25'>latch/__init__.py:25:5:</a> F401 `latch.resources.tasks.medium_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L26'>latch/__init__.py:26:5:</a> F401 [*] `latch.resources.tasks.small_gpu_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L26'>latch/__init__.py:26:5:</a> F401 `latch.resources.tasks.small_gpu_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L27'>latch/__init__.py:27:5:</a> F401 [*] `latch.resources.tasks.small_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L27'>latch/__init__.py:27:5:</a> F401 `latch.resources.tasks.small_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L29'>latch/__init__.py:29:38:</a> F401 [*] `latch.resources.workflow.workflow` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L29'>latch/__init__.py:29:38:</a> F401 `latch.resources.workflow.workflow` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L7'>latch/__init__.py:7:38:</a> F401 [*] `latch.functions.messages.message` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L7'>latch/__init__.py:7:38:</a> F401 `latch.functions.messages.message` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L9'>latch/__init__.py:9:5:</a> F401 [*] `latch.functions.operators.combine` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L9'>latch/__init__.py:9:5:</a> F401 `latch.functions.operators.combine` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L10'>latch/types/__init__.py:10:5:</a> F401 [*] `latch.types.metadata.LatchMetadata` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L10'>latch/types/__init__.py:10:5:</a> F401 `latch.types.metadata.LatchMetadata` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L11'>latch/types/__init__.py:11:5:</a> F401 [*] `latch.types.metadata.LatchParameter` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L11'>latch/types/__init__.py:11:5:</a> F401 `latch.types.metadata.LatchParameter` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L12'>latch/types/__init__.py:12:5:</a> F401 [*] `latch.types.metadata.LatchRule` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L12'>latch/types/__init__.py:12:5:</a> F401 `latch.types.metadata.LatchRule` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L13'>latch/types/__init__.py:13:5:</a> F401 [*] `latch.types.metadata.Params` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L13'>latch/types/__init__.py:13:5:</a> F401 `latch.types.metadata.Params` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L14'>latch/types/__init__.py:14:5:</a> F401 [*] `latch.types.metadata.Section` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L14'>latch/types/__init__.py:14:5:</a> F401 `latch.types.metadata.Section` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
... 52 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F401 | 104 | 52 | 52 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+52 -52 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:21:</a> F401 [*] `.backup.ScheduledBackup` imported but unused
+ <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:21:</a> F401 `.backup.ScheduledBackup` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:38:</a> F401 [*] `.backup.ScheduledBackupRun` imported but unused
+ <a href='https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/models/__init__.py#L2'>housewatch/models/__init__.py:2:38:</a> F401 `.backup.ScheduledBackupRun` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+50 -50 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L10'>latch/__init__.py:10:5:</a> F401 [*] `latch.functions.operators.group_tuple` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L10'>latch/__init__.py:10:5:</a> F401 `latch.functions.operators.group_tuple` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L11'>latch/__init__.py:11:5:</a> F401 [*] `latch.functions.operators.inner_join` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L11'>latch/__init__.py:11:5:</a> F401 `latch.functions.operators.inner_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L12'>latch/__init__.py:12:5:</a> F401 [*] `latch.functions.operators.latch_filter` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L12'>latch/__init__.py:12:5:</a> F401 `latch.functions.operators.latch_filter` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L13'>latch/__init__.py:13:5:</a> F401 [*] `latch.functions.operators.left_join` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L13'>latch/__init__.py:13:5:</a> F401 `latch.functions.operators.left_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L14'>latch/__init__.py:14:5:</a> F401 [*] `latch.functions.operators.outer_join` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L14'>latch/__init__.py:14:5:</a> F401 `latch.functions.operators.outer_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L15'>latch/__init__.py:15:5:</a> F401 [*] `latch.functions.operators.right_join` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L15'>latch/__init__.py:15:5:</a> F401 `latch.functions.operators.right_join` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L17'>latch/__init__.py:17:41:</a> F401 [*] `latch.resources.conditional.create_conditional_section` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L17'>latch/__init__.py:17:41:</a> F401 `latch.resources.conditional.create_conditional_section` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L18'>latch/__init__.py:18:39:</a> F401 [*] `latch.resources.map_tasks.map_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L18'>latch/__init__.py:18:39:</a> F401 `latch.resources.map_tasks.map_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L19'>latch/__init__.py:19:48:</a> F401 [*] `latch.resources.reference_workflow.workflow_reference` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L19'>latch/__init__.py:19:48:</a> F401 `latch.resources.reference_workflow.workflow_reference` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L21'>latch/__init__.py:21:5:</a> F401 [*] `latch.resources.tasks.custom_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L21'>latch/__init__.py:21:5:</a> F401 `latch.resources.tasks.custom_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L22'>latch/__init__.py:22:5:</a> F401 [*] `latch.resources.tasks.custom_memory_optimized_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L22'>latch/__init__.py:22:5:</a> F401 `latch.resources.tasks.custom_memory_optimized_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L23'>latch/__init__.py:23:5:</a> F401 [*] `latch.resources.tasks.large_gpu_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L23'>latch/__init__.py:23:5:</a> F401 `latch.resources.tasks.large_gpu_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L24'>latch/__init__.py:24:5:</a> F401 [*] `latch.resources.tasks.large_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L24'>latch/__init__.py:24:5:</a> F401 `latch.resources.tasks.large_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L25'>latch/__init__.py:25:5:</a> F401 [*] `latch.resources.tasks.medium_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L25'>latch/__init__.py:25:5:</a> F401 `latch.resources.tasks.medium_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L26'>latch/__init__.py:26:5:</a> F401 [*] `latch.resources.tasks.small_gpu_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L26'>latch/__init__.py:26:5:</a> F401 `latch.resources.tasks.small_gpu_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L27'>latch/__init__.py:27:5:</a> F401 [*] `latch.resources.tasks.small_task` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L27'>latch/__init__.py:27:5:</a> F401 `latch.resources.tasks.small_task` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L29'>latch/__init__.py:29:38:</a> F401 [*] `latch.resources.workflow.workflow` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L29'>latch/__init__.py:29:38:</a> F401 `latch.resources.workflow.workflow` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L7'>latch/__init__.py:7:38:</a> F401 [*] `latch.functions.messages.message` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L7'>latch/__init__.py:7:38:</a> F401 `latch.functions.messages.message` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L9'>latch/__init__.py:9:5:</a> F401 [*] `latch.functions.operators.combine` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/__init__.py#L9'>latch/__init__.py:9:5:</a> F401 `latch.functions.operators.combine` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L10'>latch/types/__init__.py:10:5:</a> F401 [*] `latch.types.metadata.LatchMetadata` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L10'>latch/types/__init__.py:10:5:</a> F401 `latch.types.metadata.LatchMetadata` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L11'>latch/types/__init__.py:11:5:</a> F401 [*] `latch.types.metadata.LatchParameter` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L11'>latch/types/__init__.py:11:5:</a> F401 `latch.types.metadata.LatchParameter` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L12'>latch/types/__init__.py:12:5:</a> F401 [*] `latch.types.metadata.LatchRule` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L12'>latch/types/__init__.py:12:5:</a> F401 `latch.types.metadata.LatchRule` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L13'>latch/types/__init__.py:13:5:</a> F401 [*] `latch.types.metadata.Params` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L13'>latch/types/__init__.py:13:5:</a> F401 `latch.types.metadata.Params` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L14'>latch/types/__init__.py:14:5:</a> F401 [*] `latch.types.metadata.Section` imported but unused
+ <a href='https://github.com/latchbio/latch/blob/359790c04d3220681d4142a0820824840fdada8a/latch/types/__init__.py#L14'>latch/types/__init__.py:14:5:</a> F401 `latch.types.metadata.Section` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
... 52 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F401 | 104 | 52 | 52 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @zanieb on 2024-03-12 18:29_

---

_Review requested from @AlexWaygood by @zanieb on 2024-03-12 18:29_

---

_Comment by @charliermarsh on 2024-03-12 19:41_

What's the motivation for retaining the diagnostic but not offering the fix? I'm generally not a fan of that pattern -- it ends up being somewhat user-unfriendly.

---

_Comment by @zanieb on 2024-03-12 19:54_

@charliermarsh I think the fix is more than unsafe, I think it's wrong. I'd prefer to offer a different fix: inclusion in `__all__` if present in the file, otherwise a redundant alias. Until then, I'd like the fix to be off but am making use of the existing setting for people to recover the current behavior.

---

_Comment by @charliermarsh on 2024-03-12 19:58_

Should we be raising the diagnostic at all then?

---

_Comment by @zanieb on 2024-03-12 20:04_

@charliermarsh I think having a diagnostic without an automated fix is totally normal?

---

_Comment by @zanieb on 2024-03-12 20:05_

To be clear: I think in the future we should remove the setting entirely and always offer a safe fix in `__init__` files.

---

_Comment by @charliermarsh on 2024-03-12 20:10_

For context, this is a good example of what I want to avoid: https://github.com/astral-sh/ruff/pull/10238. This was really disruptive for users, we received multiple issues about it and changed it quickly: https://github.com/astral-sh/ruff/pull/10263.

---

_Comment by @charliermarsh on 2024-03-12 20:13_

That's obviously a different situation since we were introducing _more_ diagnostics, but it made me generally even more cautious of the idea of gating fixes from users.

In this case, I'm mostly just wondering if we should flag this at all, since we have no idea if the import is unused.

---

_Comment by @zanieb on 2024-03-12 20:29_

I think that's a fair point. I think there's a great deal of value to this diagnostic but honestly it seems like it should be under a different rule code. 

I think what I have here is still the best past forward for parity with the existing tool (i.e. https://github.com/PyCQA/pyflakes/issues/471). It moves us towards better default behavior while allowing users to fully recover the existing behavior. I think we have a reasonable path towards additional improvements, I'm just not doing them here because I'm trying to do a small amount of work to close old pull requests and issues.

---

_Comment by @AlexWaygood on 2024-03-13 15:43_

What if in `__init__.py` files, we did something similar to what [`autoflake`](https://pypi.org/project/autoflake/) does for unused imports? By default, `autoflake` only removes unused imports if they're from the standard library; you have to opt into more aggressive removals of imports.

I think it would be nice to carry on removing stdlib imports in particular, since they're really common, should be relatively easy to identify, and you're very rarely going to actually be trying to reexport a stdlib thing from some third-party package.

---

_Comment by @zanieb on 2024-03-13 16:11_

@AlexWaygood I'm all for that. Could we introduce it in a follow-up or do you think it's blocking?

---

_Review comment by @AlexWaygood on `crates/ruff/tests/snapshots/integration_test__rule_f401.snap`:39 on 2024-03-13 16:17_

```suggestion
When `ignore_init_module_imports` is disabled, fixes can remove unused imports in `__init__` files.
```

---

_@AlexWaygood reviewed on 2024-03-13 16:17_

---

_@AlexWaygood reviewed on 2024-03-13 16:19_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:98 on 2024-03-13 16:19_

I'd usually refer to this as an "explicit reexport" rather than "redundant alias", FWIW, but "redundant alias" is probably clearer for people unfamiliar with the idea

---

_@AlexWaygood reviewed on 2024-03-13 16:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:274 on 2024-03-13 16:20_

```suggestion
    // It's unsafe to remove things from `__init__.py` because it can break public interfaces
```

---

_@AlexWaygood approved on 2024-03-13 16:22_

> @AlexWaygood I'm all for that. Could we introduce it in a follow-up or do you think it's blocking?

I think it can be done as a followup (but let's open an issue to track it, since I agree with @charliermarsh that we should do our best to provide autofixes for errors whenever possible).

This LGTM; I agree removing imports automatically from `__init__.py` files seems too risky for it to be done as a safe fix. Left some minor comments above.

---

_@zanieb reviewed on 2024-03-13 17:31_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:98 on 2024-03-13 17:31_

"redundant alias" is the term used in Pyright which is usually my reference for this

---

_Comment by @charliermarsh on 2024-03-13 17:38_

To clarify: does that mean we wouldn't flag F401 for non-standard library imports in `__init__.py`, or we'd flag in either case but would only add the _fix_ for standard-library?

---

_Comment by @zanieb on 2024-03-13 17:40_

> does that mean we wouldn't flag F401 for non-standard library imports in __init__.py, or we'd flag in either case but would only add the fix for standard-library?

I think the ideal behavior is:

1. Always flag unused imports in `__init__.py`
2. Fix standard library imports by removing
3. Fix other imports by appending to `__all__` or using a redundant alias

We could consider not doing (1) for a subset until we have the fixes e.g. (3) but I would rather have the diagnostic without a fix than no diagnostic at all.

---

_Comment by @AlexWaygood on 2024-03-13 17:41_

> To clarify: does that mean we wouldn't flag F401 for non-standard library imports in `__init__.py`, or we'd flag in either case but would only add the _fix_ for standard-library?

I was suggesting the latter.

I think if you want to re-export in an `__init__.py` file, you should be forced to either use the "redundant alias" or add it to `__all__`. So I think it's correct for us to emit F401 errors on `__init__.py` files the same way as we would with any other files. But unlike with normal files, it's impossible for us to know whether the correct fix would be to remove the import or add it to `__all__`/mark it as explicit. So I think our default should be to only _fix_ F401 errors in `__init__.py` files if they're unused imports from the stdlib.

---

_Comment by @charliermarsh on 2024-03-13 17:42_

Okay, seems reasonable to me.

---

_Comment by @AlexWaygood on 2024-03-13 17:43_

> 3\. Fix other imports by appending to `__all__` or using a redundant alias

Unless we can reliably distinguish between local imports and third-party dependencies, I'd honestly probably just not fix non-stdlib imports at all in an `__init__.py` file

---

_Comment by @zanieb on 2024-03-13 17:58_

> Unless we can reliably distinguish between local imports and third-party dependencies,

I think we have reasonable support for this

---

_Merged by @zanieb on 2024-03-13 17:58_

---

_Closed by @zanieb on 2024-03-13 17:58_

---

_Branch deleted on 2024-03-13 17:58_

---

_Comment by @zanieb on 2024-03-13 17:58_

Tracking follow-ups in

- #10390
- #10391 

---

_Comment by @AlexWaygood on 2024-03-13 18:03_

> Tracking follow-ups in
> 
>     * [Add fix to remove unused standard library imports from `__init__.py` files #10390](https://github.com/astral-sh/ruff/issues/10390)
> 
>     * [Add fix to re-export unused first-party imports in `__init__.py` files #10391](https://github.com/astral-sh/ruff/issues/10391)

Thanks!

---

_Comment by @tylerlaprade on 2024-03-25 21:21_

FWIW, I've had both `F401` and `F403` disabled in all `__init__` files from the start. Does it make sense to ever even apply those rules to those files?

---

_Comment by @zanieb on 2024-03-25 22:12_

@tylerlaprade Yeah I think so. If you're a library author and you want to define an API, you should properly export types. If you don't, `F401` will warn you. The fix of "removing" the import is generally going to be wrong for internal types though, instead, we want to suggest export of the types (#10391) so downstream tooling can read your interface. See the [PyRight documentation](https://github.com/microsoft/pyright/blob/ada1ea63e8360fe3bd203bff3b0069c34d142817/docs/typed-libraries.md#library-interface) on library interfaces for more details on exporting types.

---

_Comment by @Hnasar on 2024-04-19 20:27_

@zanieb note that this is not just a pyright convention, but a Python typing convention in general:  
https://typing.readthedocs.io/en/latest/source/libraries.html#library-interface-public-and-private-symbols

Looking forward to the two _follow up_ improvements though! F401 is one of my faves 😄 

That said, after updating, we were surprised to see this fix wasn't being applied, so we reverted to the pre-3.3 behavior with:
```toml
[tool.ruff.lint]
# Allow F401 to be honored in __init__.py files
ignore-init-module-imports = false
# Allow applying these unsafe fixes without the `--unsafe-fixes` flag
extend-safe-fixes = ["F401"]
```





---

_Comment by @zanieb on 2024-04-19 21:11_

Thanks for the feedback!

Glad to see that's documented clearly elsewhere, pyright was just my goto resource for a while :)



---
