```yaml
number: 17739
title: "py-fuzzer: fix minimization logic when `--only-new-bugs` is passed"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - testing
assignees: []
merged: true
base: main
head: alex/fix-fuzzer
created_at: 2025-04-30T15:27:40Z
updated_at: 2025-04-30T17:48:33Z
url: https://github.com/astral-sh/ruff/pull/17739
synced_at: 2026-01-10T19:03:00Z
```

# py-fuzzer: fix minimization logic when `--only-new-bugs` is passed

---

_Pull request opened by @AlexWaygood on 2025-04-30 15:27_

## Summary

This PR fixes an issue in the py-fuzzer script described in https://github.com/astral-sh/ruff/pull/17702#issuecomment-2841720242. Namely, if run with `--only-new-bugs`, it's not sufficient for the script to just check that the bug exists in the new version of Ruff/red-knot when the script is minimizing the code that triggers the bug. The script must also check that the minimized version of the code _also_ continues to _not_ cause a panic on the baseline executable if `--only-new-bugs` is passed. Otherwise, the minimization has gone "too far". It's no longer a _new_ bug if it's been minimized to a snippet that causes a panic on both the test executable _and_ the baseline executable.

The first commit here just simplifies the way we run red-knot on the randomly generated code: the `--project` CLI argument is no longer necessary, and it's no longer necessary to write a `pyproject.toml` file to the temporary directory. The second commit here is the one that meaningfully fixes the bug.

## Test Plan

I checked that the script continues to pass with mypy using `uv run --directory ./python/py-fuzzer --dev mypy` from the root of my local Ruff clone.

I also did the following to test this change:
1. I checked out https://github.com/astral-sh/ruff/commit/4a621c2c12fb65fae343883ad17ce535411c5712 (the commit prior to https://github.com/astral-sh/ruff/commit/8a6787b39e178ff829d58d775b4d65d83b061c53)
2. I ran `cargo build -p red_knot --target-dir ./target/baseline` to create a baseline executable.
3. I checked out this branch again.
4. I ran the following command:

   ```
   uvx \                                           
   --python=3.13 \
   --force-reinstall \
   --from=./python/py-fuzzer \
   fuzz \
   --baseline-executable="./target/baseline/debug/red_knot" \
   --bin=red_knot \
   --only-new-bugs \
   50
   ```

   which produced the following output:

   ```
   Running `cargo build --release` since no test executable was specified...
   Sequentially running the fuzzer on 1 randomly generated source-code files...
   Ran fuzzer on seed 50                                         [1/1]
   The following code triggers a red-knot panic:

   class name_3[name_2: name_0]:
       pass
   name_3.name_4
   assert name_3[name_0]
   import name_0

   New bugs found in the following seeds:
   50
   ```

   I then confirmed that the script had correctly minimized the snippet to something that caused a panic on this branch but had _not_ caused a panic prior to https://github.com/astral-sh/ruff/commit/8a6787b39e178ff829d58d775b4d65d83b061c53, unlike the buggy behaviour that the script displays on `main`.   


---

_Label `bug` added by @AlexWaygood on 2025-04-30 15:27_

---

_Label `testing` added by @AlexWaygood on 2025-04-30 15:27_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-30 15:27_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-30 15:27_

---

_Comment by @github-actions[bot] on 2025-04-30 15:37_

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

_@carljm approved on 2025-04-30 17:47_

Thank you!

---

_Merged by @AlexWaygood on 2025-04-30 17:48_

---

_Closed by @AlexWaygood on 2025-04-30 17:48_

---

_Branch deleted on 2025-04-30 17:48_

---
