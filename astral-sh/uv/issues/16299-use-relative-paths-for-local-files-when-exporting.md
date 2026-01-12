```yaml
number: 16299
title: "Use relative paths for local files when exporting `pylock.toml`"
type: issue
state: open
author: ncoghlan
labels:
  - bug
assignees: []
created_at: 2025-10-14T17:31:28Z
updated_at: 2025-10-17T09:55:07Z
url: https://github.com/astral-sh/uv/issues/16299
synced_at: 2026-01-12T16:02:28Z
```

# Use relative paths for local files when exporting `pylock.toml`

---

_@ncoghlan_

### Summary

The `pylock.toml` format supports relative file paths (defining them to be relative to the lock file).

When `uv lock` is run with local wheel directories configured via either `find-links` or flat file indexes, `uv export` currently emits `pylock.toml` files with absolute paths, so they end up coupled to the specific build location, rather than allowing the lock file and the local wheels it references to be published together using relative paths.

This is a feature request to have it emit paths relative to the exported `pylock.toml` file instead. Whether that's opt-in or just new standard behaviour would depend on if there are any expected use cases for publishing absolute paths that would *break* if a relative path was specified instead.

(I considered commenting on #13239 instead of filing a new feature request, but this feels like something narrower in scope that's less likely to have unwanted side effects, since it would only affect exported `pylock.toml` files, not `uv.lock` itself)

### Example

This specifically came up when migrating `venvstacks` from the legacy `requirements.txt` format over to `pylock.toml`: https://github.com/lmstudio-ai/venvstacks/issues/291

The expectation for the `venvstacks` use is that the layer lock files will be checked in, and the required locally built wheels (if that features is used) will be made available in a consistent location relative to those checked in files (perhaps via git-lfs, perhaps via separate downloads), rather than publishing them to a compliant index server.

---

_Label `enhancement` added by @ncoghlan on 2025-10-14 17:31_

---

_Label `enhancement` removed by @konstin on 2025-10-17 09:55_

---

_Label `bug` added by @konstin on 2025-10-17 09:55_

---
