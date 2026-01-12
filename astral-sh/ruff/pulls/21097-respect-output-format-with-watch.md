```yaml
number: 21097
title: "Respect `--output-format` with `--watch`"
type: pull_request
state: merged
author: ntBre
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: brent/watch-output-formats
created_at: 2025-10-27T13:17:21Z
updated_at: 2025-10-27T16:04:57Z
url: https://github.com/astral-sh/ruff/pull/21097
synced_at: 2026-01-12T15:57:16Z
```

# Respect `--output-format` with `--watch`

---

_@ntBre_

Summary
--

Fixes #19550

This PR copies our non-watch diagnostic rendering code into `Printer::write_continuously` in preview mode, allowing it to use whatever output format is passed in.

I initially marked this as also fixing #19552, but I guess that's not true currently but will be true once this is stabilized and we can remove the warning.

Test Plan
--

Existing tests, but I don't think we have any `watch` tests, so some manual testing as well. The default with just `ruff check --watch` is still `concise`, adding just `--preview` still gives the `full` output, and then specifying any other output format works, with JSON as one example:

<img width="695" height="719" alt="Screenshot 2025-10-27 at 9 21 41 AM" src="https://github.com/user-attachments/assets/98957911-d216-4fc4-8b6c-22c56c963b3f" />



---

_Label `cli` added by @ntBre on 2025-10-27 13:17_

---

_Label `preview` added by @ntBre on 2025-10-27 13:17_

---

_Renamed from "Respect `--output-format` in `ruff watch`" to "Respect `--output-format` in `--watch`" by @ntBre on 2025-10-27 13:19_

---

_Renamed from "Respect `--output-format` in `--watch`" to "Respect `--output-format` with `--watch`" by @ntBre on 2025-10-27 13:20_

---

_Comment by @github-actions[bot] on 2025-10-27 13:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-10-27 13:47_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-27 13:47_

---

_@MichaReiser reviewed on 2025-10-27 14:07_

---

_Review comment by @MichaReiser on `crates/ruff/src/printer.rs`:414 on 2025-10-27 14:07_

How's `DisplayDiagnostics` different from `render_diagnostics`? Or why can't we do

```
let format = if preview {
	self.format
} else {
	DiagnosticFormat::Concise
};

let config = DisplayDiagnosticConfig::default()
    .preview(preview)
    .hide_severity(true)
    .color(!cfg!(test) && colored::control::SHOULD_COLORIZE.should_colorize())
    .with_show_fix_status(show_fix_status(self.fix_mode, fixables.as_ref()))
    .with_fix_applicability(self.unsafe_fixes.required_applicability())
    .show_fix_diff(preview);

render_diagnostics(writer, self.format, config, &context, &diagnostics.inner)?;
```

---

_@ntBre reviewed on 2025-10-27 14:13_

---

_Review comment by @ntBre on `crates/ruff/src/printer.rs`:414 on 2025-10-27 14:13_

Yeah, I think that should be equivalent. I was trying not to change too much since we don't have tests for `--watch`, but this seems pretty easy to verify in hindsight 🤦 

---

_Comment by @dylwil3 on 2025-10-27 14:19_

> I initially marked this as also fixing https://github.com/astral-sh/ruff/issues/19552, but I guess that's not true currently but will be true once this is stabilized and we can remove the warning.

Don't we generally mark the issue as closed even if the solution is in `preview`?

---

_Comment by @ntBre on 2025-10-27 14:20_

> > I initially marked this as also fixing #19552, but I guess that's not true currently but will be true once this is stabilized and we can remove the warning.
> 
> Don't we generally mark the issue as closed even if the solution is in `preview`?

In general yes, but this felt a  little different  since it's basically a preview warning.

---

_Comment by @ntBre on 2025-10-27 14:22_

Actually the warning itself probably should be updated to mention preview now, but the warning isn't being shown anyway because of #19552 :upside_down_face: 

---

_@MichaReiser approved on 2025-10-27 16:02_

Thank you

---

_Merged by @ntBre on 2025-10-27 16:04_

---

_Closed by @ntBre on 2025-10-27 16:04_

---

_Branch deleted on 2025-10-27 16:04_

---
