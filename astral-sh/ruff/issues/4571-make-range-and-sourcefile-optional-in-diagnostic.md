```yaml
number: 4571
title: "Make `Range` and `SourceFile` optional in `Diagnostic` and `Message`"
type: issue
state: open
author: konstin
labels:
  - internal
  - diagnostics
assignees: []
created_at: 2023-05-22T08:12:10Z
updated_at: 2025-08-07T13:33:49Z
url: https://github.com/astral-sh/ruff/issues/4571
synced_at: 2026-01-10T11:09:47Z
```

# Make `Range` and `SourceFile` optional in `Diagnostic` and `Message`

---

_Issue opened by @konstin on 2023-05-22 08:12_

There are some cases where we can't produce a relevant span or may not be able to read the source into a string, e.g. [`toml` sometimes doesn't emit spans](https://github.com/charliermarsh/ruff/pull/4496/files#r1198653173), `IOError` or files larger than 4GB. We should therefore make `Range` and `SourceFile` optional in `Diagnostic` and `Message`.

---

_Label `internal` added by @MichaReiser on 2023-05-22 08:45_

---

_Label `diagnostics` added by @MichaReiser on 2024-12-09 12:33_

---

_Comment by @MichaReiser on 2025-08-07 13:17_

I think this is partially addressed by @ntBre's diagnostic refactor. I think the only missing piece is that we still have some calls that expect a range to be present. @ntBre how difficult would it be to replace those (just asking, not something we have to prioritise)

---

_Comment by @ntBre on 2025-08-07 13:24_

This sounds straightforward to me, especially for `IOError`. `Violation::into_diagnostic` and `create_lint_diagnostic` both expect a range and file currently, but we could either make those optional, or construct these diagnostics separately. It's definitely supported by the new diagnostic model.

---

_Assigned to @ntBre by @ntBre on 2025-08-07 13:24_

---

_Comment by @MichaReiser on 2025-08-07 13:27_

I'm mainly worried about 

https://github.com/astral-sh/ruff/blob/b324ae1be3183220ee42c01b394e81c91f0012f5/crates/ruff_db/src/diagnostic/mod.rs#L437

and similar calls

---

_Comment by @ntBre on 2025-08-07 13:31_

Oh right 🤦 I think those are only used in a few remaining output formats. Finding references shows github, gitlab, sarif, and grouped. It would be nice to update those formats and remove the `expect` methods entirely.

---

_Comment by @MichaReiser on 2025-08-07 13:33_

Perfect. That means there's a design question on what to do if a diagnostic doesn't have a range but leaving that decision to the output format seems better than assuming one when creating the diagnostic.

---
