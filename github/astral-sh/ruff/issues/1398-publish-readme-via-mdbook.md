---
number: 1398
title: Publish README via mdBook
type: issue
state: closed
author: ischaojie
labels:
  - documentation
assignees: []
created_at: 2022-12-27T03:41:23Z
updated_at: 2023-01-28T15:34:15Z
url: https://github.com/astral-sh/ruff/issues/1398
synced_at: 2026-01-07T13:12:14-06:00
---

# Publish README via mdBook

---

_Issue opened by @ischaojie on 2022-12-27 03:41_

Currently, the README file so long to tired to read, Maybe can create specialized docs' Dir (Using mkdocs or some others).

---

_Comment by @charliermarsh on 2022-12-27 13:29_

Just publishes their README via mdBook: https://just.systems/man/en/. I think this alone could be useful for us maybe. And would probably only take an hour or so to hook up...

See: https://github.com/casey/just/blob/master/bin/generate-book/src/main.rs.


---

_Label `documentation` added by @charliermarsh on 2022-12-27 13:29_

---

_Renamed from "Consider adding specialized docs for usage intro" to "Publish README via mdBook" by @charliermarsh on 2022-12-27 13:29_

---

_Comment by @ischaojie on 2022-12-28 03:50_

Woo, that is amazing. But I worry that as more and more instructions are added to the README file, it will become difficult to maintain, although it is easy to generate a book.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-30 21:04_

---

_Referenced in [astral-sh/ruff#1467](../../astral-sh/ruff/issues/1467.md) on 2022-12-31 18:23_

---

_Referenced in [astral-sh/ruff#2244](../../astral-sh/ruff/pulls/2244.md) on 2023-01-27 07:46_

---

_Comment by @ischaojie on 2023-01-28 05:19_

I notice that `ruff` intends to use `mkdocs` to build the documentation, but it looks like it just moves the entire README file to the `docs` folder. For a better reading experience, it would be nice to have a script that splits the README into multiple chapter files (or a one-off, with subsequent updates made directly under docs). If possible I would create a pull request for this purpose.

---

_Comment by @charliermarsh on 2023-01-28 12:24_

Yeah I'm gonna split it up.

---

_Comment by @charliermarsh on 2023-01-28 12:25_

It'll come soon :)

---

_Comment by @charliermarsh on 2023-01-28 15:34_

We're now live at https://beta.ruff.rs/docs/.

---

_Closed by @charliermarsh on 2023-01-28 15:34_

---
