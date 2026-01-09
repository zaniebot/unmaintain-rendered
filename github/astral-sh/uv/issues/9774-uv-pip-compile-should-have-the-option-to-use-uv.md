---
number: 9774
title: "`uv pip compile` should have the option to use `uv.lock` as constraints file"
type: issue
state: closed
author: fnep
labels:
  - needs-decision
assignees: []
created_at: 2024-12-10T14:44:06Z
updated_at: 2025-05-02T11:41:32Z
url: https://github.com/astral-sh/uv/issues/9774
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip compile` should have the option to use `uv.lock` as constraints file

---

_Issue opened by @fnep on 2024-12-10 14:44_

We use the `pyproject.toml` to maintain dependencies of our project (which is not a library, but a product using libraries). 

To install the project we use `uv sync` or `uv pip install -r pyproject.toml`, but the final product is rolled out on servers where `uv` is not available, and so we need to compile a set of `requirements.txt` files for different features / extras when we "bundle" all the files.

For this, we use `uv pip compile` with `pyproject.toml` as source file, but for this the `uv.lock` file is ignored, what makes it hard to maintain  "stable" `requirements.txt` files.

The current workaround is to use the output of `uv export` as input for `--constraints`, but this feels like a hack.

```bash
uv export --all-extras | uv pip compile ---constraints - --output-file requirements.txt pyproject.toml
``` 

Instead, please allow giving `uv.lock` as constraints file to `uv pip compile` to make this easier to use it as a single source of truth.

```bash
uv pip compile --constraints uv.lock --output-file requirements.txt pyproject.toml
```

---

_Comment by @zanieb on 2024-12-10 14:46_

Why does using `uv export` feel like a hack?

---

_Comment by @fnep on 2024-12-10 14:50_

Its not the `uv export` itself, but more that we have to pipe it into another uv command as input.

---

_Label `needs-decision` added by @zanieb on 2024-12-10 14:53_

---

_Comment by @zanieb on 2024-12-10 14:54_

Thanks 👍 

We need to decide if we want to support this. It has other implications, like probably `--requirements` support too for consistency. I'm a little hesitant since `export` exists for this purpose. 

---

_Renamed from "`uv pip compile` should have the option to use `uv.lock` as constraint file" to "`uv pip compile` should have the option to use `uv.lock` as constraints file" by @fnep on 2024-12-10 15:05_

---

_Comment by @fnep on 2024-12-10 15:10_

Thank you for considering it.

I know feeding the information generated using a tool into the same tool again is not uncommon in Unix tools. But it is not very intuitive to use and understand.

I'm not entirely sure how much sense it makes to give a `uv.lock` file to `--requirements`. What the practical use case would be. The file is in its meaning very close to the constraints option, but not to the requirements option. For pip they share the same format, so maybe there is one.

---

_Comment by @fnep on 2024-12-10 15:51_

Thinking about your comment, i assume i missed one very important detail. 

We do not need to run `uv pip compile` at all. The `uv export` with some options, will provide almost the same. There is no need for compile.

I think we can close this issue. Sorry for wasting your time.

---

_Closed by @fnep on 2024-12-10 16:22_

---

_Referenced in [astral-sh/uv#13245](../../astral-sh/uv/issues/13245.md) on 2025-05-02 11:39_

---

_Comment by @patrick-kidger on 2025-05-02 11:41_

I'd like to reopen this issue: I have a use for `uv pip compile foo bar --constraints uv.lock`, namely https://github.com/astral-sh/uv/issues/13245#issuecomment-2846861803 . This is useful as a way to ad-hoc include extra dependencies (e.g. a custom debugger for tackling a particular problem) without affecting the rest of the packages already available in an environment.

---
