```yaml
number: 22470
title: Apply formatting to markdown code blocks
type: pull_request
state: open
author: amyreese
labels:
  - preview
assignees: []
base: main
head: amy/ruffen-docs
created_at: 2026-01-09T01:26:45Z
updated_at: 2026-01-21T09:58:55Z
url: https://github.com/astral-sh/ruff/pull/22470
synced_at: 2026-01-21T11:00:00Z
```

# Apply formatting to markdown code blocks

---

_@amyreese_

Adds initial support for formatting Python code blocks inside Markdown files.

- Adds `Markdown` source types/kinds
- Maps `.md` file extension to `Markdown` by default
- Uses simple regex adapted from blacken-docs to find and format fenced python code blocks
- Dedents contents before formatting, and reapplies indent from fenced <code>```py</code> header
- Selects `Python` vs `Stub` options based on language label on code block
- Silently skips formatting for any code block with syntax errors or that produce formatting errors.
- CLI tests formatting via both stdin and from filesystem
- Requires running with `--preview`, and otherwise warns user and skips formatting when given a markdown file
- Requires a user to `extend-include = ["**/*.md"]` if they want to format markdown files by default

Limitations:

- Raises `unimplemented!()` if run with a range of any sort
- Ignores unlabeled code blocks or implicit code blocks (no code fence)
- Doesn't yet support `~~~` fences, arbitrary fence lengths, or code blocks inside blockquotes

Issue #3792


---

_Comment by @astral-sh-bot[bot] on 2026-01-09 01:39_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Renamed from "Ruffen docs prototype" to "WIP: Ruffen docs prototype" by @amyreese on 2026-01-09 16:35_

---

_Comment by @amyreese on 2026-01-13 02:21_

minimal working prototype using regex adapted from blacken-docs:

`````
amethyst@lunatone ~/workspace/ruff amy/ruffen-docs » cat ~/scratch/test.md
Hello, this is a *markdown* document.

This is a rust code block:

```rust
fn main() {
    for x in 0..10 {
        println!("x = {x}");
    }
}
```

This is a poorly formatted python code block:

```py
def foo(arg1,
           arg2):
    print( "hello world")



foo(1 , 2)
```

This is another python code block, also poorly formatted, but now in a list:

1. List item 1
2. List item 2

    ```python
    dataset = [1, 2, 3,
        4, 5, 6]
    if 1+2==3:
        print('yes')
    ```

And here's an unlabeled code block that happens to have valid python code — what do we do?

```
print("hello")
```

amethyst@lunatone ~/workspace/ruff amy/ruffen-docs » cargo run -p ruff -- format --no-cache --diff ~/scratch/test.md
   Compiling ruff v0.14.11 (/Users/amethyst/workspace/ruff/crates/ruff)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 1.66s
     Running `target/debug/ruff format --no-cache --diff /Users/amethyst/scratch/test.md`
--- /Users/amethyst/scratch/test.md
+++ /Users/amethyst/scratch/test.md
@@ -13,13 +13,11 @@
 This is a poorly formatted python code block:

 ```py
-def foo(arg1,
-           arg2):
-    print( "hello world")
+def foo(arg1, arg2):
+    print("hello world")


-
-foo(1 , 2)
+foo(1, 2)
 ```

 This is another python code block, also poorly formatted, but now in a list:
@@ -28,10 +26,9 @@
 2. List item 2

     ```python
-    dataset = [1, 2, 3,
-        4, 5, 6]
-    if 1+2==3:
-        print('yes')
+    dataset = [1, 2, 3, 4, 5, 6]
+    if 1 + 2 == 3:
+        print("yes")
     ```

 And here's an unlabeled code block that happens to have valid python code — what do we do?

1 file would be reformatted
> [1]

`````

---

_Comment by @amyreese on 2026-01-13 02:25_

Some notes on the regex adapted from blacken-docs:

- it does not support `~~~` delimited code blocks
- it does not support code blocks delimited by more than three backticks/tildes
- it does verify that indentation and the end delimiter matches the starting delimiter

Prototype also silently discards any formatting error, and those should either be warnings or get tracked somehow to re-raise them outside of the closure.

---

_Comment by @amyreese on 2026-01-13 02:25_

Also need to decide on if/how to gate this behind `--preview`

---

_Review requested from @MichaReiser by @amyreese on 2026-01-20 21:23_

---

_Review requested from @ntBre by @amyreese on 2026-01-20 21:23_

---

_Renamed from "WIP: Ruffen docs prototype" to "Apply formatting to markdown code blocks" by @amyreese on 2026-01-20 21:23_

---

_Marked ready for review by @amyreese on 2026-01-20 22:57_

---

_Review requested from @carljm by @amyreese on 2026-01-20 22:57_

---

_Review requested from @AlexWaygood by @amyreese on 2026-01-20 22:57_

---

_Review requested from @sharkdp by @amyreese on 2026-01-20 22:57_

---

_Review requested from @dcreager by @amyreese on 2026-01-20 22:57_

---

_Review requested from @dhruvmanila by @amyreese on 2026-01-20 22:57_

---

_Comment by @amyreese on 2026-01-20 22:58_

Not familiar enough with ty to know why this changed what files ty looks at, or how to make ty ignore the md files when loading the project.

---

_Review requested from @Gankra by @amyreese on 2026-01-21 00:51_

---

_@amyreese reviewed on 2026-01-21 00:52_

---

_Review comment by @amyreese on `crates/ty_project/src/walk.rs`:237 on 2026-01-21 00:52_

This was claude's suggestion for resolving the issue when running ty on py-fuzzer. I have no idea if this is the best/correct place for this change.

---

_Comment by @astral-sh-bot[bot] on 2026-01-21 00:53_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-21 00:57_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `bool | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, str] | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:42:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_containers.py:55:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_containers.py:68:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_containers.py:81:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerHost | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:21:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
- src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:31:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
- src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:53:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
- src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:429:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:20:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:29:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:38:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:57:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:103:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:149:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:195:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:240:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:286:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:344:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:18:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:38:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:70:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:92:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:113:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:141:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:36:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:52:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:68:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:87:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:107:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:131:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:159:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:29:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:46:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:78:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:96:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:137:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:167:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
- src/prefect/task_engine.py:1721:47: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1734:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1780:48: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/task_engine.py:1792:29: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | dict[str, Any]]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | T@resolve_variables]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | T@resolve_variables | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | T@resolve_variables]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | T@resolve_variables | str | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5412 diagnostics
+ Found 5338 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4455 diagnostics
+ Found 4453 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @tvatter on 2026-01-21 06:49_

It would be great to also support quarto markdown (qmd) files, where executability of code chunks is triggered by the addition of brackets 3\`{} vs 3\`:

\`\`\`{python}
\# this is executable code
\`\`\`

\`\`\`python
\# this isn't 
\`\`\`

---

_Review request for @dcreager removed by @MichaReiser on 2026-01-21 09:12_

---

_Review request for @carljm removed by @MichaReiser on 2026-01-21 09:12_

---

_Review request for @sharkdp removed by @MichaReiser on 2026-01-21 09:12_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2026-01-21 09:12_

---

_Label `preview` added by @MichaReiser on 2026-01-21 09:12_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:541 on 2026-01-21 09:17_

It's okay to not support range formatting but aborting with `unimplemented` (which shows the user that ruff panicked), seems far from ideal. 

Let's handle it the same as for notebooks by making the returned error type more generic 

```rust
            if range.is_some() {
                return Err(FormatCommandError::RangeFormatNotebook(
                    path.map(Path::to_path_buf),
                ));
            }
```

and name the error `RangeFormatNotSupported`

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:506 on 2026-01-21 09:19_

It's fine to leave this to a follow-up, but should we try to format code blocks without a language specifier if they parse successfully?

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:523 on 2026-01-21 09:20_

I think I'd prefer aborting with an error, so that ruff sets the correct exit code

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:536 on 2026-01-21 09:21_

`ruff_python_trivia`'s dedent rules are Pythono specific. It doesn't strip all whitespace. Is this also correct for markdown?

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:549 on 2026-01-21 09:25_

See the `replace_all` documentation


```
    /// # Fallibility
    ///
    /// If you need to write a replacement routine where any individual
    /// replacement might "fail," doing so with this API isn't really feasible
    /// because there's no way to stop the search process if a replacement
    /// fails. Instead, if you need this functionality, you should consider
    /// implementing your own replacement routine:
    ///
    /// ```
    /// use regex::{Captures, Regex};
    ///
    /// fn replace_all<E>(
    ///     re: &Regex,
    ///     haystack: &str,
    ///     replacement: impl Fn(&Captures) -> Result<String, E>,
    /// ) -> Result<String, E> {
    ///     let mut new = String::with_capacity(haystack.len());
    ///     let mut last_match = 0;
    ///     for caps in re.captures_iter(haystack) {
    ///         let m = caps.get(0).unwrap();
    ///         new.push_str(&haystack[last_match..m.start()]);
    ///         new.push_str(&replacement(&caps)?);
    ///         last_match = m.end();
    ///     }
    ///     new.push_str(&haystack[last_match..]);
    ///     Ok(new)
    /// }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:554 on 2026-01-21 09:26_

It seems unfortunate that we keep returning `origional.to_string` (which makes `replace_all` to replace the text even in cases where the code is correctly formatted. 

Ideally, we'd skip doing any work unless the formatted code has changed. We can probably do so by implementing our own `replace_all`

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/format.rs`:2394 on 2026-01-21 09:28_

Let's add a helper to `CliTest` for resolving a file path that's relative to the crate root.

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/format.rs`:2415 on 2026-01-21 09:29_

Let's add this filter to the default `CliTest` filters

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/format.rs`:2429 on 2026-01-21 09:31_

I'd slightly prefer to inline the `unformatted.md` file text into the cli test as we do for other tests and make it much shorter. 

We should write a dedicated integration test to test various markdown formatting behaviors. 

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/format.rs`:2490 on 2026-01-21 09:32_

Same here. Using the full `unformatted.md` feels overkill for what we're testing here. A much shorter markdown snippet should be sufficient to demonstrate the same functionality and is much less verbose.

---

_Review comment by @MichaReiser on `crates/ruff/resources/test/fixtures/unformatted.md`:1 on 2026-01-21 09:43_

Let's add a test that `ruff check markdown.md` logs a warning that no python files were found


```
❯ cargo run --bin ruff -p ruff check README.md --no-cache -v
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check README.md --no-cache -v`
[2026-01-21][10:43:37][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/micha/astral/ruff/pyproject.toml
[2026-01-21][10:43:37][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/.gitignore_global
[2026-01-21][10:43:37][ruff::commands::check][DEBUG] Identified files to lint in: 5.983166ms
[2026-01-21][10:43:37][ruff::diagnostics][DEBUG] Checking: /Users/micha/astral/ruff/README.md
[2026-01-21][10:43:37][ruff::commands::check][DEBUG] Checked 1 files in: 1.230625ms
All checks passed!
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/lib.rs`:93 on 2026-01-21 09:54_

Hmm, adding `Markdown` to `PySourceType` feels wrong. It's not a different Python dialect. It also forces us to handle `Markdown` in many places where it's impossible that we'll ever see the `Markdown`: The `PySourceType` when formatting a markdown cell will always be `Python` or `Stub` but never `Markdown`, which is different from `Ipynb` where cells are formatted with `PySourceType::Ipynb`.

I think what you want is to add `Markdown` to `SourceType`, which is Ruff's enum over the file types it supports. We may have to introduce a new `FormatSourceType` enum that you pass to `format_path`, like so:

```rust
enum FormatSourceType {
	Python(PySourceType),
	Markdown
```


You may have to create `SourceKind` manually for the `Markdown` variant (you won't be able to delegate to `SourceKind::from(py_source_type)

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:499 on 2026-01-21 09:56_

I suggest moving the markdown core logic (given a document, iterate over the code blocks and format each of them) into its own function so that you can write unit tests against it. 



---

_Review comment by @MichaReiser on `crates/ruff/src/diagnostics.rs`:247 on 2026-01-21 09:58_

I think we need to exclude those files earlier or Ruff stops warning that it didn't find any files that it can operate on. We might need to parametrize `python_files_in_path` with a flag indicating whether markdown files are allowed.

---

_@MichaReiser reviewed on 2026-01-21 09:58_

Nice that you got something working so quickly. I think there's a bit more that we need to do to ensure the markdown formatting doesn't leak into other tools and we should also improve error handling, aborting isn't a great user experience.

---
