```yaml
number: 20375
title: "[`ruff`] Add `analyze.string-imports-min-dots` to settings"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - documentation
assignees: []
merged: true
base: main
head: analyze-string-imports-min-dots
created_at: 2025-09-13T14:08:05Z
updated_at: 2025-09-16T11:30:40Z
url: https://github.com/astral-sh/ruff/pull/20375
synced_at: 2026-01-10T17:40:28Z
```

# [`ruff`] Add `analyze.string-imports-min-dots` to settings

---

_Pull request opened by @TaKO8Ki on 2025-09-13 14:08_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20110

`string_imports_min_dots` was introduced in #19538, so the option has been available since then; this PR adds documentation and a config test.

## Test Plan

<!-- How was it tested? -->

Add integration test `string_detection_from_config` to verify the config setting works.


---

_Comment by @github-actions[bot] on 2025-09-13 14:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `configuration` added by @MichaReiser on 2025-09-16 08:28_

---

_Review comment by @MichaReiser on `crates/ruff/tests/analyze_graph.rs`:259 on 2025-09-16 08:30_

I think this should be `min-dots` for consistency with the CLI

```
Options:
      --direction <DIRECTION>            The direction of the import map. By default, generates a dependency map, i.e., a map from file to files that it depends on. Use `--direction
                                         dependents` to generate a map from file to files that depend on it [default: dependencies] [possible values: dependencies, dependents]
      --detect-string-imports            Attempt to detect imports from string literals
      --min-dots <MIN_DOTS>              The minimum number of dots in a string import to consider it a valid import
      --preview                          Enable preview mode. Use `--no-preview` to disable
      --target-version <TARGET_VERSION>  The minimum Python version that should be supported [possible values: py37, py38, py39, py310, py311, py312, py313, py314]
      --python <PYTHON>                  Path to a virtual environment to use for resolving additional dependencies
  -h, --help
```

---

_@MichaReiser approved on 2025-09-16 08:32_

This looks good but I think we should rename the option to match the CLI

---

_@TaKO8Ki reviewed on 2025-09-16 10:49_

---

_Review comment by @TaKO8Ki on `crates/ruff/tests/analyze_graph.rs`:259 on 2025-09-16 10:49_

Ok. I have renamed it to `min-dots`!

---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-09-16 10:50_

---

_Renamed from "[`ruff`] Add `analyze.string-imports-min-dots` to settings docs" to "[`ruff`] Add `analyze.min-dots` to settings docs" by @MichaReiser on 2025-09-16 11:01_

---

_Renamed from "[`ruff`] Add `analyze.min-dots` to settings docs" to "[`ruff`] Add `analyze.min-dots` to settings" by @MichaReiser on 2025-09-16 11:02_

---

_Comment by @MichaReiser on 2025-09-16 11:03_

Oh wait, I made a mistake. I assumed you added the new option but that isn't the case. This is a pre-existing option (if I understand your summary correctly) and this PR only adds documentation. 

If that's the case, then we shouldn't rename the option because that would break existing users

---

_Renamed from "[`ruff`] Add `analyze.min-dots` to settings" to "[`ruff`] Add `analyze.string-imports-min-dots` to settings" by @MichaReiser on 2025-09-16 11:04_

---

_Comment by @TaKO8Ki on 2025-09-16 11:08_

@MichaReiser Thanks for the clarification. I had the same concern, so I didn’t rename the option; I’ll keep the existing name as is.

---

_Label `configuration` removed by @MichaReiser on 2025-09-16 11:19_

---

_Label `documentation` added by @MichaReiser on 2025-09-16 11:19_

---

_Merged by @MichaReiser on 2025-09-16 11:19_

---

_Closed by @MichaReiser on 2025-09-16 11:19_

---

_Branch deleted on 2025-09-16 11:30_

---
