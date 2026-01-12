```yaml
number: 19708
title: Include column numbers in GitLab output format.
type: pull_request
state: merged
author: cristian64
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: gitlab_format_column_numbers
created_at: 2025-08-02T22:56:03Z
updated_at: 2025-08-03T12:45:05Z
url: https://github.com/astral-sh/ruff/pull/19708
synced_at: 2026-01-12T15:56:45Z
```

# Include column numbers in GitLab output format.

---

_@cristian64_

## Summary

The CodeClimate report format, which GitLab adopted in a reduced form, supports two ways for specifying diagnostic error locations:

```json
{
  "path": "path/to/file.css",
  "lines": {
    "begin": 13,
    "end": 14
  }
}
```

```json
{
  "path": "path/to/file.css",
  "positions": {
    "begin": {
      "line": 3,
      "column": 10
    },
    "end": {
      "line": 4,
      "column": 12
    }
  }
}
```

The second form is richer, as it can include column numbers, which Ruff happens to be able to provide. Ruff will now use the second form.

More details on the specification in https://github.com/codeclimate/platform/blob/master/spec/analyzers/SPEC.md#locations.

GitLab supports both forms; see https://docs.gitlab.com/ci/testing/code_quality/#code-quality-report-format.

## Test Plan

Change is covered by unit tests; snapshots have been updated.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/gitlab.rs`:78 on 2025-08-03 07:54_

Nit

```rust
let (start_location, end_location) = if self.context.is_notebook(&filename) {
	Default::default()
} else {
	start_location = diagnostic.expect_ruff_start_location();
  end_location = diagnostic.expect_ruff_end_location();
};
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/gitlab.rs`:120 on 2025-08-03 07:55_

Would this work?
```suggestion
                        "begin": start_location,
                        "end": end_location,
```

---

_@MichaReiser reviewed on 2025-08-03 07:55_

Thank you. I've a few nit comments but that overall looks good

---

_Label `diagnostics` added by @MichaReiser on 2025-08-03 07:56_

---

_Review comment by @cristian64 on `crates/ruff_linter/src/message/gitlab.rs`:78 on 2025-08-03 09:00_

Did you mean

```rust
            let (start_location, end_location) = if self.context.is_notebook(&filename) {
                // We can't give a reasonable location for the structured formats,
                // so we show one that's clearly a fallback
                (LineColumn::default(), LineColumn::default())
            } else {
                (
                    diagnostic.expect_ruff_start_location(),
                    diagnostic.expect_ruff_end_location(),
                )
            };
```

---

_@cristian64 reviewed on 2025-08-03 09:00_

---

_@MichaReiser reviewed on 2025-08-03 09:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/gitlab.rs`:78 on 2025-08-03 09:05_

Oh yes, sorry. The else branch should return a tuple. I think using `Default::default` might work for the notebook branch (I think)

---

_@MichaReiser approved on 2025-08-03 12:33_

Thank you

---

_Merged by @MichaReiser on 2025-08-03 12:37_

---

_Closed by @MichaReiser on 2025-08-03 12:37_

---

_Comment by @github-actions[bot] on 2025-08-03 12:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
