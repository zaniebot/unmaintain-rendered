```yaml
number: 15120
title: Update benchmark scripts, use uv
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/upgrade-benchmark-script
created_at: 2024-12-23T09:04:07Z
updated_at: 2024-12-23T10:14:17Z
url: https://github.com/astral-sh/ruff/pull/15120
synced_at: 2026-01-10T20:42:27Z
```

# Update benchmark scripts, use uv

---

_Pull request opened by @MichaReiser on 2024-12-23 09:04_

## Summary

Use uv instead of poetry, use `ruff check` vs `ruff`


Related https://github.com/astral-sh/ruff/issues/15116

## Test Plan

I ran all the benchmark scripts locally. I couldn't get pylint to work


---

_Review requested from @carljm by @MichaReiser on 2024-12-23 09:04_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-23 09:04_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-23 09:04_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-12-23 09:04_

---

_Label `internal` added by @MichaReiser on 2024-12-23 09:04_

---

_Review comment by @dhruvmanila on `CONTRIBUTING.md`:470 on 2024-12-23 09:08_

These two commands are the same but the text says to run the first and then the second (same) command.

---

_Review comment by @dhruvmanila on `scripts/.python-version`:1 on 2024-12-23 09:12_

What's the reason for this?

---

_Comment by @github-actions[bot] on 2024-12-23 09:13_

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

_Review comment by @dhruvmanila on `scripts/benchmarks/README.md`:10 on 2024-12-23 09:14_

The `uv sync ./scripts/benchmarks` doesn't work. Did you mean to use `--project` flag? Although, even after that it throws an error:
```console
$ uv sync --project ./scripts/benchmark
Using CPython 3.8.18 interpreter at: /Users/dhruv/.local/bin/python3.8
error: The Python request from `scripts/.python-version` resolved to Python 3.8.18, which is incompatible with the project's Python requirement: `>=3.11`. Use `uv python pin` to update the `.python-version` file to a compatible version.
```

Removing the `./scripts/.python-version` does make the command run but that creates the virtual environment in `./scripts` directory while the `uv venv --project ./scripts/benchmarks` command creates the environment in `./scripts/benchmarks` directory. 

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-12-23 09:16_

---

_Review comment by @dhruvmanila on `scripts/benchmarks/run_plugins.sh`:43 on 2024-12-23 09:19_

I think this should be `--extend-select RUF`

---

_@dhruvmanila reviewed on 2024-12-23 09:19_

---

_@MichaReiser reviewed on 2024-12-23 09:26_

---

_Review comment by @MichaReiser on `scripts/.python-version`:1 on 2024-12-23 09:26_

It's to match what we had in the `pyproject.toml` but I'm not sure if it even works. I'll removee

---

_@dhruvmanila approved on 2024-12-23 09:59_

Thank you

---

_Comment by @MichaReiser on 2024-12-23 10:14_

Thank you for your careful review and sorry for all my silly mistakes.

---

_Merged by @MichaReiser on 2024-12-23 10:14_

---

_Closed by @MichaReiser on 2024-12-23 10:14_

---

_Branch deleted on 2024-12-23 10:14_

---
