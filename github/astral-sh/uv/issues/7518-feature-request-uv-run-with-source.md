---
number: 7518
title: "Feature Request: `uv run --with-source`"
type: issue
state: closed
author: ahal
labels:
  - question
assignees: []
created_at: 2024-09-18T20:59:51Z
updated_at: 2024-09-20T18:49:10Z
url: https://github.com/astral-sh/uv/issues/7518
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature Request: `uv run --with-source`

---

_Issue opened by @ahal on 2024-09-18 20:59_

This would be the opposite of the `--no-sources` flag, where we would temporarily resolve dependencies using a source that is *not* defined in `pyproject.toml`.

One use case for this is when you're working on something in one project, that's blocking some feature work in another project. Sometimes the upstream work needs to be tested in the downstream project. Currently you can accomplish this by temporarily adding a source that points at a local path or a pull request, e.g:
```
[tool.uv.sources]
foo = { path = "/path/to/my/wip" }
```

But it's all too easy to forget to remove this again and editing the config file twice just to run one command is a bit clunky. I'd love to see something like:
```
uv run --with-source "foo=path:/path/to/my/wip"
```
as a way to temporarily *add* a source. I think this would complement `--no-sources` nicely.  I don't have any strong preferences on the format of the argument.

---

_Comment by @zanieb on 2024-09-18 22:02_

We layer environments so I think this should just work with `uv run --with /path/to/my/wip ...`. Is that not the case for you?

---

_Label `question` added by @zanieb on 2024-09-18 22:02_

---

_Comment by @ahal on 2024-09-20 18:49_

Thanks, I got tunnel visioned in on the sources config that it didn't occur to me to check `--with`. This works great!

---

_Closed by @ahal on 2024-09-20 18:49_

---
