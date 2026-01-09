---
number: 14273
title: "Ability to specify platform when running `uv sync`"
type: issue
state: closed
author: LordAro
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-06-26T14:12:04Z
updated_at: 2025-07-11T01:38:30Z
url: https://github.com/astral-sh/uv/issues/14273
synced_at: 2026-01-07T13:12:18-06:00
---

# Ability to specify platform when running `uv sync`

---

_Issue opened by @LordAro on 2025-06-26 14:12_

### Summary

So I'm using uv to generate a distributable python install with some extra dependencies. Not strictly what it was meant for, but hey.

Currently I'm doing this with:

```
uv python install 3.12.10 --install-dir .
cp cpython-3.12.10-linux-x86_64-gnu my-python-dist # Copy can't be avoided due to #11933
uv export --no-hashes --no-default-groups > requirements-core.txt # convert pyproject.toml deps to pip compatible
uv pip install --requirements requirements-core.txt --break-system-packages --link-mode copy --python-platform x86_64-manylinux_2_28 --python my-python-dist
```

This works, but is rather complicated and requires several steps and redundant copies.

I can *almost* replace it with `UV_PROJECT_ENVIRONMENT=my-python-dist uv sync --link-mode copy` except that there's no specific platform, so running this command on a newer system might end up downloading a wheel that is incompatible with the intended systems. The "simple solution" would be to add `--python-platform` to `uv sync` (& friends, most likely) to match `pip`.

I've tried fudging around with `environments` & `required-environments`, but there's no way to specify "<= manylinux_2_28" as packaging markers as far as I can tell.

Unless there's A Better Way, then by all means let me know

### Example

`uv sync --python-platform x86_64-manylinux_2_28`

---

_Label `enhancement` added by @LordAro on 2025-06-26 14:12_

---

_Comment by @charliermarsh on 2025-06-26 14:17_

Yeah I think we should have `--python-platform` on `uv sync`. I've wanted that before too.

---

_Label `help wanted` added by @charliermarsh on 2025-06-26 15:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-27 17:21_

---

_Referenced in [astral-sh/uv#14320](../../astral-sh/uv/pulls/14320.md) on 2025-06-27 17:48_

---

_Closed by @charliermarsh on 2025-07-11 01:38_

---

_Referenced in [NixOS/nixpkgs#422471](../../NixOS/nixpkgs/issues/422471.md) on 2025-07-11 15:41_

---

_Referenced in [astral-sh/uv#15035](../../astral-sh/uv/issues/15035.md) on 2025-08-02 23:57_

---
