```yaml
number: 7312
title: uv equivalent to pipx inject
type: issue
state: closed
author: zachvalenta
labels:
  - question
  - uv tool
assignees: []
created_at: 2024-09-11T21:09:45Z
updated_at: 2025-02-13T15:35:26Z
url: https://github.com/astral-sh/uv/issues/7312
synced_at: 2026-01-12T15:59:12Z
```

# uv equivalent to pipx inject

---

_@zachvalenta_

I searched through issues but didn't find anything similar - apologies if there's already an issue that I missed.

pipx let's you install (['inject'](https://pipx.pypa.io/stable/#inject-a-package)) additional packages into an env e.g. add `psycopg2` into the env for `visidata` so you can connect to Postgres:
```sh
pipx install visidata
pipx inject visidata psycopg2
```

Does `uv` have this capability now or, if not, is it on the roadmap?

---

_Renamed from "uv tool equivalent for pipx inject" to "uv equivalent to pipx inject" by @zachvalenta on 2024-09-11 21:10_

---

_Comment by @zanieb on 2024-09-11 21:18_

I think we explicitly won't provide this — you should enumerate all the dependencies in the installation.

There's #3560 but I'm not sure if it's explicitly discussed.

---

_Label `question` added by @zanieb on 2024-09-11 21:19_

---

_Label `uv tool` added by @zanieb on 2024-09-11 21:19_

---

_Comment by @zachvalenta on 2024-09-11 22:56_

Based on the issue you linked, seems like the way this works in `uv` is via the [`--with`](https://docs.astral.sh/uv/concepts/tools/#including-additional-dependencies) arg, yes?

```sh
# pipx
pipx install visidata
pipx inject visidata psycopg2

# uv
uv tool install --with psycopg2 visidata
```

---

_Comment by @zanieb on 2024-09-12 00:32_

Yep! This is covered in https://docs.astral.sh/uv/concepts/tools/#including-additional-dependencies — the RFC isn't necessarily exactly what was implemented.

---

_Comment by @zachvalenta on 2024-09-12 13:00_

Great. Will close this issue, and now anyone coming from pipx and the semantics of 'inject' will find the uv way of doing it.

---

_Closed by @zachvalenta on 2024-09-12 13:00_

---

_Comment by @holmboe on 2025-02-13 10:56_

An observation.

`uv tool install --with PKG ...` doesn't adress what can be achieved with:

```shell
pipx install sphinx
pipx inject --include-apps sphinx sphinx-autobuild
```

With uv I don't get the `sphinx-autobuild` executable on my path.

---

_Comment by @zanieb on 2025-02-13 15:35_

That's #6314 

---
