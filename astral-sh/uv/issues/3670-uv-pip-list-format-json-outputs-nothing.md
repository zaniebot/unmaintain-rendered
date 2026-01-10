---
number: 3670
title: "`uv pip list --format json` outputs _nothing_"
type: issue
state: closed
author: thejcannon
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-05-20T15:48:22Z
updated_at: 2024-05-20T21:09:49Z
url: https://github.com/astral-sh/uv/issues/3670
synced_at: 2026-01-10T01:23:30Z
---

# `uv pip list --format json` outputs _nothing_

---

_Issue opened by @thejcannon on 2024-05-20 15:48_

It'd be nice if we could reliably parse the output as JSON, but right now if there's nothing installed matching the criteria I get _nothing_ (rather than an empty list). 😭 

```console
$ uv venv .venv
$ uv pip list --format json --python .venv/bin/python
$
$ uv --version                                           
uv 0.1.44 (d417daad7 2024-05-14)
```

(same for `--editable` in a seeded venv)


---

_Label `bug` added by @charliermarsh on 2024-05-20 15:49_

---

_Comment by @thejcannon on 2024-05-20 15:52_

I suspect this might be around where the bug is: https://github.com/astral-sh/uv/blob/c32fb8647f53639a6bf19f93e9971ed36115b7f8/crates/uv/src/commands/pip/list.rs#L69

Dunno if `uv` should special-case JSON, or allow `results` to flow through all output formats

---

_Comment by @charliermarsh on 2024-05-20 15:54_

My instinct would be to check what `pip` does and follow that for compatibility. PR welcome if you'd like! Otherwise I can get to it today.

---

_Comment by @thejcannon on 2024-05-20 16:01_

`pip` does `[]`

```console
$ uv venv --seed
$ .venv/bin/pip list --format json --editable
$ []
$ .venv/bin/pip --version                    
pip 24.0 from ...
```

---

_Label `good first issue` added by @zanieb on 2024-05-20 16:03_

---

_Comment by @thejcannon on 2024-05-20 16:06_

(If I had a dev env ready for hacking I'd totally just dive in 😭 )

---

_Comment by @charliermarsh on 2024-05-20 16:17_

Haha no worries. I can fix today.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-20 16:22_

---

_Referenced in [astral-sh/uv#3671](../../astral-sh/uv/pulls/3671.md) on 2024-05-20 16:28_

---

_Closed by @charliermarsh on 2024-05-20 16:39_

---

_Comment by @thejcannon on 2024-05-20 17:26_

To close the loop with `pip` and the other formats:
```console
$ .venv/bin/pip list --editable --format columns
$ .venv/bin/pip list --editable --format freeze
$
```

so it's just JSON

---

_Comment by @charliermarsh on 2024-05-20 21:09_

Out now in [v0.1.45](https://github.com/astral-sh/uv/releases/tag/0.1.45).

---
