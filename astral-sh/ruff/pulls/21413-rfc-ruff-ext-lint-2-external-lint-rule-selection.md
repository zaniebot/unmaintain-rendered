```yaml
number: 21413
title: "RFC [ruff][ext-lint] 2: external lint rule selection"
type: pull_request
state: open
author: pieterh-oai
labels: []
assignees: []
base: main
head: pieterh/ext-lint-2-config
created_at: 2025-11-12T20:54:25Z
updated_at: 2026-01-07T01:47:14Z
url: https://github.com/astral-sh/ruff/pull/21413
synced_at: 2026-01-12T15:57:23Z
```

# RFC [ruff][ext-lint] 2: external lint rule selection

---

_@pieterh-oai_

## Summary

> [!Note]
> This PR 2 in  a stack of 3. This PR contains 2 commits; the base commit is PR'ed in  #21412 . The follow-on PR is #21415  . The `[ruff][ext-lint] external lint rule selection` commit is the actual delta for this PR.

This PR implements external lint rule selection in both config and from the CLI, supporting the usual `select`, `ignore`, `extend-select`, and `extend-ignore`.

To ensure that internal rules are selected and ignored exactly as before, the flags for external rules are separate (`select-external`, `ignore-external`, etc.). In addition to avoiding weird corner cases, having separate flags means the use of external linters is always visible/explicit.

Based on the configuration and flags, we trim an `ExternalLintRegistry` (introduced in #21412) to contain the correct rules for each configuration scope. The assumption is that building registries will be relatively infrequent, while quickly finding and executing the correct rules is common and more performance sensitive.

Based on how we’ve set up the config (in #21412), the available set of external lint rules is not constant across the workspace. Any non-root `ruff.toml` that we encounter could define new linters+rules that don’t exist in other parts of the same codebase. In addition, linter .toml files can flag a linter as enabled/disabled.

I’d say this is fairly gnarly, but I’m not sure there is a great way to better share code with the internal linter selection path. 

## Test Plan

```bash
cargo clippy --workspace --all-targets --all-features -- -D warnings
RUFF_UPDATE_SCHEMA=1 cargo test --workspace --exclude ty
uvx pre-commit run --all-files --show-diff-on-failure
```

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 21:04_


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

_Review requested from @amyreese by @amyreese on 2025-11-12 22:43_

---

_Review requested from @MichaReiser by @amyreese on 2025-11-12 22:43_

---

_Review requested from @ntBre by @amyreese on 2025-11-12 22:43_

---

_Comment by @MichaReiser on 2025-11-13 08:47_

Hmm, this is a bit painful to review because I can't change the baseline to your first PR. 

---

_Comment by @MichaReiser on 2025-11-13 08:56_

> To ensure that internal rules are selected and ignored exactly as before, the flags for external rules are separate (select-external, ignore-external, etc.). In addition to avoiding weird corner cases, having separate flags means the use of external linters is always visible/explicit.

Can you say more about the decision to use separate configuration options? The part that I'm worried about is that this wont' scale well. We'll need an `external` variant anywhere where we accept rule codes (fixable, unfixable, and possibly other places in the future)

---

_Comment by @pieterh-oai on 2025-11-13 17:11_

> Hmm, this is a bit painful to review because I can't change the baseline to your first PR.

Sorry about that. I'm used to stacking and I had no idea while setting this up that I'd have to base each PR off of `main`. I use this to look at these, fwiw:
<img width="424" height="332" alt="Screenshot 2025-11-13 at 8 59 31 AM" src="https://github.com/user-attachments/assets/73306381-9452-46b7-862c-14c3f5dcb992" />
<img width="485" height="239" alt="Screenshot 2025-11-13 at 8 59 54 AM" src="https://github.com/user-attachments/assets/0a0b7371-4e5a-4b41-bcad-05b6d62d4091" />

I'm not sure that fits into how you do reviews generally, but hopefully it's helpful.

> Can you say more about the decision to use separate configuration options?

My main concern, at least when I started out, was around overlapping rule names. Suppose someone writes RLI001 as an external linter somewhere in a private repo. Later, someone coincidentally writes a built-in rule with the same code. That sort of thing.

I think it depends a little on the 'policies' we want to set around external linters:
* Should it be possible to define new external linters in nested TOML config files?
* If a nested TOML file defines an external linter, should I be able to enable it / disable it from a 'parent' TOML?

On the plus side, I think most alternatives are probably simpler than this current implementation, so in some sense this PR represents the worst case scenario for external rule selection :)

---

_Comment by @pieterh-oai on 2025-11-19 20:04_

(rebase)

---

_Comment by @pieterh-oai on 2025-11-21 04:09_

(rebase)

---

_Comment by @astral-sh-bot[bot] on 2026-01-07 01:38_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-07 01:39_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5134 diagnostics
+ Found 5133 diagnostics


```

</details>


No memory usage changes detected ✅



---
