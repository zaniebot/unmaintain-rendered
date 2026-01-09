---
number: 1911
title: Why is the default line-length 88?
type: issue
state: closed
author: UtkarshVerma
labels: []
assignees: []
created_at: 2023-01-16T09:58:09Z
updated_at: 2023-01-16T16:28:44Z
url: https://github.com/astral-sh/ruff/issues/1911
synced_at: 2026-01-07T13:12:14-06:00
---

# Why is the default line-length 88?

---

_Issue opened by @UtkarshVerma on 2023-01-16 09:58_

PEP8 suggests a maximum line-length of 79 as stated on https://peps.python.org/pep-0008/#maximum-line-length. Therefore, I'm just curious as to why 88 is the default of line-length.

---

_Comment by @timotk on 2023-01-16 10:56_

Because Ruff [is designed to be compatible with Black](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html)

> Yes. Ruff is compatible with [Black](https://github.com/psf/black) out-of-the-box, as long as the line-length setting is consistent between the two.

> As a project, Ruff is designed to be used alongside Black and, as such, will defer implementing stylistic lint rules that are obviated by autoformatting.

You can read up on how Black does formatting [here](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html)

---

_Comment by @charliermarsh on 2023-01-16 16:28_

That's right! It's for compatibility with Black.

---

_Closed by @charliermarsh on 2023-01-16 16:28_

---

_Referenced in [scipy/scipy#18019](../../scipy/scipy/pulls/18019.md) on 2023-02-21 18:23_

---
