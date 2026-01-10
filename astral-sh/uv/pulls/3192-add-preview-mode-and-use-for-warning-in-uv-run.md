```yaml
number: 3192
title: "Add preview mode and use for warning in `uv run`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/preview
created_at: 2024-04-22T17:13:15Z
updated_at: 2024-04-22T20:41:15Z
url: https://github.com/astral-sh/uv/pull/3192
synced_at: 2026-01-10T14:43:32Z
```

# Add preview mode and use for warning in `uv run`

---

_Pull request opened by @zanieb on 2024-04-22 17:13_

Adds hidden `--preview` / `--no-preview` flags with `UV_PREVIEW` environment variable support. Copies the `PreviewMode` type from Ruff.

Does a little bit of extra work to port `uv run` to the new settings model.

Note we allow `uv run` invocations without preview and only use its presence to toggle an experimental warning.

## Test plan

```
❯ cargo run -q -- run --no-workspace -- python --version
warning: `uv run` is experimental and may change without warning.
Python 3.12.2
❯ cargo run -q -- run --no-workspace --preview -- python --version
Python 3.12.2
❯ UV_PREVIEW=1 cargo run -q -- run --no-workspace -- python --version
Python 3.12.2
```

---

_Label `internal` added by @zanieb on 2024-04-22 17:13_

---

_Review requested from @konstin by @zanieb on 2024-04-22 17:21_

---

_Review requested from @charliermarsh by @zanieb on 2024-04-22 17:21_

---

_Marked ready for review by @zanieb on 2024-04-22 17:21_

---

_@charliermarsh reviewed on 2024-04-22 17:25_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/run.rs`:56 on 2024-04-22 17:25_

Should it not _require_ preview?

---

_@zanieb reviewed on 2024-04-22 17:45_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:56 on 2024-04-22 17:45_

Naw, maybe if we make it so its visibility is toggled by preview but right now the command is hidden and I think it's weird to require a preview flag to use a command e.g. `uv run --preview` feels awkward. I prefer just using preview to drop the nag for now. We can certainly revisit in the future, the main goal here is a flag for Konsti's project.

---

_@charliermarsh approved on 2024-04-22 20:19_

---

_Merged by @zanieb on 2024-04-22 20:41_

---

_Closed by @zanieb on 2024-04-22 20:41_

---

_Branch deleted on 2024-04-22 20:41_

---
