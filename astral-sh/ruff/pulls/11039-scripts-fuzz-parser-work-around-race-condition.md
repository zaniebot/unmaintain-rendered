```yaml
number: 11039
title: "`scripts/fuzz-parser`: work around race condition from running `cargo build` concurrently"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: workaround-race-condition
created_at: 2024-04-19T13:16:30Z
updated_at: 2024-04-19T13:42:33Z
url: https://github.com/astral-sh/ruff/pull/11039
synced_at: 2026-01-10T22:37:01Z
```

# `scripts/fuzz-parser`: work around race condition from running `cargo build` concurrently

---

_Pull request opened by @AlexWaygood on 2024-04-19 13:16_

## Summary

Running this script over a larger range of seeds, I noticed an occasional race condition due to the fact that the script runs `cargo run` in multiple processes concurrently.

The script was very occasionally reporting bugs where none actually existed. The `cargo run` subprocess would fail to run the checked-out `ruff` branch due to the race condition, so would exit with a nonzero exit code. The script then incorrectly assumed that that meant that ruff itself had failed to parse the generated code.

This PR works around the race condition by double-checking that the original snippet actually _does_ reproduce buggy behaviour if we get to a point where the original subprocess had a nonzero exit code but subsequent calls to `pysource_minimize.minimize()` then fail to reproduce the bug.

## Test Plan

In a Python virtual environment, I ran `uv pip install -r scripts/fuzz-parser/requirements.txt`, and then ran `py scripts/fuzz-parser/fuzz.py 0-10_000`. (Warning: this may make your computer fans whirr quite loudly.)


---

_Label `internal` added by @AlexWaygood on 2024-04-19 13:16_

---

_Renamed from "`scripts/fuzz-parser`: workaround race condition from running `cargo build` concurrently" to "`scripts/fuzz-parser`: work around race condition from running `cargo build` concurrently" by @AlexWaygood on 2024-04-19 13:16_

---

_@dhruvmanila approved on 2024-04-19 13:40_

Oh, I didn't realize it builds the release target. It doesn't need to be done in this PR or even now, but I think we should just take  path of two `ruff` executables via the CLI arguments similar to how `ruff-ecosystem` does. I don't think the script should be responsible to build `ruff`.

---

_Comment by @AlexWaygood on 2024-04-19 13:41_

> Oh, I didn't realize it builds the release target. It doesn't need to be done in this PR or even now, but I think we should just take path of two `ruff` executables via the CLI arguments similar to how `ruff-ecosystem` does. I don't think the script should be responsible to build `ruff`.

Oh, right. Yeah, that's probably a better way of doing things! Well, I'll merge this now, anyway, and look into how `ruff-ecosystem` does it later.

---

_Merged by @AlexWaygood on 2024-04-19 13:42_

---

_Closed by @AlexWaygood on 2024-04-19 13:42_

---

_Branch deleted on 2024-04-19 13:42_

---
