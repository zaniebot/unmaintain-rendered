---
number: 4654
title: "Show tool entry points in `uv tool list`"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - needs-design
  - cli
  - preview
assignees: []
created_at: 2024-06-29T22:53:58Z
updated_at: 2024-07-03T04:54:20Z
url: https://github.com/astral-sh/uv/issues/4654
synced_at: 2026-01-10T01:23:40Z
---

# Show tool entry points in `uv tool list`

---

_Issue opened by @zanieb on 2024-06-29 22:53_

We should display the name of the executable entry points that are provided by an installed tool e.g.

```
$ uv tool list
black
    black
    blackd
```

The format is up for debate, open questions include things like:

- Should we show the path to the executable?
- Should this be opt-in or display by default?
- Should we omit it for tools that provide a single entry points with the same name?

Another idea is like

```
$ uv tool list
black (provides: black, blackd)
```

When proposing a design here, we should take into account the output of other tools like `pipx`.

Related:

- #4653

---

_Label `help wanted` added by @zanieb on 2024-06-29 22:53_

---

_Label `needs-design` added by @zanieb on 2024-06-29 22:53_

---

_Label `cli` added by @zanieb on 2024-06-29 22:53_

---

_Label `preview` added by @zanieb on 2024-06-29 22:53_

---

_Referenced in [astral-sh/uv#4486](../../astral-sh/uv/issues/4486.md) on 2024-06-29 22:54_

---

_Referenced in [astral-sh/uv#4653](../../astral-sh/uv/issues/4653.md) on 2024-06-30 14:30_

---

_Comment by @zanieb on 2024-06-30 14:44_

The proposal to match `cargo install --list` in https://github.com/astral-sh/uv/issues/4653#issuecomment-2198488484 seems nice.

---

_Comment by @moreaupascal56 on 2024-06-30 15:13_

Hello I would like to have a look and help on this; but I am relatively new to uv and rust so it will not be immediate, if this is urgent I will just collaborate and have a look at the proposed solution to learn about the codebase and be useful someday else :) 
Anyway uv is amazing thanks to te dev team !

---

_Comment by @zanieb on 2024-06-30 15:20_

Thanks @moreaupascal56! Appreciate it :) 

It's likely someone will tackle this soon (or I will) but feel free to ask questions on the pull request. We'll be tagging more things as https://github.com/astral-sh/uv/labels/good%20first%20issue so keep an eye out.

---

_Referenced in [astral-sh/uv#4661](../../astral-sh/uv/pulls/4661.md) on 2024-06-30 17:35_

---

_Comment by @T-256 on 2024-06-30 17:52_

> The proposal to match `cargo install --list` in [#4653 (comment)](https://github.com/astral-sh/uv/issues/4653#issuecomment-2198488484) seems nice.

I like it, another possible design could be `uv tool list` show entrypoints instead of installed tools:
```sh
$ uv tool list
black
blackd (from black)
```
By that, no indentions applied to output and user will know what names are available to use.
A benefit from this design is that when entrypoints of a tool don't have self contained:
```
tool-name = test-cli
entrypoints = tester, testing
```
with this design it'd be:
```
$ uv tool list
tester (from test-cli)
testing (from test-cli)
```
with indentions:
```
$ uv tool list
test-cli
    tester
    testing
```

first one prioritizes entrypoints visually, and more looks good into my eyes.

---

_Comment by @zanieb on 2024-06-30 22:35_

I kind of like that, but it gets a little tricky with versions. 

Nit: If you've used `uv tool install` you shouldn't use `uv tool run`, they're added to your path and invoked directly. To run `blackd` with `uv tool run` you'd need `uv tool run --from black blackd`.

---

_Closed by @zanieb on 2024-07-03 04:54_

---
