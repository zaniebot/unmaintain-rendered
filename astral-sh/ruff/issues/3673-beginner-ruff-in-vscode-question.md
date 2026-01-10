```yaml
number: 3673
title: Beginner ruff in vscode question
type: issue
state: closed
author: dlasusa
labels:
  - question
assignees: []
created_at: 2023-03-22T19:46:50Z
updated_at: 2023-03-22T22:46:59Z
url: https://github.com/astral-sh/ruff/issues/3673
synced_at: 2026-01-10T11:09:46Z
```

# Beginner ruff in vscode question

---

_Issue opened by @dlasusa on 2023-03-22 19:46_

Sorry for such a basic question, but I've always used the tools built into vscode.
TLDR: Are there any basic tutorials on how to use ruff inside of VSC?

I recently installed the Ruff extension but I'm not sure how to configure it.  Currently, I'm mostly interested in ignoring the line length warning.

I created a `ruff.toml` file in the root of my workspace/folder and added:

 ```toml
[tool.ruff]
ignore = ["E501"]
```
https://beta.ruff.rs/docs/rules/line-too-long/

But now I don't have any detected issues (and I know I have some)

Are there any basic tutorials on how to use Ruff inside of VSC?

Thanks!
Dan

---

_Comment by @charliermarsh on 2023-03-22 20:01_

Unfortunately I don't know that we have a great _tutorial_ beyond the tutorial in the [docs](https://beta.ruff.rs/docs/tutorial/), which is mostly focused on command-line usage. But I'm happy to answer questions here or in Discord.

In this case, I _think_ what you're experiencing is that if you use `ruff.toml` (instead of `pyproject.toml`), you should omit the `[tool.ruff]`, so your entire file would be:

```toml
ignore = ["E501"]
```

So, Ruff is probably trying to read that file and failing, and the error is being hidden in the VS Code logs.


---

_Label `question` added by @charliermarsh on 2023-03-22 20:01_

---

_Comment by @dlasusa on 2023-03-22 20:07_

Ahhh...Bingo!  That was it.  I'm still learning how to use the different .toml files.  

Thanks so much for the quick reply!

---

_Closed by @dlasusa on 2023-03-22 20:07_

---

_Comment by @charliermarsh on 2023-03-22 20:09_

No worries! `ruff.toml` is basically a unique thing within Ruff, and exists to support those that don't want to or can't adopt a `pyproject.toml` file (which is a standard configuration file in the Python ecosystem, but introducing it has other implications especially for existing projects that use `setup.py` or other systems).

---

_Comment by @dlasusa on 2023-03-22 20:12_

Good to know.  Most of my code is one-off to automate tasks for our staff members, so I've had learning more about `pyproject.toml` on my todo list, might be time to move it up the list and incorporate my ruff config in there.  Thanks for a great tool! 

---

_Comment by @MichaReiser on 2023-03-22 22:33_

Should we add a warning if we find any ruff. Setting in a ruff.toml?

---

_Comment by @charliermarsh on 2023-03-22 22:46_

Yeah we should. We should also figure out how to surface warnings within the extension. Right now, I think it just fails silently.

---
