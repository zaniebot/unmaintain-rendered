```yaml
number: 16476
title: Uv tool install from git+https fails to find dependency on pypi
type: issue
state: closed
author: arvid-norlander
labels:
  - bug
assignees: []
created_at: 2025-10-28T08:47:53Z
updated_at: 2025-10-28T08:50:14Z
url: https://github.com/astral-sh/uv/issues/16476
synced_at: 2026-01-12T16:02:32Z
```

# Uv tool install from git+https fails to find dependency on pypi

---

_@arvid-norlander_

### Summary

```console
❯ uv tool install git+https://github.com/keirf/greaseweazle@latest
  × No solution found when resolving dependencies:
  ╰─▶ Because crcmod was not found in the package registry and greaseweazle==1.22 depends on crcmod, we can conclude that greaseweazle==1.22 cannot be used.
      And because only greaseweazle==1.22 is available and you require greaseweazle, we can conclude that your requirements are unsatisfiable.
```

However pipx manages this just fine:

```console
❯ pipx install git+https://github.com/keirf/greaseweazle@latest
  installed package greaseweazle 1.22, installed using Python 3.12.3
  These apps are now globally available
    - gw
done! ✨ 🌟 ✨
```

The dependency is on pypi: https://pypi.org/project/crcmod/

I might be doing something wrong, not sure. This is the first time I tried installing a tool from github as opposed to pypi. Looking at https://docs.astral.sh/uv/guides/tools/#requesting-different-sources it seems it should be supported? I tried with `uvx` also (as opposed to `uv tool`) and got the same result though.

### Platform

Ubuntu 24.04 (Linux 6.14.0-33-generic x86_64 GNU/Linux)

### Version

uv 0.9.5

### Python version

Python 3.12.3

---

_Label `bug` added by @arvid-norlander on 2025-10-28 08:47_

---

_Comment by @arvid-norlander on 2025-10-28 08:50_

Ah, never mind, I figured out the issue. I had a left over UV_INDEX_URL set, the issue is with the mirroring of the public pypi to that mirror I guess.

---

_Closed by @arvid-norlander on 2025-10-28 08:50_

---
