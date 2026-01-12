```yaml
number: 8124
title: "[Feature] Allow setting no-editable in `pyproject.toml`"
type: issue
state: open
author: b0o
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-10-11T05:57:34Z
updated_at: 2025-05-04T18:19:44Z
url: https://github.com/astral-sh/uv/issues/8124
synced_at: 2026-01-12T15:59:19Z
```

# [Feature] Allow setting no-editable in `pyproject.toml`

---

_@b0o_

My project includes multiple native C modules with a rather complicated build setup. Editable installs of my package don't work as expected:

When I run `uv build`, it ends up creating `./.venv/lib/python3.12/site-packages/my_package/` which contains lots of `.so` files, and it also creates a `./.venv/lib/python3.12/site-packages/_my_package.pth`. When I run my tests, the `.so` modules are not found because the `_my_package.pth` takes precedence and points to my source dir.

Using the `--no-editable` flag works around this, but I need to specify it every time I run a uv command, or else uv will re-sync and create an editable install.

It would be preferable to specify this in my `pyproject.toml`, for example:

```toml
[tool.uv]
no-editable = true
```

---

_Comment by @charliermarsh on 2024-10-14 14:09_

This makes sense -- thanks!

---

_Label `enhancement` added by @charliermarsh on 2024-10-14 14:09_

---

_Comment by @charliermarsh on 2024-10-14 14:09_

We probably want this to be an inherited setting, such that if it's set on the workspace, it's inherited by its members (but overridden by the CLI flag).

---

_Label `help wanted` added by @charliermarsh on 2024-10-14 14:09_

---

_Comment by @theholy7 on 2025-05-04 18:19_

I just came across this issue while deploying on Render. I used `uv export --no-editable > requirements.txt`.

Given I am deploying a Django app, I think I'd set this in the `pyproject.toml` file as suggested in this fix. Would love to see this feature added.

---
