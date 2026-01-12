```yaml
number: 19569
title: "[`ruff`] Implement `global-or-nonlocal-in-branch` (`RUF103`)"
type: pull_request
state: closed
author: ya7on
labels:
  - rule
  - preview
assignees: []
base: main
head: feature/misleading-global-nonlocal
created_at: 2025-07-26T17:25:01Z
updated_at: 2025-08-18T12:25:17Z
url: https://github.com/astral-sh/ruff/pull/19569
synced_at: 2026-01-12T15:56:42Z
```

# [`ruff`] Implement `global-or-nonlocal-in-branch` (`RUF103`)

---

_@ya7on_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement the `global-or-nonlocal-in-branch` rule (`RUF103`), which detects cases where a variable
declared as `global` or `nonlocal` within a function is assigned in a separate branch of the function 
without an explicit declaration along that code path. Such usage can be confusing and is often unintended.

Example:

```python
g = 1

def f(b: bool) -> None:
    if b:
        global g
        g = 2
    else:
        g = 3

f(False)
print(g)  # 3, not 1
```

Closes #6470.

## Test Plan

Added unit tests covering:
- Assignments in the same branch as the declaration (no violation).
- Assignments in a different branch without declaration (violation).
- Cases with multiple branches and CFG paths.
- `nonlocal` usage (mirrors the global behavior).



---

_Label `rule` added by @ntBre on 2025-07-26 18:39_

---

_Label `preview` added by @ntBre on 2025-07-26 18:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:1055 on 2025-07-26 18:44_

I'm not quite sure, but I think the `RUF100` rules are a bit special. It looks like they're all related to noqa codes. I'd suggest pushing this up to `RUF066`, which is the next available code after `RUF065`, which is currently used in https://github.com/astral-sh/ruff/pull/19518.

---

_@ntBre reviewed on 2025-07-26 18:47_

Very cool! This is the first rule I've seen using the control flow graph, and it's closing quite a long-standing rule request too.

I'll leave the review to @dylwil3 since he wrote the CFG code, but I had one small suggestion about the rule code in passing.

---

_Review requested from @dylwil3 by @ntBre on 2025-07-26 18:48_

---

_Comment by @github-actions[bot] on 2025-07-26 18:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -0 violations, +0 -0 fixes in 8 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2c677a49eccb35d61bc0cf00605ada833de103a2/dev/breeze/src/airflow_breeze/global_constants.py#L669'>dev/breeze/src/airflow_breeze/global_constants.py:669:9:</a> RUF066 The variable PROVIDER_DEPENDENCIES is declared using global, but is used in other branches of this function — this may be unintuitive
+ <a href='https://github.com/apache/airflow/blob/2c677a49eccb35d61bc0cf00605ada833de103a2/dev/breeze/src/airflow_breeze/utils/publish_docs_to_s3.py#L326'>dev/breeze/src/airflow_breeze/utils/publish_docs_to_s3.py:326:17:</a> RUF066 The variable version_error is declared using global, but is used in other branches of this function — this may be unintuitive
+ <a href='https://github.com/apache/airflow/blob/2c677a49eccb35d61bc0cf00605ada833de103a2/scripts/ci/pre_commit/check_providers_subpackages_all_have_init.py#L72'>scripts/ci/pre_commit/check_providers_subpackages_all_have_init.py:72:13:</a> RUF066 The variable fail_pre_commit is declared using global, but is used in other branches of this function — this may be unintuitive
+ <a href='https://github.com/apache/airflow/blob/2c677a49eccb35d61bc0cf00605ada833de103a2/scripts/ci/pre_commit/check_providers_subpackages_all_have_init.py#L73'>scripts/ci/pre_commit/check_providers_subpackages_all_have_init.py:73:13:</a> RUF066 The variable fatal_error is declared using global, but is used in other branches of this function — this may be unintuitive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/audio.py#L59'>examples/server/app/spectrogram/audio.py:59:13:</a> RUF066 The variable _f_carrier is declared using global, but is used in other branches of this function — this may be unintuitive
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/audio.py#L60'>examples/server/app/spectrogram/audio.py:60:13:</a> RUF066 The variable _f_mod is declared using global, but is used in other branches of this function — this may be unintuitive
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/audio.py#L61'>examples/server/app/spectrogram/audio.py:61:13:</a> RUF066 The variable _ind_mod is declared using global, but is used in other branches of this function — this may be unintuitive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/efdfa00d1094fa63308e9455b37a200ed3a3d025/libs/core/tests/unit_tests/runnables/test_runnable_events_v2.py#L2444'>libs/core/tests/unit_tests/runnables/test_runnable_events_v2.py:2444:13:</a> RUF066 The variable outer_cancelled is declared using nonlocal, but is used in other branches of this function — this may be unintuitive
+ <a href='https://github.com/langchain-ai/langchain/blob/efdfa00d1094fa63308e9455b37a200ed3a3d025/libs/core/tests/unit_tests/runnables/test_runnable_events_v2.py#L2509'>libs/core/tests/unit_tests/runnables/test_runnable_events_v2.py:2509:13:</a> RUF066 The variable outer_cancelled is declared using nonlocal, but is used in other branches of this function — this may be unintuitive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/affe3b6d128d108210465445ee59fd90af7b6da7/src/latch/ldata/path.py#L333'>src/latch/ldata/path.py:333:13:</a> RUF066 The variable _download_idx is declared using global, but is used in other branches of this function — this may be unintuitive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/45080c39c59b5dbc8a5766d8afb9c08bba8d3896/_version_helper.py#L93'>_version_helper.py:93:9:</a> RUF066 The variable version is declared using global, but is used in other branches of this function — this may be unintuitive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/9d9ff9ec27f9bfff54e7dae0677997229704573f/reflex/utils/exec.py#L191'>reflex/utils/exec.py:191:13:</a> RUF066 The variable frontend_process is declared using global, but is used in other branches of this function — this may be unintuitive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/e590d39ffa04f751bf10e5b35864686cb7cd6bbb/src/trio/_core/_tests/test_asyncgen.py#L216'>src/trio/_core/_tests/test_asyncgen.py:216:17:</a> RUF066 The variable needs_retry is declared using nonlocal, but is used in other branches of this function — this may be unintuitive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/6a368438f29ea9b7996022168eaf9e3a675fb668/astropy/io/fits/util.py#L513'>astropy/io/fits/util.py:513:17:</a> RUF066 The variable CHUNKED_FROMFILE is declared using global, but is used in other branches of this function — this may be unintuitive
+ <a href='https://github.com/astropy/astropy/blob/6a368438f29ea9b7996022168eaf9e3a675fb668/astropy/io/fits/util.py#L515'>astropy/io/fits/util.py:515:17:</a> RUF066 The variable CHUNKED_FROMFILE is declared using global, but is used in other branches of this function — this may be unintuitive
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF066 | 15 | 15 | 0 | 0 | 0 |

</p>
</details>




---

_@ya7on reviewed on 2025-07-27 09:58_

---

_Review comment by @ya7on on `crates/ruff_linter/src/codes.rs`:1055 on 2025-07-27 09:58_

Thank you so much for reviewing my PR!  
This is my first contribution to this repository, so I wasn’t familiar with the rules for choosing a code number.  
I’ll update the rule to use RUF066.


---

_Comment by @dylwil3 on 2025-07-29 12:16_

Thanks @ya7on ! Have you gotten a chance to look through the ecosystem results? It seems like there are cases where we have a violation even when the assignment is in the same branch (unless I'm missing something). Let me know if the ecosystem results look okay to you and I can take another look.

Also, work on the control flow graph got paused at some point so it's very possible there are some bugs - let me know if you run into trouble or if something doesn't seem to be working right!

---

_Comment by @ya7on on 2025-07-29 12:38_

Thanks for the review!

I only tested the rule against the unit tests so far, didn't realize I should also check the ecosystem results. I'll run that and see what comes up. If I find issues, I'll fix them — and if it looks like a CFG bug, I'll come back with questions.

Appreciate you pointing this out!

---

_Comment by @dylwil3 on 2025-07-29 13:35_

Thanks! One thing, just in case it wasn't clear:

> I should also check the ecosystem results. I'll run that and see what comes up.

You don't have to run the ecosystem check yourself - you can click through the results in this comment https://github.com/astral-sh/ruff/pull/19569#issuecomment-3122475062

---

_Comment by @ya7on on 2025-07-29 13:45_

> you can click through the results in this comment https://github.com/astral-sh/ruff/pull/19569#issuecomment-3122475062

Oh, i see. Thank you! I'll check those results.

---

_Comment by @ya7on on 2025-07-31 16:32_

It looks like there is indeed a bug in the control-flow graph. I tried running `build_cfg` on a more nested function like this
```py
def test_func():
    a = True
    b = True
    if a:
        global GLOBAL_NAME
        GLOBAL_NAME = 1
        if b:
            return
```
The generated CFG contains only 2 blocks

Block 0 includes:
```py
a = True
b = True
if a:
```

But block 1 is completely empty. It doesnt include the `global` statement, the assignment and the inner `return`.

What would you recommend i do in this case?
I can switch to an AST-only approach for this rule instead of relying on CFG. The algorithm would work by walking through each branch (`if`, `while`, `for`, `try`) and checking whether a declaration and an assignment appear in different branches.

UPD: after a little debugging i find out following. `build_cfg` currently does not create separate blocks for statements inside `if` bodies.
Event control-altering statements like `return` or nested `if` are embedded inside a parent `StmtIf`, and not surfaced into the graph

```
ControlFlowGraph {
    blocks: [
        Block 0:
            stmts: [
                Assign a = True
                Assign b = True
                If a: [nested body with all other function code]
            ]
            outgoing: [BlockId(1)]
        Block 1:
            stmts: [] // Empty
    ]
}
```

---

_Comment by @dylwil3 on 2025-08-01 21:11_

Sorry this is my bad - I did not quite remember how much of the CFG work had been merged in. It looks like we never actually merged in the work on if and match statements https://github.com/astral-sh/ruff/pull/17268 (so not much functionality is available then...)

I don't have an immediate answer for when work is planned to continue on that. It's up to you whether you'd like to wait a bit, or whether you'd like to try using the available methods on the `SemanticModel` for handling branches. Happy to review either way!

---

_Comment by @MichaReiser on 2025-08-18 07:42_

@ya7on can you help me understand what the status of this PR is? Are you planning additional work or are you waiting on input a review from us?

---

_Comment by @ya7on on 2025-08-18 08:58_

@MichaReiser Hi! I am currently not working on this PR as i am waiting for updates or fixes to the CFG. I am happy to return to this once that work is unblocked, but in the meantime i think it makes sense to close this PR for now. Thanks again for the review!

---

_Comment by @dylwil3 on 2025-08-18 12:25_

Apologies again @ya7on - I do hope to return to the CFG work at some point!

---

_Closed by @dylwil3 on 2025-08-18 12:25_

---
