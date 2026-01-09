---
number: 1845
title: "feature: Allow uv pip compile to take input from CLI"
type: issue
state: closed
author: gaborbernat
labels:
  - question
assignees: []
created_at: 2024-02-22T01:56:29Z
updated_at: 2024-06-19T00:37:52Z
url: https://github.com/astral-sh/uv/issues/1845
synced_at: 2026-01-07T13:12:16-06:00
---

# feature: Allow uv pip compile to take input from CLI

---

_Issue opened by @gaborbernat on 2024-02-22 01:56_

I would appreciate it if we could get the resolved packages list without needing to install them into a virtual environment. For example it would be super helpful to be able to do:

```bash
uv resolve -p 3.8 sphinx tox
```

that would equal to:

```bash
rm .venv -rf
uv venv -p 3.8
uv pip install sphinx tox
uv pip freeze
rm .venv -rf
```

---

_Comment by @charliermarsh on 2024-02-22 02:00_

Is this different than `uv pip compile`?

---

_Comment by @gaborbernat on 2024-02-22 02:09_

I guess that is the same would one invoke, but without requiring a `requirements.in` file 🤔 

```
uv pip compile -p 3.8 requirements.in --no-annotate --no-header
```

Can pip compile get then a flag to take the input from the CLI? 😆 

---

_Comment by @zanieb on 2024-02-22 02:09_

You can do `echo "foo" | uv pip compile -`

---

_Label `question` added by @zanieb on 2024-02-22 02:10_

---

_Comment by @gaborbernat on 2024-02-22 02:13_

```
❯ printf 'tox\nsphinx' | uv pip compile -p 3.8 --no-annotate --no-header -
Resolved 35 packages in 188ms
```

This does mean though that we're using here a Unix only construct. Perhaps would be more cross-platform if we could do:


`uv pip compile -p 3.8 --no-annotate --no-header --raw 'tox\nsphinx'` instead?

---

_Renamed from "feature: Add uv resolve command" to "feature: Allow uv pip compile to take input from CLI" by @gaborbernat on 2024-02-22 02:16_

---

_Comment by @zanieb on 2024-06-19 00:37_

It seems unlikely we'll add another option for direct input to `pip compile` unless we get a lot of feedback that piping is insufficient — I think it's not common enough of a use case to maintain.

---

_Closed by @zanieb on 2024-06-19 00:37_

---
