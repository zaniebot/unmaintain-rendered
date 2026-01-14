```yaml
number: 21980
title: "fix: target-version fallback with extend"
type: pull_request
state: open
author: denyszhak
labels:
  - breaking
  - configuration
assignees: []
base: main
head: fix/target-version-extended
created_at: 2025-12-14T19:41:08Z
updated_at: 2026-01-14T15:44:07Z
url: https://github.com/astral-sh/ruff/pull/21980
synced_at: 2026-01-14T16:39:05Z
```

# fix: target-version fallback with extend

---

_@denyszhak_

## Summary

Closes #21956 

**Root cause:** When a .ruff.toml, ruff.toml config was discovered via ancestor search, Ruff eagerly derived target-version from requires-python in pyproject.toml during config loading. That fallback value then participated in config merging and incorrectly overrode an explicit target-version defined in an extended config.

**Fix:** Defer the requires-python fallback until after the full extend chain across .ruff.toml / ruff.toml is merged, and apply it only if target-version is still unset.

**Impact:** Explicit target-version settings in extended configs are now respected, requires-python is only used as a fallback.

## Test Plan

Added a new test. I was useful to validate the fix but may be redundant to keep, lmk if you would love me to remove it


---

_Comment by @astral-sh-bot[bot] on 2025-12-14 19:49_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+15 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/facebookresearch/chameleon">facebookresearch/chameleon</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/inference/chameleon.py#L648'>chameleon/inference/chameleon.py:648:19:</a> invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/miniviewer/miniviewer.py#L190'>chameleon/miniviewer/miniviewer.py:190:16:</a> invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_distributed.py#L405'>chameleon/viewer/backend/models/chameleon_distributed.py:405:13:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_distributed.py#L818'>chameleon/viewer/backend/models/chameleon_distributed.py:818:17:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_local.py#L633'>chameleon/viewer/backend/models/chameleon_local.py:633:13:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L131'>chameleon/viewer/backend/models/service.py:131:21:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L154'>chameleon/viewer/backend/models/service.py:154:17:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L226'>chameleon/viewer/backend/models/service.py:226:25:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/async_qdrant_fastembed.py#L226'>qdrant_client/async_qdrant_fastembed.py:226:16:</a> invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/qdrant_fastembed.py#L223'>qdrant_client/qdrant_fastembed.py:223:16:</a> invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

<pre>
+ examples/Context_summarization_with_realtime_api.ipynb:cell 15:7:16: invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ examples/Embedding_long_inputs.ipynb:cell 12:9:12: invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ examples/agents_sdk/session_memory.ipynb:cell 42:30:53: invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ examples/chatgpt/rag-quickstart/azure/Azure_AI_Search_with_Azure_Functions_and_GPT_Actions_in_ChatGPT.ipynb:cell 21:7:12: invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ examples/chatgpt/rag-quickstart/gcp/Getting_started_with_bigquery_vector_search_and_openai.ipynb:cell 18:7:12: invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| invalid-syntax: | 15 | 15 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/facebookresearch/chameleon">facebookresearch/chameleon</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/inference/chameleon.py#L648'>chameleon/inference/chameleon.py:648:19:</a> invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/miniviewer/miniviewer.py#L190'>chameleon/miniviewer/miniviewer.py:190:16:</a> invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_distributed.py#L405'>chameleon/viewer/backend/models/chameleon_distributed.py:405:13:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_distributed.py#L818'>chameleon/viewer/backend/models/chameleon_distributed.py:818:17:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_local.py#L633'>chameleon/viewer/backend/models/chameleon_local.py:633:13:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L131'>chameleon/viewer/backend/models/service.py:131:21:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L154'>chameleon/viewer/backend/models/service.py:154:17:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L226'>chameleon/viewer/backend/models/service.py:226:25:</a> invalid-syntax: Cannot use `match` statement on Python 3.7 (syntax was added in Python 3.10)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/async_qdrant_fastembed.py#L226'>qdrant_client/async_qdrant_fastembed.py:226:16:</a> invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/qdrant_fastembed.py#L223'>qdrant_client/qdrant_fastembed.py:223:16:</a> invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

<pre>
+ examples/Context_summarization_with_realtime_api.ipynb:cell 15:7:16: invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ examples/Embedding_long_inputs.ipynb:cell 12:9:12: invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ examples/agents_sdk/session_memory.ipynb:cell 42:30:53: invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ examples/chatgpt/rag-quickstart/azure/Azure_AI_Search_with_Azure_Functions_and_GPT_Actions_in_ChatGPT.ipynb:cell 21:7:12: invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
+ examples/chatgpt/rag-quickstart/gcp/Getting_started_with_bigquery_vector_search_and_openai.ipynb:cell 18:7:12: invalid-syntax: Cannot use named assignment expression (`:=`) on Python 3.7 (syntax was added in Python 3.8)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| invalid-syntax: | 15 | 15 | 0 | 0 | 0 |

</p>
</details>





---

_@denyszhak reviewed on 2025-12-14 20:16_

---

_Review comment by @denyszhak on `crates/ruff/tests/cli/snapshots/cli__lint__requires_python_extend_from_shared_config.snap`:79 on 2025-12-14 20:16_

Is 3.10 intended here?

---

_@denyszhak reviewed on 2025-12-14 20:17_

---

_Review comment by @denyszhak on `crates/ruff_workspace/src/resolver.rs`:1129 on 2025-12-14 20:17_

There is also `requires_python_extend_from_shared_config` in cli tests so we can remove that one if that feels redundant

---

_Label `breaking` added by @MichaReiser on 2025-12-15 07:02_

---

_Label `configuration` added by @MichaReiser on 2025-12-15 07:02_

---

_Comment by @MichaReiser on 2025-12-15 07:04_

Thanks for looking into this. 

Would you mind taking some time and update your summary with an explanation:

* What's the root cause for the bug observed in https://github.com/astral-sh/ruff/issues/21956
* What solution do you propose in this PR and why does it solve https://github.com/astral-sh/ruff/issues/21956
* Did you review the ecosystem changes? Do they look correct to you?

---

_Comment by @jbarnoud on 2025-12-20 17:58_

For what it is worth, I tried this MR (at commit e35fa57c85e70acb8bdb3ffed9adfa45a30d5656) with my reproducer example described in #21956 and it behaved as desired, using the `target-version` from the `ruff.toml`:

```
$ cargo run check --select UP040 --verbose ~/dev/ruff-repro
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.59s
     Running `target/debug/ruff check --select UP040 --verbose /home/jon/dev/ruff-repro`
[2025-12-20][14:15:46][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/jon/src/denyszhak-ruff/pyproject.toml
warning: Detected debug build without --no-cache.
[2025-12-20][14:15:46][ruff_workspace::pyproject][DEBUG] No `target-version` found in `/home/jon/dev/ruff-repro/.ruff.toml`
[2025-12-20][14:15:46][ruff_workspace::pyproject][DEBUG] No `target-version` found in `/home/jon/dev/ruff-repro/.ruff.toml`
[2025-12-20][14:15:46][ignore::gitignore][DEBUG] opened gitignore file: /home/jon/dev/ruff-repro/.ruff_cache/.gitignore
[2025-12-20][14:15:46][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/jon/dev/ruff-repro/main.py"
[2025-12-20][14:15:46][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/jon/dev/ruff-repro/pyproject.toml"
[2025-12-20][14:15:46][ignore::gitignore][DEBUG] opened gitignore file: /home/jon/dev/ruff-repro/.venv/.gitignore
[2025-12-20][14:15:46][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/jon/dev/ruff-repro/.venv"
[2025-12-20][14:15:46][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/jon/dev/ruff-repro/.ruff_cache"
[2025-12-20][14:15:46][ruff::commands::check][DEBUG] Identified files to lint in: 12.629661ms
[2025-12-20][14:15:46][ruff::diagnostics][DEBUG] Checking: /home/jon/dev/ruff-repro/pyproject.toml
[2025-12-20][14:15:46][ruff::diagnostics][DEBUG] Checking: /home/jon/dev/ruff-repro/main.py
[2025-12-20][14:15:46][ruff::commands::check][DEBUG] Checked 2 files in: 2.154425ms
All checks passed!
```

I also tried the following scenarios:
1. removing `target-version` from `ruff.toml` so that the ruff configuration does not defined the target version at all. Ruff successfully fell back to the `pyproject.toml`
2. adding `target-version` from `.ruff.toml`; that one is used, as expected, according to `--show-settings`
3. same as the original scenario but without the `requires-python` in the `pyproject.toml`, ruff uses the target version from the `.ruff.toml`, as expected
4. same as 1 (no `target-version` defined) but without the `requires-python` in the `pyproject.toml`, `target-version` is `none` as expected
5. same as 2 (`target-version` in both the `.ruff.toml` and the `ruff.toml`) but without the `requires-python` in the `pyproject.toml`, ruff uses the target version from the `.ruff.toml`, as expected

I only tested variations around my very minimal example, but they all behaved as expected. Thank you.

---

_Comment by @denyszhak on 2025-12-20 18:22_

> For what it is worth, I tried this MR (at commit [e35fa57](https://github.com/astral-sh/ruff/commit/e35fa57c85e70acb8bdb3ffed9adfa45a30d5656)) with my reproducer example described in #21956 and it behaved as desired, using the `target-version` from the `ruff.toml`:
> 
> ```
> $ cargo run check --select UP040 --verbose ~/dev/ruff-repro
>     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.59s
>      Running `target/debug/ruff check --select UP040 --verbose /home/jon/dev/ruff-repro`
> [2025-12-20][14:15:46][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/jon/src/denyszhak-ruff/pyproject.toml
> warning: Detected debug build without --no-cache.
> [2025-12-20][14:15:46][ruff_workspace::pyproject][DEBUG] No `target-version` found in `/home/jon/dev/ruff-repro/.ruff.toml`
> [2025-12-20][14:15:46][ruff_workspace::pyproject][DEBUG] No `target-version` found in `/home/jon/dev/ruff-repro/.ruff.toml`
> [2025-12-20][14:15:46][ignore::gitignore][DEBUG] opened gitignore file: /home/jon/dev/ruff-repro/.ruff_cache/.gitignore
> [2025-12-20][14:15:46][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/jon/dev/ruff-repro/main.py"
> [2025-12-20][14:15:46][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/jon/dev/ruff-repro/pyproject.toml"
> [2025-12-20][14:15:46][ignore::gitignore][DEBUG] opened gitignore file: /home/jon/dev/ruff-repro/.venv/.gitignore
> [2025-12-20][14:15:46][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/jon/dev/ruff-repro/.venv"
> [2025-12-20][14:15:46][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/jon/dev/ruff-repro/.ruff_cache"
> [2025-12-20][14:15:46][ruff::commands::check][DEBUG] Identified files to lint in: 12.629661ms
> [2025-12-20][14:15:46][ruff::diagnostics][DEBUG] Checking: /home/jon/dev/ruff-repro/pyproject.toml
> [2025-12-20][14:15:46][ruff::diagnostics][DEBUG] Checking: /home/jon/dev/ruff-repro/main.py
> [2025-12-20][14:15:46][ruff::commands::check][DEBUG] Checked 2 files in: 2.154425ms
> All checks passed!
> ```
> 
> I also tried the following scenarios:
> 
> 1. removing `target-version` from `ruff.toml` so that the ruff configuration does not defined the target version at all. Ruff successfully fell back to the `pyproject.toml`
> 2. adding `target-version` from `.ruff.toml`; that one is used, as expected, according to `--show-settings`
> 3. same as the original scenario but without the `requires-python` in the `pyproject.toml`, ruff uses the target version from the `.ruff.toml`, as expected
> 4. same as 1 (no `target-version` defined) but without the `requires-python` in the `pyproject.toml`, `target-version` is `none` as expected
> 5. same as 2 (`target-version` in both the `.ruff.toml` and the `ruff.toml`) but without the `requires-python` in the `pyproject.toml`, ruff uses the target version from the `.ruff.toml`, as expected
> 
> I only tested variations around my very minimal example, but they all behaved as expected. Thank you.

Damn, thank you for testing!

---

_Review requested from @dylwil3 by @MichaReiser on 2025-12-22 10:05_

---

_Review comment by @dylwil3 on `crates/ruff/tests/cli/snapshots/cli__lint__requires_python_extend_from_shared_config.snap`:79 on 2026-01-12 20:03_

I think you're correct that this should be 3.11 - must have slipped through the initial creation of this test, thanks!

---

_@dylwil3 requested changes on 2026-01-12 20:10_

This looks correct to me, and much nicer than the original incorrect implementation 😄 

The two things I'd love to see are:

1. A verification that the ecosystem changes are as expected (let me know if you need help with this)
2. Have you done any testing in VSCode to see whether we need changes to `ruff_server`? See some of the discussion later in the original implementation here https://github.com/astral-sh/ruff/pull/16319 IIRC there was some work to ensure that the `ruff_server` configuration resolution was compatible with the CLI.

---

_Comment by @denyszhak on 2026-01-13 23:47_

@dylwil3 

On the VSCode test, after you question I tested with next setup by comparing the released v0.14.8 with my local build that includes this patch.

Test setup:
- ruff.toml → target-version = "py310"
- .ruff.toml → select = ["UP040"]
- main.py → uses type alias (py312+ only)
- pyproject.toml → requires-python = ">=3.13"

Before (released v0.14.8): 
VSCode pointed to ruff@0.14.8
UP040 was still reported, ignoring the target-version that confirmed the bug was present in both the CLI and .ruff_server.

After (local build + fix): 
VSCode pointed to my local ~/.cargo/bin/ruff
UP040 no longer reported, as expected after the fix.

---

_Comment by @denyszhak on 2026-01-14 15:44_

@dylwil3 

I wasn't able to locate the exact config files in these repos (may they be using chains or the ecosystem tests may inject configs?). However, the violations are clearly legitimate - the code uses modern Python syntax (walrus operators, match statements) that's incompatible with a Python 3.7 target version somewhere in their config chain. The fix is correctly detecting these incompatibilities that were previously hidden.

facebookresearch/chameleon is archived though

---
