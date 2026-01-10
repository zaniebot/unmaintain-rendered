```yaml
number: 17020
title: "Add hint for misplaced `--verbose`  in `uv tool run`"
type: pull_request
state: merged
author: F4RAN
labels:
  - error messages
assignees: []
merged: true
base: main
head: feature-16777-verbose-error-hint
created_at: 2025-12-07T23:37:29Z
updated_at: 2025-12-09T16:15:11Z
url: https://github.com/astral-sh/uv/pull/17020
synced_at: 2026-01-10T05:49:14Z
```

# Add hint for misplaced `--verbose`  in `uv tool run`

---

_Pull request opened by @F4RAN on 2025-12-07 23:37_

Resolves #16777

## Summary
When a command fails, users sometimes add --verbose after the package name (e.g., uvx foo --verbose) instead of before it (e.g., uvx --verbose foo). This adds a hint that suggests moving --verbose before the command.
The hint appears when a verbose flag is detected in the subcommand arguments and the command fails to resolve. It works for both uvx and uv tool run.

## Test Plan
Tested by running:
uvx foo-does-not-exist --verbose - shows the hint
uv tool run foo-does-not-exist --verbose - shows the hint
The hint only appears when verbose flags are detected, and the message shows the correct command format.

## Screenshot
<img width="920" height="34" alt="image" src="https://github.com/user-attachments/assets/f6c303f6-b5e6-441f-8d8d-9f5e6ab87c87" />

Open to feedback and happy to make changes as needed! 💯 

---

_Assigned to @EliteTK by @zanieb on 2025-12-08 12:40_

---

_Review comment by @EliteTK on `crates/uv/src/commands/tool/run.rs`:335 on 2025-12-08 13:54_

I think that the verbose hint shouldn't change how the rest of the error looks. So it would be good if it also called `.with_context("tool")`. But it looks like it's easier said than done as the relevant code doesn't support it at the moment: https://github.com/astral-sh/uv/blob/5a6f2ea319b0d7b6ffaf5b61e1de6c9060282c08/crates/uv/src/commands/diagnostics.rs#L77

It also seems like some of the other error cases drop the hint entirely.

---

_Comment by @EliteTK on 2025-12-08 14:00_

@F4RAN thanks for this.

Aside from the review comment, do you mind adding a test?

There are a few examples in [crates/uv/tests/it/tool_run.rs](https://github.com/astral-sh/uv/blob/main/crates/uv/tests/it/tool_run.rs).

---

_@zanieb reviewed on 2025-12-08 14:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:85 on 2025-12-08 14:13_

nit: I might use `let Some(...) = ... else { continue }` just to reduce nesting

---

_Review comment by @EliteTK on `crates/uv/src/commands/tool/run.rs`:84 on 2025-12-08 14:13_

Would be good to have a doc comment for this new function.

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:84 on 2025-12-08 14:13_

nit: I might write this using `args.iter().find(...)` instead?

---

_@zanieb reviewed on 2025-12-08 14:13_

---

_@EliteTK requested changes on 2025-12-08 14:15_

---

_Review comment by @F4RAN on `crates/uv/src/commands/tool/run.rs`:335 on 2025-12-08 18:37_

You're right - the diagnostic system doesn't support both yet. I'm following the same pattern as the `uvx run` hint. Happy to adjust if you prefer a different approach.

---

_@F4RAN reviewed on 2025-12-08 18:37_

---

_@F4RAN reviewed on 2025-12-08 18:38_

---

_Review comment by @F4RAN on `crates/uv/src/commands/tool/run.rs`:84 on 2025-12-08 18:38_

Refactored to use `find_map` as suggested

---

_@F4RAN reviewed on 2025-12-08 18:39_

---

_Review comment by @F4RAN on `crates/uv/src/commands/tool/run.rs`:84 on 2025-12-08 18:39_

Added doc comment for `find_verbose_flag` function

---

_@F4RAN reviewed on 2025-12-08 18:40_

---

_Review comment by @F4RAN on `crates/uv/src/commands/tool/run.rs`:85 on 2025-12-08 18:40_

Used `?` operator instead of `let...else` (clippy also suggested this)

---

_Comment by @F4RAN on 2025-12-08 18:41_

> @F4RAN thanks for this.
> 
> Aside from the review comment, do you mind adding a test?
> 
> There are a few examples in [crates/uv/tests/it/tool_run.rs](https://github.com/astral-sh/uv/blob/main/crates/uv/tests/it/tool_run.rs).

Hi @EliteTK and @zanieb. Thanks for reviewing my code.
1. Added tests for `--verbose`, `-v`, and `-vv` flags - all passing now!
2. Added doc comment for `find_verbose_flag` function
3. Refactored to use `find_map` as suggested
4. Used `?` operator instead of `let...else` (clippy also suggested this)

All feedback addressed! Happy to make any additional changes if needed.

---

_Review requested from @zanieb by @F4RAN on 2025-12-08 18:44_

---

_Review requested from @EliteTK by @F4RAN on 2025-12-08 18:44_

---

_Review comment by @EliteTK on `crates/uv/src/commands/tool/run.rs`:335 on 2025-12-09 09:36_

I think I will add an issue noting that this should be addressed, but I agree it's a bit beyond the scope of this PR.

---

_@EliteTK reviewed on 2025-12-09 09:36_

---

_Renamed from "Feature 16777 verbose error hint" to "Add hint for misplaced `--verbose`" by @konstin on 2025-12-09 09:45_

---

_Label `error messages` added by @konstin on 2025-12-09 09:45_

---

_@F4RAN reviewed on 2025-12-09 10:27_

---

_Review comment by @F4RAN on `crates/uv/src/commands/tool/run.rs`:335 on 2025-12-09 10:27_

Sounds good, thanks for clarifying!
Let me know if you’d like me to help with that follow-up issue in a separate PR.

---

_Comment by @EliteTK on 2025-12-09 15:14_

@F4RAN thanks for sorting those out. I've pushed a couple of commits to drop the filter as it's not something we normally do in the test suite and add a test case for the potential false positive case.

I've made a note to create an issue regarding the `OperationDiagnostic` problems once this is merged so I can reference the affected code.


---

_@EliteTK approved on 2025-12-09 15:15_

---

_@zanieb approved on 2025-12-09 16:14_

---

_Merged by @zanieb on 2025-12-09 16:15_

---

_Closed by @zanieb on 2025-12-09 16:15_

---

_Renamed from "Add hint for misplaced `--verbose`" to "Add hint for misplaced `--verbose`  in `uv tool run`" by @zanieb on 2025-12-09 16:15_

---
