```yaml
number: 496
title: "Add `--no-config-file` option"
type: issue
state: open
author: MichaReiser
labels:
  - cli
  - needs-design
assignees: []
created_at: 2025-05-23T13:26:02Z
updated_at: 2025-06-05T04:58:52Z
url: https://github.com/astral-sh/ty/issues/496
synced_at: 2026-01-12T15:54:23Z
```

# Add `--no-config-file` option

---

_@MichaReiser_

Similar to uv's `--no-config-file` option

```
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).

          Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.

          [env: UV_NO_CONFIG=]
```

Depends on https://github.com/astral-sh/ruff/pull/18083

---

_Label `help wanted` added by @MichaReiser on 2025-05-23 13:26_

---

_Label `cli` added by @MichaReiser on 2025-05-23 13:26_

---

_Comment by @AlexWaygood on 2025-05-23 14:33_

We discussed on Discord a while back that an option to disable `site-packages` discovery would be useful (mypy has `--no-site-packages`), and you suggested that we could combine that option with an option to disable config file discovery as a `--isolated` flag similar to Ruff:
- https://discord.com/channels/1039017663004942429/1228460843033821285/1358449954397491231
- https://discord.com/channels/1039017663004942429/1228460843033821285/1358476341825241088
- https://discord.com/channels/1039017663004942429/1228460843033821285/1358481763294511395

---

_Label `help wanted` removed by @MichaReiser on 2025-05-23 14:57_

---

_Label `needs-design` added by @MichaReiser on 2025-05-23 14:57_

---

_Comment by @MichaReiser on 2025-05-23 14:57_

Good that someone remembers our discussions. Thanks for the links!

---

_Comment by @thejchap on 2025-06-04 22:55_

once this is "designed" id be willing to work on it (seems related to some of the other configuration/config file discovery i worked on recently) - let me know :)

---

_Comment by @zanieb on 2025-06-05 02:44_

nit: uv uses `--no-config` (and I think that's a reasonable name we should re-use unless there's a strong reason not to)

---

_Comment by @MichaReiser on 2025-06-05 04:58_

--no-config seems a bit odd, given that it wouldn't disable options provided by --config 



---
