```yaml
number: 6806
title: Avoid requiring a README or any module structure
type: issue
state: closed
author: michaelmior
labels:
  - question
assignees: []
created_at: 2024-08-29T12:11:51Z
updated_at: 2024-08-29T12:57:56Z
url: https://github.com/astral-sh/uv/issues/6806
synced_at: 2026-01-12T15:59:07Z
```

# Avoid requiring a README or any module structure

---

_@michaelmior_

I've experienced this on both macOS and Linux with uv 0.32. `uv init` will create `README.md` and an `src` folder with the skeleton of a module. However, if I don't need either of those with my project, then trying to use `uv run` fails. It would be nice if neither of these were required. It's unclear why the README is necessary. I can see some logic behind requiring the module, but it would be nice to skip over it if it doesn't exist. Sometimes I just want dependency management for a set of scripts then I never intend to redistribute as a package.

---

_Comment by @my1e5 on 2024-08-29 12:52_

Try uv 0.4.0 - there have been many changes. `uv init` now creates a simple script project not a library.

---

_Comment by @charliermarsh on 2024-08-29 12:52_

Yeah, I believe `uv init --app` should solve this for you!

---

_Closed by @charliermarsh on 2024-08-29 12:52_

---

_Label `question` added by @charliermarsh on 2024-08-29 12:52_

---

_Comment by @michaelmior on 2024-08-29 12:57_

@charliermarsh Thanks for the fast reply. uv 0.4.does indeed seem to solve the problem. Specifically, removing the following from my existing `pyproject.toml` resolved the issue.

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---
