```yaml
number: 329
title: Rewrite the getting started documentation
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/getting-started
created_at: 2025-05-12T17:14:29Z
updated_at: 2025-05-12T19:59:44Z
url: https://github.com/astral-sh/ty/pull/329
synced_at: 2026-01-10T02:34:10Z
```

# Rewrite the getting started documentation

---

_Pull request opened by @zanieb on 2025-05-12 17:14_

[Rendered](https://github.com/astral-sh/ty/tree/zb/getting-started?tab=readme-ov-file#getting-started)

---

_Label `documentation` added by @zanieb on 2025-05-12 17:14_

---

_@MichaReiser reviewed on 2025-05-12 17:15_

---

_Review comment by @MichaReiser on `README.md`:48 on 2025-05-12 17:15_

This was sort of intentional to use `uv run` over `uvx` here because, as far as I understand, uvx doesn't set the virtual environment automatically. Meaning, your out-of-the-box experience is worse.

---

_@zanieb reviewed on 2025-05-12 17:30_

---

_Review comment by @zanieb on `README.md`:48 on 2025-05-12 17:30_

I'm exploring that, but the experience should be about equivalent with a `.venv` because ty detects that? `uv run` just implements discovery of `.venv` in parents, so I don't think the experience should be much different.

Since it's alpha, I think we might want to encourage the ephemeral experience over "make this a dependency of the project for other developers".

I'm just trying things out here right now though. I'm not sure what the best way to capture all the invocation / installation methods is yet.

---

_@MichaReiser reviewed on 2025-05-12 17:33_

---

_Review comment by @MichaReiser on `README.md`:48 on 2025-05-12 17:33_

I'm fine either way. I only wanted to point out why I went with `uv run` over `uvx`

---

_@zanieb reviewed on 2025-05-12 19:28_

---

_Review comment by @zanieb on `README.md`:48 on 2025-05-12 19:28_

I think I'm going to prefer `uvx` here because I don't want to encourage people to add it as a dependency (yet) and after a few iterations I think it's cleaner to avoid multiple installation methods in the README itself.

---

_Marked ready for review by @zanieb on 2025-05-12 19:28_

---

_Merged by @zanieb on 2025-05-12 19:59_

---

_Closed by @zanieb on 2025-05-12 19:59_

---

_Branch deleted on 2025-05-12 19:59_

---
