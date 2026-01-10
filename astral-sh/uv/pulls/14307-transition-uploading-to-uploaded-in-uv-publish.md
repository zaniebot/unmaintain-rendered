```yaml
number: 14307
title: "Transition \"Uploading\" to \"Uploaded\" in `uv publish`"
type: pull_request
state: closed
author: winstonallo
labels: []
assignees: []
draft: true
base: main
head: 14196-publish-transition-uploading-to-uploaded
created_at: 2025-06-27T09:43:10Z
updated_at: 2025-06-27T10:25:43Z
url: https://github.com/astral-sh/uv/pull/14307
synced_at: 2026-01-10T07:01:23Z
```

# Transition "Uploading" to "Uploaded" in `uv publish`

---

_Pull request opened by @winstonallo on 2025-06-27 09:43_

## Summary

`uv publish` shows `Uploading <filename> <size>` for each file when uploading, it however does not transition to `Uploaded <filename> <size>` when finished. 

Closes #14196 

---

_Comment by @konstin on 2025-06-27 10:18_

I don't we can just reset the line here, this is a more complicated change where we want to switch to the progress reporter and use a similar pattern as the `Building`/`Built` messages.

---

_Comment by @winstonallo on 2025-06-27 10:25_

> I don't we can just reset the line here, this is a more complicated change where we want to switch to the progress reporter and use a similar pattern as the `Building`/`Built` messages.

Alright, thanks for the heads up!

---

_Closed by @winstonallo on 2025-06-27 10:25_

---
