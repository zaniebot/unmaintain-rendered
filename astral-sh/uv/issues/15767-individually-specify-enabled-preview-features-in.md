---
number: 15767
title: Individually specify enabled preview features in configuration
type: issue
state: open
author: thcrt
labels:
  - enhancement
  - help wanted
  - configuration
assignees: []
created_at: 2025-09-10T12:56:03Z
updated_at: 2025-09-10T13:06:38Z
url: https://github.com/astral-sh/uv/issues/15767
synced_at: 2026-01-10T01:25:59Z
---

# Individually specify enabled preview features in configuration

---

_Issue opened by @thcrt on 2025-09-10 12:56_

### Summary

Currently, preview features can be enabled in two ways:
- in configuration, with either the `tool.uv.preview` key in `pyproject.toml` or the `preview` key in `uv.toml`; or
- on the command line, with the `--preview-features` or `--preview` flags, or the `UV_PREVIEW` or `UV_PREVIEW_FEATURES` environment variables.

There's a disparity here: On the command line, I have a choice between enabling *all* preview features (with `--preview` or `UV_PREVIEW=1`) and only enabling *individual* features (with `--preview-features=foo,bar,baz` or `UV_PREVIEW_FEATURES=foo,bar,baz`). However, in configuration, I only have the former choice, and no way to enable specific features individually.

It would be nice to have this granularity be available in configuration.

### Example

A potential solution could be to change the type of the `tool.uv.preview` / `preview` value from a boolean to a list of strings:

```toml
# pyproject.toml
[tool.uv]
preview = [
  "json-output",
  "format"
]
```

Alternatively, to allow more flexibility when merging configs (plus backwards compatibility), we could use this format:
```toml
# pyproject.toml
[tool.uv]
preview = true

[tool.uv.preview-features]
json-output = true
format = true
pylock = false
```

In the latter case, setting `tool.uv.preview-features.pylock = false` would disable it, taking precedence over the feature potentially being enabled at a user level. If `tool.uv.preview` were changed to `false`, all preview features would be disabled, regardless of the content of `tool.uv.preview-features`.

---

_Label `enhancement` added by @thcrt on 2025-09-10 12:56_

---

_Comment by @zanieb on 2025-09-10 13:06_

Yeah we should add something here. I'm not exactly sure what the best option is. We should probably have `[tool.uv.preview-features]` with the same semantics as the CLI.

---

_Label `configuration` added by @zanieb on 2025-09-10 13:06_

---

_Label `help wanted` added by @zanieb on 2025-09-10 13:06_

---

_Referenced in [astral-sh/uv#16452](../../astral-sh/uv/pulls/16452.md) on 2025-10-26 07:41_

---
