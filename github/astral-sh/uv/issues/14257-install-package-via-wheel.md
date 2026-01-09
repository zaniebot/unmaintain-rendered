---
number: 14257
title: Install package via wheel
type: issue
state: closed
author: GitRon
labels:
  - question
assignees: []
created_at: 2025-06-25T10:25:16Z
updated_at: 2025-07-08T19:19:55Z
url: https://github.com/astral-sh/uv/issues/14257
synced_at: 2026-01-07T13:12:18-06:00
---

# Install package via wheel

---

_Issue opened by @GitRon on 2025-06-25 10:25_

### Question

Hi there!

I need to install a uv lockfile which contains `python-ldap` which I have to install manually via a wheel file on windows. I checked the docs but couldn't find a way of telling this to uv.

When I run `uv sync --frozen`, the setup fails and that's it.

Anything that I can do differently?

Thx in advance ❤ 

### Platform

Windows 11

### Version

0.7.6

---

_Label `question` added by @GitRon on 2025-06-25 10:25_

---

_Comment by @charliermarsh on 2025-06-25 16:39_

Can you share the `pyproject.toml`? Typically, you'd add something like:

```toml
[tool.uv.sources]
python-ldap = { path = "./path/to/wheel" }
```

---

_Comment by @GitRon on 2025-06-27 06:37_

Hi @charliermarsh 

thx for the fast reply! ❤ 

That's a good idea, I'll try this out. My - and probably others, too - use case is that I have a unix based Docker image but I want to install the packages locally, too for performance and simplicity reasons. In other package managers this package will just be ignored and I can install it manually.

Here, the process fails and I can't continue. Can you think of a way of making my edge case work?

Thx!

---

_Comment by @GitRon on 2025-07-02 08:11_

````toml
[tool.uv.sources]
python-ldap = { path = "./path/to/wheel" }
````

Works like a charm! Would be awesome to have some kind of condition so I can commit it but still: It works. 

---

_Comment by @konstin on 2025-07-02 16:21_

If you have a directory for vendored source distributions and wheels, you can use a flat index (https://docs.astral.sh/uv/concepts/indexes/#flat-indexes). 

A feature that may more directly solve your problem is that you can limit sources to a specific platform (https://docs.astral.sh/uv/concepts/projects/dependencies/#platform-specific-sources)

---

_Closed by @charliermarsh on 2025-07-08 19:19_

---
