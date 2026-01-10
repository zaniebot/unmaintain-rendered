---
number: 11534
title: Make uv tool upgrade --all tolerant to deleted Python versions
type: issue
state: open
author: owenlamont
labels:
  - enhancement
assignees: []
created_at: 2025-02-15T11:05:08Z
updated_at: 2025-02-15T16:33:21Z
url: https://github.com/astral-sh/uv/issues/11534
synced_at: 2026-01-10T01:25:07Z
---

# Make uv tool upgrade --all tolerant to deleted Python versions

---

_Issue opened by @owenlamont on 2025-02-15 11:05_

### Summary

This is partly a follow up to some of the commentary on https://github.com/astral-sh/uv/issues/8067 I found if I deleted a uv python version (in this case because I'd installed Python 3.13.2 with uv), e.g.

```shell
uv python uninstall cpython-3.13.1-windows-x86_64-none
```

and that was the Python my uv tools were installed against, when I run:

```shell
uv tool upgrade --all
```

I get a long list of errors, e.g.

```shell
error: Failed to upgrade hatch
  Caused by: `hatch` is missing a valid environment; run `uv tool install --force hatch` to reinstall
error: Failed to upgrade linkchecker
  Caused by: `linkchecker` is missing a valid environment; run `uv tool install --force linkchecker` to reinstall
error: Failed to upgrade maturin
  Caused by: `maturin` is missing a valid environment; run `uv tool install --force maturin` to reinstall
error: Failed to upgrade mkdocs
  Caused by: `mkdocs` is missing a valid environment; run `uv tool install --force mkdocs` to reinstall
```

My only options (I'm aware of) to get around this are to:

1. For each tool call `uv tool install --force <tool name>` as suggested (but I have dozens of tools and that's onerous).
2. Reinstall the Python I deleted `uv python install cpython-3.13.1-windows-x86_64-none`, then run `uv tool upgrade --all --python 3.13.2`, then uninstall the older Python after the upgrade `uv python install cpython-3.13.1-windows-x86_64-none` - which is easy when you understand the workflow but not obvious and for me required searching uv documentation / issues

I was hoping the behaviour of `uv tool upgrade --all` could be changed to do a force reinstall itself in the situation the previous Python it was installed against was removed - or there could be some new CLI options, e.g. `uv tool upgrade --all --force` or `uv tool reinstall --all` - something like that where I could express I do want to force reinstall everything in the case I know I have deleted the Python the tool was installed with.

I guess a related bit of feature parity it'd be nice to have versus pipx is `pipx list` shows the Python version each package was installed with whereas `uv tool list` doesn't show that.

My current uv version is: `uv 0.6.0 (591f38c25 2025-02-14)`

---

_Label `enhancement` added by @owenlamont on 2025-02-15 11:05_

---

_Assigned to @zanieb by @zanieb on 2025-02-15 16:33_

---

_Referenced in [astral-sh/uv#14814](../../astral-sh/uv/issues/14814.md) on 2025-07-22 15:59_

---
