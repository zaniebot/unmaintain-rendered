---
number: 9820
title: "[Feature Request] Static type checker and language server à la Pyright"
type: issue
state: closed
author: matthewlloyd
labels: []
assignees: []
created_at: 2024-02-05T01:37:31Z
updated_at: 2024-02-10T16:16:32Z
url: https://github.com/astral-sh/ruff/issues/9820
synced_at: 2026-01-07T13:12:15-06:00
---

# [Feature Request] Static type checker and language server à la Pyright

---

_Issue opened by @matthewlloyd on 2024-02-05 01:37_

Ruff subsumed Python linters improving upon their performance by more than an order of magnitude. Amazing.

Then, the Ruff team subsumed Black and isort, again revolutionizing Python developer tools, giving us a fast, well maintained, high quality alternative with excellent documentation. Awesome!

There is just one more tool left in my Python workflow: Pyright. Since introducing static typing to my Python repo, it has become an essential sidekick, with the LSP making for an awesome interactive development experience as part of the VS Code extension, and the command-line static type checker has become an integral part of my pre-commit hook.

There is just one small problem with Pyright: it's not as fast as Ruff! A full Pyright run takes 15.430s on the 60K lines of Python in one of my repos. That can be a long time to wait when committing changes. Ruff check takes 0.068s, and Ruff format takes 0.078s.

This feature request is therefore for the Ruff team to do the obvious thing and implement static type checking to subsume the functionality of Pyright and other similar tools. That would make Ruff a true, all-in-one, complete tool for Python linting, formatting, and type checking - a one-stop shop for writing high quality Python code and the only tool you need in your pre-commit.

---

_Comment by @qarmin on 2024-02-05 07:05_

Duplicate of https://github.com/astral-sh/ruff/issues/3893


---

_Renamed from "[Feature Request] Static type checker and language server" to "[Feature Request] Static type checker and language server à la Pyright" by @matthewlloyd on 2024-02-05 17:56_

---

_Comment by @zanieb on 2024-02-10 16:16_

Tracking in https://github.com/astral-sh/ruff/issues/3893

---

_Closed by @zanieb on 2024-02-10 16:16_

---
