```yaml
number: 22335
title: "[ty] Publish diagnostics for all open files (#1652)"
type: pull_request
state: open
author: ficd0
labels:
  - server
  - ty
assignees: []
base: main
head: fix-1652
created_at: 2026-01-02T01:45:40Z
updated_at: 2026-01-09T17:59:15Z
url: https://github.com/astral-sh/ruff/pull/22335
synced_at: 2026-01-10T15:56:07Z
```

# [ty] Publish diagnostics for all open files (#1652)

---

_Pull request opened by @ficd0 on 2026-01-02 01:45_

This commit fixes https://github.com/astral-sh/ty/issues/1652, improving support for LSP clients that don't support pull diagnostics by doing the following:

- Implemented support for the `didSave` notification type:
  - Registered handler and advertise this capability on server init
  - Updated tests: init snapshots now include the new capability
- On `didSave` for any document, we call `publish_diagnostics_if_needed` on any open documents.

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @carljm by @ficd0 on 2026-01-02 01:45_

---

_Review requested from @MichaReiser by @ficd0 on 2026-01-02 01:45_

---

_Review requested from @sharkdp by @ficd0 on 2026-01-02 01:45_

---

_Review requested from @dcreager by @ficd0 on 2026-01-02 01:45_

---

_Comment by @ficd0 on 2026-01-02 01:48_

Sorry, not sure why the issue wasn't automatically linked. The issue in question: https://github.com/astral-sh/ty/issues/1652

---

_Label `ty` added by @AlexWaygood on 2026-01-02 01:54_

---

_Label `server` added by @AlexWaygood on 2026-01-02 01:54_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_save.rs`:29 on 2026-01-05 10:11_

I think simply iterating over `text_document_handles` here will result in duplicate diagnostics for notebooks because each cell is represented as its own text document. 

I think we want to add a new method to `session` that returns an iterator over all `file_documents`. That is, all `TextDocument` where `notebook` is `None` and all `NotebookDocument`s. 



---

_@MichaReiser requested changes on 2026-01-05 10:12_

Thanks for working on this. 

We'll need to extend this a little to properly support notebooks. We should also add an E2E test (ideally including a notebook and a regular text document), see https://github.com/astral-sh/ruff/blob/dea48ecef06803502a265488e4a16b3e35778a57/crates/ty_server/tests/e2e/publish_diagnostics.rs#L18

---

_Comment by @ficd0 on 2026-01-09 17:45_

Thank you for the review and the helpful comments. I will get to work on implementing these changes when I'm able. If I'm confused about anything, I'll ping you here if you don't mind. Cheers! 

---

_Comment by @ficd0 on 2026-01-09 17:50_

@MichaReiser is there anything else I should keep in mind re notebooks? I've actually never used them so I kind of forgot the feature even existed, hence their omission from the first draft. 

Mainly, if I add an appropriate end to end test that covers both regular files and notebooks, do I still need to manually test with notebooks in my editor, too, or is the test enough for us to be confident it works? 

---

_Comment by @MichaReiser on 2026-01-09 17:59_

> Mainly, if I add an appropriate end to end test that covers both regular files and notebooks, do I still need to manually test with notebooks in my editor, too, or is the test enough for us to be confident it works?

An E2E test should be sufficient. I'm also more than happy to give it a try if I think any extra testing is necessary

---
