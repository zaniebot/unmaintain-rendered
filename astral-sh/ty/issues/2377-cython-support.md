```yaml
number: 2377
title: Cython support
type: issue
state: open
author: sr0lle
labels:
  - wish
assignees: []
created_at: 2026-01-07T15:44:08Z
updated_at: 2026-01-12T10:39:20Z
url: https://github.com/astral-sh/ty/issues/2377
synced_at: 2026-01-12T15:54:26Z
```

# Cython support

---

_@sr0lle_

Just like [the related issue in the ruff repo](https://github.com/astral-sh/ruff/issues/10250), I wanted to ask if you all would consider adding support for [Cython](https://github.com/cython/cython). Writing Cython is a common way for Python developers to write high performance code that compares to C speeds by typing the code properly, without the need to write in two languages like similar projects C++ & nanbind/pybind or Rust & pyo3.

Unfortunately, Cython doesn't have a proper linter or language server, as far as I know, making it quite challenging to write Cython code as projects grow. Since this issue is inside the ty repo, I'd like to focus on the latter point: having a language server like ty that also offers features like auto-completions for Cython would really enhance productivity.

---

_Label `wish` added by @MichaReiser on 2026-01-07 15:45_

---

_Comment by @asukaminato0721 on 2026-01-08 15:24_

> Unfortunately, Cython doesn't have a proper linter or language server, as far as I know

https://github.com/ktnrg45/cyright

---

_Comment by @sr0lle on 2026-01-12 10:39_

> > Unfortunately, Cython doesn't have a proper linter or language server, as far as I know
> 
> https://github.com/ktnrg45/cyright

Thanks, but this is only a linter, not a language server as this issue is about. Plus, the project has been abandoned for two years.

---
