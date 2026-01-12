```yaml
number: 19181
title: "[ty] Track open files in the server"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - internal
  - server
  - ty
assignees: []
draft: true
base: main
head: dhruv/track-open-files-in-server
created_at: 2025-07-07T14:32:36Z
updated_at: 2025-07-10T14:41:32Z
url: https://github.com/astral-sh/ruff/pull/19181
synced_at: 2026-01-12T15:56:33Z
```

# [ty] Track open files in the server

---

_@dhruvmanila_

## Summary

Closes: astral-sh/ty#619

This PR changes the way files are being tracked in the ty server. Specifically, it makes the following changes for the respective notification handler:

- `didOpen`: Add the `File` corresponding to the document that was opened in the open file set. This requires interning the `File` eagerly because previously it was interned only when it was required and not on open notification
- `didClose`: Remove the `File` corresponding to the document was is closed from the open file set

Note: This PR on it's own breaks workspace diagnostics because the diagnostic builder is only created if the file (that the diagnostic is being reported for) is open. This will be fixed in a follow-up PR (#19182).

## Test Plan

Confirmed that workspace diagnostics break as expected (refer to the above note) and confirmed that it's fixed by #19182).

---

_Label `internal` added by @dhruvmanila on 2025-07-07 14:32_

---

_Label `server` added by @dhruvmanila on 2025-07-07 14:32_

---

_Label `ty` added by @dhruvmanila on 2025-07-07 14:32_

---

_Comment by @github-actions[bot] on 2025-07-07 14:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @dhruvmanila on 2025-07-08 09:15_

I don't think this approach works well with workspace diagnostics. If we start tracking open files using the existing open file set, then workspace diagnostics will stop working because the diagnostic builder checks if the file is open or not and avoids creating the builder if it's not opened.

---

_Comment by @MichaReiser on 2025-07-08 09:26_

> I don't think this approach works well with workspace diagnostics. If we start tracking open files using the existing open file set, then workspace diagnostics will stop working because the diagnostic builder checks if the file is open or not and avoids creating the builder if it's not opened.

It may require changing how we define `is_file_open` and how `CheckMode` works. That's what I mentioned on the workspace diagnostics where I said we may want to make more changes. I haven't thought it through fully but the simplest solution is to change how `is_file_open` defined (at least the version of it that the diagnostic builder uses) but it could also mean that we re-think how we check the "open files only"

---

_Comment by @dhruvmanila on 2025-07-08 12:26_

> It may require changing how we define `is_file_open` and how `CheckMode` works. That's what I mentioned on the workspace diagnostics where I said we may want to make more changes. I haven't thought it through fully but the simplest solution is to change how `is_file_open` defined (at least the version of it that the diagnostic builder uses) but it could also mean that we re-think how we check the "open files only"

Thanks, I'd need to spend some time thinking through what this would look like. I'm not exactly sure what kind of change are you thinking for the `is_file_open` function because this would somehow require the `CheckMode` as parameter.

---

_Comment by @MichaReiser on 2025-07-08 12:34_

> Thanks, I'd need to spend some time thinking through what this would look like. I'm not exactly sure what kind of change are you thinking for the is_file_open function because this would somehow require the CheckMode as parameter.

One option is to store the `CheckMode` on Project rather than passing it as an argument. This way it becomes stateful the same as many other properties on `Project`. We could then introduce a new `is_checked` or similar method that returns `true` if the file is included according to the selected `CheckMode`. 



---

_Comment by @dhruvmanila on 2025-07-08 13:05_

> One option is to store the `CheckMode` on Project rather than passing it as an argument. This way it becomes stateful the same as many other properties on `Project`. We could then introduce a new `is_checked` or similar method that returns `true` if the file is included according to the selected `CheckMode`.

I see. Yeah, that could work. We'll need to add it to the `ProjectMetadata`. I was wondering if changing the check mode would invalidate the salsa cache which doesn't seem useful because the diagnostics wouldn't really change here.

---

_Comment by @MichaReiser on 2025-07-08 13:54_

> I see. Yeah, that could work. We'll need to add it to the ProjectMetadata. I was wondering if changing the check mode would invalidate the salsa cache which doesn't seem useful because the diagnostics wouldn't really change here.

That's a fair concern and yes, it would invalidate all type inference queries that try to emit a diagnostic. These shouldn't be too many

---

_Comment by @Gankra on 2025-07-08 14:45_

How does WorkspaceDiagnostic ever even get requested? I don't see any obvious setting/selections for it in vscode. (Intuitively it seems like workspace/file mode is a thing a user would set once and forget about, and I went digging for the setting to confirm that, but now I have no clue.)

---

_Comment by @MichaReiser on 2025-07-08 14:57_

@Gankra see https://github.com/astral-sh/ty/blob/main/docs/reference/editor-settings.md#diagnosticmode (you may need to update your extension)

---

_Comment by @Gankra on 2025-07-08 16:04_

Aha yes, I didn't realize this was bleeding edge! Then yes I think it's ~fine to thrash if you toggle this setting.

---

_Comment by @dhruvmanila on 2025-07-10 14:36_

I'm going to merge them in a single PR as I'm finding it difficult to maintain this stack.

---

_Closed by @dhruvmanila on 2025-07-10 14:36_

---

_Branch deleted on 2025-07-10 14:41_

---
