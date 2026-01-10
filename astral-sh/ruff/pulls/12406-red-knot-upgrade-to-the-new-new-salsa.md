```yaml
number: 12406
title: "[red-knot] Upgrade to the *new* *new* salsa"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: upgrade-to-new-new-salsa
created_at: 2024-07-19T13:47:42Z
updated_at: 2024-07-29T07:36:03Z
url: https://github.com/astral-sh/ruff/pull/12406
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Upgrade to the *new* *new* salsa

---

_Pull request opened by @MichaReiser on 2024-07-19 13:47_

This PR upgrades Salsa and refactors our code to the [*hassle free* salsa](https://salsa.zulipchat.com/#narrow/stream/333573-salsa-3.2E0/topic/branch.3A.20.22hassle-free.22.20salsa).

Most notable changes:

* Salsa macros no longer generate clippy warnings, :partying_face: 
* `Jar` is gone. Adding `[#salsa::tracked]` is enough
* Implementing any `Db` trait requires the `[#salsa::db]` macro
* `ParallelDatabase` is gone, use `Handle` instead
* `DebugWithDb` no longer exist. Instead use the normal `Debug` implementation. Outside a query, use `db.attach(|db| dbg!(ingredient))`.

## Performance regression

I invested quiet some time in figuring out where the regression comes from but I haven't been successful so far. See [this zulip thread](https://salsa.zulipchat.com/#narrow/stream/333573-salsa-3.2E0/topic/Beautiful.20Salsa.20perf.20incremental.20checking.20perf.20regression) for the entire conversation. I don't think we should block the PR based on the regression.

There are ideas on how to mitigate the perf regression. I don't want to block this PR by it.

---

_Comment by @codspeed-hq[bot] on 2024-07-20 09:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/upgrade-to-new-new-salsa)

### Merging #12406 will **degrade performances by 16.85%**

<sub>Comparing <code>upgrade-to-new-new-salsa</code> (a753af2) with <code>main</code> (9495331)</sub>



### Summary

`❌ 2` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/upgrade-to-new-new-salsa)._

### Benchmarks breakdown

|     | Benchmark | `main` | `upgrade-to-new-new-salsa` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `red_knot_check_file[incremental]` | 445.1 µs | 535.3 µs | -16.85% |
| ❌ | `red_knot_check_file[without_parse]` | 6.5 ms | 6.8 ms | -4.42% |


---

_Comment by @github-actions[bot] on 2024-07-20 10:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-07-25 14:30_

This is mostly done. The part that's missing is to find a way to assert whether a query did run. Salsa's new model no-longer exposes the query itself. We probably need to fallback to doing string comparisons, ugh

---

_Label `red-knot` added by @MichaReiser on 2024-07-25 14:35_

---

_Marked ready for review by @MichaReiser on 2024-07-26 08:38_

---

_Review requested from @carljm by @MichaReiser on 2024-07-26 08:38_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-26 08:38_

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:134 on 2024-07-26 16:06_

We do this just to save on drop costs since we are exiting anyway?

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:303 on 2024-07-26 16:07_

Did you mean to say "with info" here instead of "with warn"?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/db.rs`:112 on 2024-07-26 16:15_

So the new Salsa has a thread-local Db that's used for Debug, and `attach` sets the thread-local? Seems like that will have some cost, but ideally it's only used for debug code and can be avoided in the most perf sensitive cases.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:53 on 2024-07-26 16:17_

Snuck in some tracing improvements, too! Is this related to the Salsa change? Either way, I like it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2423 on 2024-07-26 16:18_

This ended up improving the usability of the API! Nice.

---

_@carljm approved on 2024-07-26 16:19_

This looks fantastic!

---

_@MichaReiser reviewed on 2024-07-26 17:21_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:134 on 2024-07-26 17:21_

yes

---

_@MichaReiser reviewed on 2024-07-26 17:22_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/db.rs`:112 on 2024-07-26 17:22_

Yes, it uses thread local but no, Salsa also uses thread local whenever it goes from external API -> internal. That's where the perf regression comes form (at least, one of them).

---

_@MichaReiser reviewed on 2024-07-26 17:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:53 on 2024-07-26 17:23_

It's because `debug` now prints the entire struct and not just the id. This was very verbose, so I had to truncate it.

---

_@MichaReiser reviewed on 2024-07-26 17:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2423 on 2024-07-26 17:23_

Yeah I think so. String matching is still a bit annoying but we at least have tests that tell us when the string matching stops working.

---

_Merged by @MichaReiser on 2024-07-29 07:21_

---

_Closed by @MichaReiser on 2024-07-29 07:21_

---

_Branch deleted on 2024-07-29 07:21_

---
