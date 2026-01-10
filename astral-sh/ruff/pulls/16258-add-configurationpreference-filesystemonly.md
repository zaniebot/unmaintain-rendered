```yaml
number: 16258
title: "Add `ConfigurationPreference::FilesystemOnly`"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - server
assignees: []
draft: true
base: main
head: ls-filesystem-only
created_at: 2025-02-19T15:27:13Z
updated_at: 2025-03-14T09:17:58Z
url: https://github.com/astral-sh/ruff/pull/16258
synced_at: 2026-01-10T19:49:01Z
```

# Add `ConfigurationPreference::FilesystemOnly`

---

_Pull request opened by @InSyncWithFoo on 2025-02-19 15:27_

## Summary

Resolves #16209.

When `configurationPreference` is set to `filesystemOnly`, Ruff ignores all editor-level configurations. This is the same as `editorOnly`, but inverted.

## Test Plan

None.


---

_Comment by @InSyncWithFoo on 2025-02-19 15:28_

This seems surprisingly easy. I take it that the versioning problem mentioned in #16209 is to be handled within the extension?

---

_Comment by @github-actions[bot] on 2025-02-19 15:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-02-19 17:57_

Could you please add a test plan (video?) demonstrating that this works as expected.

I'm also not sure if this is something we have a decision on or if it's just a feature request. @dhruvmanila ?

---

_Label `server` added by @MichaReiser on 2025-02-19 17:57_

---

_Comment by @dhruvmanila on 2025-02-20 06:03_

> I'm also not sure if this is something we have a decision on or if it's just a feature request. @dhruvmanila ?

I think it makes sense to add it.

> This seems surprisingly easy. I take it that the versioning problem mentioned in #16209 is to be handled within the extension?

It also requires updating the VS Code extension to allow the value in the editor settings. By versioning problem, I mainly meant that it might be good to check the Ruff version in the extension to see if this value is supported or not and handle it on the extension side otherwise the server will raise a deserialization error.

---

_Comment by @MichaReiser on 2025-02-20 07:49_

I'll reply on the issue. Let's keep this in draft and re-publish once we're aligned on the behavior.

---

_Converted to draft by @MichaReiser on 2025-02-20 07:49_

---

_Comment by @MichaReiser on 2025-03-14 09:17_

Thanks again for opening this PR. I don't think we want to change this at the moment. We can re-open this PR if we decide to do so in the future

---

_Closed by @MichaReiser on 2025-03-14 09:17_

---
