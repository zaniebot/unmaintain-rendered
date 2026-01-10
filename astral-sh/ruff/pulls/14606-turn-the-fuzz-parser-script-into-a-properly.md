```yaml
number: 14606
title: "Turn the `fuzz-parser` script into a properly packaged Python project"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: alex/fuzzer-package
created_at: 2024-11-26T11:44:37Z
updated_at: 2024-11-27T08:09:07Z
url: https://github.com/astral-sh/ruff/pull/14606
synced_at: 2026-01-10T20:50:57Z
```

# Turn the `fuzz-parser` script into a properly packaged Python project

---

_Pull request opened by @AlexWaygood on 2024-11-26 11:44_

## Summary

This PR gets rid of the `requirements.in` and `requirements.txt` files in the `scripts/fuzz-parser` directory, and replaces them with `pyproject.toml` and `uv.lock` files. The script is renamed from `fuzz-parser` to `py-fuzzer` (since it can now also be used to fuzz red-knot as well as the parser, following https://github.com/astral-sh/ruff/pull/14566), and moved from the `scripts/` directory to the `python/` directory, since it's now a (uv)-pip-installable project in its own right.

I've been resisting this for a while, because conceptually this script just doesn't feel "complicated" enough to me for it to be a full-blown package. However, I think it's time to do this. Making it a proper package has several advantages:
- It means we can run it from the project root using `uv run` without having to activate a virtual environment and ensure that all required dependencies are installed into that environment
- Using a `pyproject.toml` file means that we can express that the project requires Python 3.12+ to run properly; this wasn't possible before
- I've been running mypy on the project locally when I've been working on it or reviewing other people's PRs; now I can put the mypy config for the project in the `pyproject.toml` file

## Test Plan

I manually tested that all the commands detailed in `python/py-fuzzer/README.md` work for me locally.

---

_Label `testing` added by @AlexWaygood on 2024-11-26 11:44_

---

_Review requested from @zanieb by @AlexWaygood on 2024-11-26 11:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-26 11:44_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-11-26 11:44_

---

_Comment by @github-actions[bot] on 2024-11-26 11:53_

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

_Review comment by @sharkdp on `crates/ruff_python_parser/CONTRIBUTING.md`:65 on 2024-11-26 12:15_

```suggestion
uv run --no-project --with ./python/py-fuzzer fuzz
```

---

_@sharkdp reviewed on 2024-11-26 12:15_

---

_@sharkdp approved on 2024-11-26 12:20_

Thanks!

---

_@MichaReiser reviewed on 2024-11-26 12:40_

---

_Review comment by @MichaReiser on `python/py-fuzzer/pyproject.toml`:41 on 2024-11-26 12:40_

I like it :)

---

_@dhruvmanila approved on 2024-11-27 06:59_

---

_Label `internal` added by @dhruvmanila on 2024-11-27 06:59_

---

_Merged by @AlexWaygood on 2024-11-27 08:09_

---

_Closed by @AlexWaygood on 2024-11-27 08:09_

---

_Branch deleted on 2024-11-27 08:09_

---
