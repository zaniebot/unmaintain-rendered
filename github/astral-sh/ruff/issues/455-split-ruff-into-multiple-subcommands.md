---
number: 455
title: "Split `ruff` into multiple subcommands"
type: issue
state: closed
author: charliermarsh
labels:
  - breaking
  - cli
assignees: []
created_at: 2022-10-18T21:50:48Z
updated_at: 2023-02-03T18:28:29Z
url: https://github.com/astral-sh/ruff/issues/455
synced_at: 2026-01-07T13:12:14-06:00
---

# Split `ruff` into multiple subcommands

---

_Issue opened by @charliermarsh on 2022-10-18 21:50_

We could already support `ruff format` or `ruff fmt` or `ruff rtrip` (in Astor parlance). Soon there will be more tools and sub-commands, e.g., #423.

A few open questions:

- Do we want to move the core linting functionality to `ruff check`? Or leave it as a "default" sub-command? (I think the latter can cause a variety of ambiguities but it'd be nice to do something backwards compatible here.)
- Do we want to expose a dedicated `ruff fix` command in lieu of or in addition to `--fix`? Perhaps `ruff fix` applies fixes recursively.


---

_Added to milestone `Release 0.1.0` by @charliermarsh on 2022-11-02 03:08_

---

_Label `breaking` added by @charliermarsh on 2022-11-03 02:13_

---

_Comment by @thejcannon on 2022-12-21 18:43_

Taking this a smidge further: one distinction I introduced to [Pants](https://github.com/pantsbuild/pants) is the distinction between `fmt` and `fix`. The former is safe to do on save, the latter isn't. The razor is something like removing imports. Not safe to do on save (as I'm still typing code and I save very often) but is safe to do in `fix`, which I'll issue manually.

We also have `fix` imply `fmt`, so users could decide which behavior they care about.

---

_Label `cli` added by @charliermarsh on 2022-12-31 18:11_

---

_Referenced in [pantsbuild/pants#17945](../../pantsbuild/pants/pulls/17945.md) on 2023-01-09 15:54_

---

_Referenced in [astral-sh/ruff#2155](../../astral-sh/ruff/pulls/2155.md) on 2023-01-25 14:47_

---

_Comment by @not-my-profile on 2023-01-26 06:11_

@thejcannon Thanks, that's a very interesting idea! I think the relevant issue is *rule applicability* (#1997).




---

_Referenced in [astral-sh/ruff#2190](../../astral-sh/ruff/pulls/2190.md) on 2023-01-26 07:46_

---

_Closed by @charliermarsh on 2023-01-28 00:38_

---
