```yaml
number: 22582
title: "flake8-executable: allow global flags in uv shebangs (EXE003)"
type: pull_request
state: open
author: Jkhall81
labels: []
assignees: []
base: main
head: fix/exe003-uv-global-args-21753
created_at: 2026-01-14T19:36:02Z
updated_at: 2026-01-16T13:38:06Z
url: https://github.com/astral-sh/ruff/pull/22582
synced_at: 2026-01-16T13:57:10Z
```

# flake8-executable: allow global flags in uv shebangs (EXE003)

---

_@Jkhall81_

## Summary

This PR allows the use of global flags (e.g., `--quiet`, `--offline`) in `uv` shebangs for rule `EXE003`. Previously, flags placed between `uv` and `run` caused a false positive "missing python" error. The logic has been updated to independently check for the presence of `uv` and `run` within the shebang directive.

Fixes #21753

## Test Plan

- Added new test cases to `crates/ruff_linter/resources/test/fixtures/flake8_executable/EXE003_uv.py` covering `uv --quiet run` and `uv --offline run`.

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 19:46_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@MichaReiser reviewed on 2026-01-15 12:49_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_python.rs`:53 on 2026-01-15 12:49_

I'd prefer a stricter regex as mentioned in the original issue. We should also make the same change for `uvx` and `uv tool run`.

---

_@Jkhall81 reviewed on 2026-01-15 15:54_

---

_Review comment by @Jkhall81 on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_python.rs`:53 on 2026-01-15 15:54_

I used switched out contains() for a regex check.  It's 'stricter' than the contains() check but I didn't want to make it too strict.  I initially got caught up in trying to account for every possible flag a user might use.... that turned into a monster.  I chopped it down to this.  Added some tests and included uvx and uv tool run.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_python.rs`:12 on 2026-01-16 08:50_

`uvx` doesn't require `run`. So I don't thinkw e need to change anything for `uvx`.

For uv and `uv tool`, would a regex like this work `uv\s+(?:--?[a-zA-Z][\w-]*(?:[=\s]\S+)?\s+)*run(?:\s+.*)?`

---

_@MichaReiser reviewed on 2026-01-16 08:50_

---

_@Jkhall81 reviewed on 2026-01-16 13:32_

---

_Review comment by @Jkhall81 on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_python.rs`:12 on 2026-01-16 13:32_

My bad on the uvx run thing.  I took the run out of the test cases with uvx.  I tried your suggested regex.  When using uv tool, the regex didn't recognize tool, and run is still mandatory.

So, I updated the regex to use your flag logic the uv cases (requiring run) and uvx can stand alone.  I thought about just making run optional, but that would cause problems.

---

_Review comment by @Jkhall81 on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_python.rs`:12 on 2026-01-16 13:33_

Oh, and I left comments in the regex, as you can see.  I can take those out, if you think it looks cleaner without them.

---

_@Jkhall81 reviewed on 2026-01-16 13:33_

---
