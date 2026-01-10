```yaml
number: 20550
title: "[ty] Make `FileResolver::path` return a full path"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: brent/fix-ty-gitlab-diagnostics
created_at: 2025-09-24T13:50:59Z
updated_at: 2025-09-24T17:16:53Z
url: https://github.com/astral-sh/ruff/pull/20550
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Make `FileResolver::path` return a full path

---

_Pull request opened by @ntBre on 2025-09-24 13:50_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1242

From finding references with the LSP, `FileResolver::path` is only called once, in `UnifiedFile::path`, so I went through those references, and it looked safe to make this change in every case. Most of the references are in the various output formats, where we inherited the absolute vs relative path decision from Ruff. Two other uses are as fallbacks if converting a relativized path to a string fails. Finally, we use the path for sorting and in `UnifiedFile::relative_path`.

## Test Plan

Existing tests, with snapshots updated to show absolute paths (in the `TestDb` this just added a `/` in front of the file names). I also updated the GitLab CLI test to set the `CI_PROJECT_DIR` environment variable and ran a test in GitLab CI:

<img width="613" height="114" alt="image" src="https://github.com/user-attachments/assets/8ab81dba-54fd-4a24-9110-77ef89293cff" />


---

_Comment by @github-actions[bot] on 2025-09-24 13:53_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-24 13:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-09-24 13:56_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/snapshots/ruff_db__diagnostic__render__json__tests__notebook_output.snap`:13 on 2025-09-24 13:56_

Were those paths (and in other output formatters) relative or absolute before you migrated the renderers to the new diagnostic output? Some folks might depend on paths being relative or absolute.

---

_Comment by @github-actions[bot] on 2025-09-24 14:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-09-24 14:06_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/snapshots/ruff_db__diagnostic__render__json__tests__notebook_output.snap`:13 on 2025-09-24 14:06_

They were always absolute in Ruff (they just used `SourceFile::name`, which is still the case now just via `UnifiedFile`). JSON for example:

```
> uvx ruff@0.9.0 check try.py --output-format json | jq '.[].filename'
"/tmp/tmp.9hyS01e8LF/try.py"
```

---

_Marked ready for review by @ntBre on 2025-09-24 14:20_

---

_Review requested from @carljm by @ntBre on 2025-09-24 14:20_

---

_Review requested from @sharkdp by @ntBre on 2025-09-24 14:20_

---

_Review requested from @dcreager by @ntBre on 2025-09-24 14:20_

---

_Label `bug` added by @ntBre on 2025-09-24 14:20_

---

_Label `ty` added by @ntBre on 2025-09-24 14:20_

---

_Review request for @dcreager removed by @ntBre on 2025-09-24 14:21_

---

_Review request for @carljm removed by @ntBre on 2025-09-24 14:21_

---

_Review request for @sharkdp removed by @ntBre on 2025-09-24 14:21_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/snapshots/ruff_db__diagnostic__render__json__tests__notebook_output.snap`:13 on 2025-09-24 15:09_

I guess I'm slightly confused why the snapshots change...

---

_@MichaReiser reviewed on 2025-09-24 15:09_

---

_@MichaReiser approved on 2025-09-24 15:10_

Looks good, if we're certain that this isn't a behavior change for Ruff

---

_@ntBre reviewed on 2025-09-24 15:14_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/snapshots/ruff_db__diagnostic__render__json__tests__notebook_output.snap`:13 on 2025-09-24 15:14_

The snapshots in `ruff_db` use the `TestDb` infrastructure shared with ty. The Ruff snapshots like [`lint__output_format_json.snap`](https://github.com/astral-sh/ruff/blob/main/crates/ruff/tests/snapshots/lint__output_format_json.snap) from the Ruff CLI tests didn't change.

I probably should have made that more clear in the summary.

This shouldn't affect anything on the Ruff side, only formats using a `Db`.

---

_Merged by @ntBre on 2025-09-24 17:16_

---

_Closed by @ntBre on 2025-09-24 17:16_

---

_Branch deleted on 2025-09-24 17:16_

---
