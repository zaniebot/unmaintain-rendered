```yaml
number: 11492
title: Fix automatic configuration reloading for text and notebook documents
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: jane/server/fix-config-reload
created_at: 2024-05-22T09:08:56Z
updated_at: 2024-05-22T18:20:46Z
url: https://github.com/astral-sh/ruff/pull/11492
synced_at: 2026-01-12T15:55:38Z
```

# Fix automatic configuration reloading for text and notebook documents

---

_@snowsignal_

## Summary

Recent changes made in the [Jupyter Notebook feature PR](https://github.com/astral-sh/ruff/pull/11206) caused automatic configuration reloading to stop working. This was because we would check for paths to reload using the changed path, when we should have been using the parent path of the changed path (to get the directory it was changed in).

Additionally, this PR fixes an issue where `ruff.toml` and `.ruff.toml` files were not being automatically reloaded.

Finally, this PR improves configuration reloading by actively publishing diagnostics for notebook documents (which won't be affected by the workspace refresh since they don't use pull diagnostics). It will also publish diagnostics for text documents if pull diagnostics aren't supported.

## Test Plan
To test this, open an existing configuration file in a codebase, and make modifications that will affect one or more open Python / Jupyter Notebook files. You should observe that the diagnostics for both kinds of files update automatically when the file changes are saved.

Here's a test video showing what a successful test should look like:


https://github.com/astral-sh/ruff/assets/19577865/7172b598-d6de-4965-b33c-6cb8b911ef6c



---

_Label `bug` added by @snowsignal on 2024-05-22 09:08_

---

_Label `server` added by @snowsignal on 2024-05-22 09:08_

---

_Comment by @github-actions[bot] on 2024-05-22 09:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila requested changes on 2024-05-22 11:09_

Can you update the test plan to include a video showcasing this fix? I think that would helpful to quickly review this.

---

_@dhruvmanila approved on 2024-05-22 18:18_

---

_Merged by @snowsignal on 2024-05-22 18:20_

---

_Closed by @snowsignal on 2024-05-22 18:20_

---

_Branch deleted on 2024-05-22 18:20_

---
