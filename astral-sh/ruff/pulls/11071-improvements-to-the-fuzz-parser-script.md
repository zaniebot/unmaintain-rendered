```yaml
number: 11071
title: "Improvements to the `fuzz-parser` script"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: fuzzing-script-improvements
created_at: 2024-04-21T14:47:00Z
updated_at: 2024-04-22T06:47:05Z
url: https://github.com/astral-sh/ruff/pull/11071
synced_at: 2026-01-10T22:37:01Z
```

# Improvements to the `fuzz-parser` script

---

_Pull request opened by @AlexWaygood on 2024-04-21 14:47_

## Summary

- Properly fix the race condition identified in https://github.com/astral-sh/ruff/pull/11039. Instead of running the version of Ruff we're testing by invoking `cargo run --release` on each generated source file, we either (1) accept a path to an executable on the command line or (2) if that's not specified, we run `cargo build --release` once at the start and then invoke the executable found in `target/release/ruff` directly.
- Now that the race condition is properly fixed, remove the workaround for the race condition added in https://github.com/astral-sh/ruff/pull/11039.
- Also allow users to pass in an executable to compare against for the `--only-new-bugs` argument (previously it was hardcoded to always compare against the version of Ruff installed into the Python environment)
- Use `argparse.RawDescriptionHelpFormatter` as the formatter class rather than `argparse.RawTextHelpFormatter`. This means that long help texts for the individual arguments will be wrapped to a sensible width.
- On completion of the script, indicate success or failure of the script overall by raising `SytemExit` with the appropriate exit code.
- Add myself as a codeowner for the script

## Test Plan

After activating a Python virtual environment with the requirements for the script installed into it, I ran the following invocations locally to check that the script still worked as expected:
- `py scripts/fuzz-parser/fuzz.py 0-5_000`
- `py scripts/fuzz-parser/fuzz.py -h`
- `py scripts/fuzz-parser/fuzz.py 0-10 --baseline-executable ruff` (should error gracefully)
- `py scripts/fuzz-parser/fuzz.py 0-10 --only-new-bugs --baseline-executable foo` (should error gracefully)
- `py scripts/fuzz-parser/fuzz.py 0-10 --only-new-bugs --baseline-executable ruff`
- `py scripts/fuzz-parser/fuzz.py 0-10 --test-executable target/release/ruff`
- `py scripts/fuzz-parser/fuzz.py 0-10 --test-executable target/release/ruff --only-new-bugs --baseline-executable ruff`

---

_Label `internal` added by @MichaReiser on 2024-04-22 06:35_

---

_@MichaReiser approved on 2024-04-22 06:36_

---

_Merged by @AlexWaygood on 2024-04-22 06:46_

---

_Closed by @AlexWaygood on 2024-04-22 06:46_

---

_Branch deleted on 2024-04-22 06:47_

---
