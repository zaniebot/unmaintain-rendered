```yaml
number: 4710
title: "`uv pip tree` should display extras"
type: issue
state: open
author: ibraheemdev
labels:
  - help wanted
  - cli
assignees: []
created_at: 2024-07-01T21:23:22Z
updated_at: 2024-12-03T14:34:36Z
url: https://github.com/astral-sh/uv/issues/4710
synced_at: 2026-01-10T04:36:20Z
```

# `uv pip tree` should display extras

---

_Issue opened by @ibraheemdev on 2024-07-01 21:23_

`uv pip tree` should display extras that are installed in the environment.

---

_Label `cli` added by @ibraheemdev on 2024-07-01 21:23_

---

_Label `help wanted` added by @zanieb on 2024-07-02 01:48_

---

_Comment by @potiuk on 2024-07-02 13:51_

I believe that's impossible - that information is not stored after installation is finished - simply extras are only used while installation command runs and they are discarded (at least that's how it work when you install packages via `pip` - but it also means that there is no metadata stored in the system that can be queried about it.

---

_Comment by @zanieb on 2024-07-02 14:33_

Hm yeah so at best we would be guessing.

There's relevant discussion about tracking the metadata at https://github.com/pypa/packaging-problems/issues/215 maybe we should just track this?

---

_Comment by @ChannyClaus on 2024-07-03 01:14_

>There's relevant discussion about tracking the metadata at https://github.com/pypa/packaging-problems/issues/215 maybe we should just track this?

that sounds reasonable to me - in the meantime if there's a lot of demand on this feature people can react to this issue, probably? since there hasn't been any update on the linked issue for years, might make sense to just implement the best-effort 🌵 

---

_Comment by @Enayaaa on 2024-07-30 14:11_

This would be very useful, right now, packages installed via extras are orphaned in the dependency tree.
```console
$ uv venv  .venv && source .venv/bin/activate
$ uv pip install urllib3[socks]
$ uv pip tree
pysocks v1.7.1
urllib3 v2.2.2
```

---

_Comment by @ssbarnea on 2024-12-03 11:33_

Related to this, I discovered that `uv tree` does show all extras regardless of using `--package` option while `uv pip tree` does not show them. I guess the workaround is to create an isolated venv just for running the tree commands in order to obtain the desired outcome.

---

_Comment by @zanieb on 2024-12-03 14:34_

`uv tree` can use the lockfile metadata to identify which extra a package was installed from, whereas the `uv pip` interface (and virtual environments more generally) do not have this metadata (as discussed above).

---
