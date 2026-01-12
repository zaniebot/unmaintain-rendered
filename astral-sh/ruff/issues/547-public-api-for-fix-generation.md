```yaml
number: 547
title: Public api for fix generation
type: issue
state: closed
author: Seamooo
labels: []
assignees: []
created_at: 2022-11-02T09:15:57Z
updated_at: 2022-11-02T13:44:48Z
url: https://github.com/astral-sh/ruff/issues/547
synced_at: 2026-01-12T15:54:40Z
```

# Public api for fix generation

---

_@Seamooo_

Currently the checks returned by `ruff::check` will never include fixes, as `ruff::check_path` is called with `ruff::autofix::fixer::Mode::None`.

It would be preferable if either `ruff::autofix::fixer::Mode::Generate` was the default or that this is a parameter passed by the public API.

Alternatively another interface could be created such as `check_with_fixes` that runs with `Mode::Generate` instead of `Mode::None`.

In essence any public api for fix generation

---

_Comment by @Seamooo on 2022-11-02 09:17_

I'm happy to put together a PR for this but given that the solution is more personal preference than anything else, this issue exists.

---

_Comment by @charliermarsh on 2022-11-02 12:33_

Ah yeah, my mistake. Let’s change the default in that path to Generate.

---

_Closed by @charliermarsh on 2022-11-02 13:44_

---
