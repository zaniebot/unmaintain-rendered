---
number: 4173
title: "How to use different index url with `pyproject.toml` when using `uv pip compile`"
type: issue
state: closed
author: SamComber
labels:
  - question
assignees: []
created_at: 2024-06-09T17:43:22Z
updated_at: 2024-06-24T22:26:59Z
url: https://github.com/astral-sh/uv/issues/4173
synced_at: 2026-01-10T01:23:35Z
---

# How to use different index url with `pyproject.toml` when using `uv pip compile`

---

_Issue opened by @SamComber on 2024-06-09 17:43_

I'm trying to convert my Pipfile to a pyproject.toml, so that I can run:

`uv pip compile pyproject.toml --generate-hashes -o requirements-lock.txt`

The issue is I have a package, torch, that uses a different index in my Pipfile

```
[dev-packages]
torch = { index = "pytorch_cpu"}

[[source]]
url = "https://download.pytorch.org/whl/cpu"
verify_ssl = true
name = "pytorch_cpu"
```

How can I reproduce what my Pipfile is achieving here, and specify a different index url to install `torch` here? Ideally I'd like to keep this all contained within `pyproject.toml` - is this possible?

P.S. loving the blazing speed of `uv`!

---

_Comment by @charliermarsh on 2024-06-09 17:45_

You should be able to do:

```toml
[tool.uv.pip]
extra-index-url = ["https://download.pytorch.org/whl/cpu"]
```

...in your `pyproject.toml`. We don't support assigning a package to a specific index yet but it's on the roadmap (#171).


---

_Label `question` added by @charliermarsh on 2024-06-09 17:51_

---

_Comment by @charliermarsh on 2024-06-09 17:52_

If you need Torch to be an optional dependency, we don't yet support development dependencies like that though it's coming soon (and already supported in `preview` mode but not everywhere you'd need it). So you would need to use extras, like:

```toml
[project.optional-dependencies]
dev = ["torch"]
```

And then `uv pip compile pyproject.toml --extra dev --generate-hashes -o requirements-lock.txt`.

---

_Comment by @SamComber on 2024-06-10 08:45_

Thank you @charliermarsh, will give this a whirl.

One last blocker if you would care to help me out..

Would you know how to install your project in editable mode? Is this possible? 

```
[dev-packages]
my_project = {editable = true, path = "."}
```

I've been trying

```
[dev-packages]
"my_project@file:///Users/sam.comber/Documents/kraken/my_project"
```

`uv pip compile pyproject.toml --extra dev --generate-hashes -o requirements-lock.txt`

But I can't see the project as editable inside the `requirements-lock.txt?`

`uv pip install --compile-bytecode -r requirements.lock -e .` works for me, but i need my project inside the lock file, otherwise `uv pip sync requirements.lock` will remove it and ill need to reinstall each time. Is this possible?


---

_Comment by @patrick-kidger on 2024-06-12 18:40_

I can answer this one. You want `printf '-e .' | uv pip sync - requirements.lock`.

---

_Comment by @charliermarsh on 2024-06-24 22:26_

Thanks @patrick-kidger :)

---

_Closed by @charliermarsh on 2024-06-24 22:26_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-24 22:26_

---

_Referenced in [astral-sh/uv#15132](../../astral-sh/uv/issues/15132.md) on 2025-08-09 13:48_

---
