---
number: 118
title: Support and Integration for VSCode
type: issue
state: closed
author: harshalchaudhari35
labels: []
assignees: []
created_at: 2022-09-07T09:27:14Z
updated_at: 2023-12-12T05:37:30Z
url: https://github.com/astral-sh/ruff/issues/118
synced_at: 2026-01-07T13:12:14-06:00
---

# Support and Integration for VSCode

---

_Issue opened by @harshalchaudhari35 on 2022-09-07 09:27_

Is there a way to add ruff as a python linter in VScode? 

---

_Label `enhancement` added by @charliermarsh on 2022-09-07 14:14_

---

_Referenced in [astral-sh/ruff#181](../../astral-sh/ruff/issues/181.md) on 2022-09-13 19:51_

---

_Comment by @Jackenmen on 2022-09-13 23:10_

If the maintainers decide that editor integration is in scope for this project rather than separate dedicated projects, I would like to suggest using Language Server Protocol for this to allow usage with many editors at once (editors that support it include: Visual Studio, Visual Studio Code, Neovim, Sublime Text, JetBrains Fleet, and more):
https://microsoft.github.io/language-server-protocol/

---

_Comment by @HallerPatrick on 2022-09-15 18:21_

I think @jack1142 made a good suggestion here. I just researched a little, and integrating `ruff` as a linter in VSCode is not trivial. The best way is to create a PR for the official Python [extension](https://github.com/microsoft/vscode-python). It doesn't look too [complicated](https://github.com/microsoft/vscode-python), but I am also not sure if they are willing to support a new, not fully stable linter.

As a Neovim user, I am happier with a LSP, which is easily integrable. I might take a look at it.

---

_Comment by @Jackenmen on 2022-09-15 19:07_

VS Code Python team is actually pushing for this as well (LSP-based integrations, not necessarily the linter themselves implementing language server protocol themselves), they're working on extension templates and stuff like that to make it easy to create new integrations for linters with hopefully no TypeScript knowledge required (unlike the current solution which does require that). Considering that a lot of this is still being worked on, it might not make sense to work on VS Code specific extension just yet. LSP is generic enough that it's likely that not much will be needed to make a VS Code specific extension for it in the future.

Here are a bunch of related issues:
- https://github.com/psf/black/issues/2883
- https://github.com/PyCQA/isort/issues/1904
- https://github.com/PyCQA/pylint/issues/5796
- https://github.com/PyCQA/flake8/issues/1467

---

_Comment by @dinhanhx on 2022-09-21 04:45_

[harupy/vscode-ruff](https://github.com/harupy/vscode-ruff) and [microsoft/vscode-python Support Rust linter Ruff](https://github.com/microsoft/vscode-python/issues/19843
)


---

_Referenced in [astral-sh/ruff#271](../../astral-sh/ruff/issues/271.md) on 2022-09-28 21:39_

---

_Referenced in [astral-sh/ruff#655](../../astral-sh/ruff/pulls/655.md) on 2022-11-08 04:52_

---

_Closed by @charliermarsh on 2022-11-08 21:12_

---

_Comment by @charliermarsh on 2022-12-18 22:52_

Worth mentioning, for anyone that comes across this issue in the future, that we now have a dedicated [Ruff VS Code Extension](https://github.com/charliermarsh/vscode-ruff/).

---

_Comment by @CodeFanaticX on 2023-01-01 17:03_

> Worth mentioning, for anyone that comes across this issue in the future, that we now have a dedicated [Ruff VS Code Extension](https://github.com/charliermarsh/vscode-ruff/).

Hello. The vscode extension is not yet updated. I really want the fix for "f-string without any placeholders"

---

_Comment by @charliermarsh on 2023-01-02 00:09_

I cut a new pre-release extension with the latest Ruff version.

---

_Comment by @jaxwagner on 2023-12-12 01:44_

Is there a way to specify the version of ruff that is run in the vscode extension? specifically, it would be super helpful to be able to run version 0.1.6 or 0.1.7. Is this the only way to run one of these later versions https://github.com/astral-sh/ruff-vscode#using-a-custom-version-of-ruff ? thanks so much! :pray: 

---

_Comment by @zanieb on 2023-12-12 04:27_

@jaxwagner The extension will use your Python project's Ruff version if available. It will only fall back to the bundled version if it cannot find a compatible one in your environment.

---

_Comment by @jaxwagner on 2023-12-12 05:37_

thanks so much @zanieb ! super helpful really appreciate it :pray: 

---
