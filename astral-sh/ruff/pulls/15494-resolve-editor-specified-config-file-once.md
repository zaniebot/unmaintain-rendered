```yaml
number: 15494
title: Resolve editor-specified config file once
type: pull_request
state: closed
author: dhruvmanila
labels:
  - server
assignees: []
draft: true
base: main
head: dhruv/resolve-config-once
created_at: 2025-01-15T12:42:00Z
updated_at: 2025-02-20T11:07:53Z
url: https://github.com/astral-sh/ruff/pull/15494
synced_at: 2026-01-12T15:55:51Z
```

# Resolve editor-specified config file once

---

_@dhruvmanila_

## Summary

This PR updates the logic for handling `ruff.configuration` setting to read and resolve the path (if specified) once.

The main motivation for this is so that we can send a request to the client to show the notification in case there are any failures when processing this setting. Without this change, the notification will be send as many times as the number of config files present in the project.

Closes: #12572

## Test Plan

<!-- How was it tested? -->


---

_Label `server` added by @dhruvmanila on 2025-01-15 12:42_

---

_Comment by @github-actions[bot] on 2025-01-15 12:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2025-01-15 13:07_

Hmm, this might not be the correct solution. We'll also need to register the file watcher for the path specified in `ruff.configuration` and update it. And, we don't really control the file-watcher on server side as we rely on the client side so we can't be sure that this is happening which is also the case for existing config files.

---

_Comment by @dhruvmanila on 2025-02-20 11:07_

I'm going to close this for now with the changes that are going to happen for in-editor settings.

---

_Closed by @dhruvmanila on 2025-02-20 11:07_

---
