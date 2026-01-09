---
number: 15130
title: uv run --package not working as expected with workspaces
type: issue
state: closed
author: sebadevo
labels:
  - question
assignees: []
created_at: 2025-08-07T11:54:44Z
updated_at: 2025-08-09T13:58:45Z
url: https://github.com/astral-sh/uv/issues/15130
synced_at: 2026-01-07T13:12:19-06:00
---

# uv run --package not working as expected with workspaces

---

_Issue opened by @sebadevo on 2025-08-07 11:54_

### Summary

# Context
When executing the command `uv run` inside a project, the project environment will be created and updated before invoking the command ([docs](https://docs.astral.sh/uv/reference/cli/#uv-run:~:text=When%20used%20in%20a%20project%2C%20the%20project%20environment%20will%20be%20created%20and%20updated%20before%20invoking%20the%20command.)). as per your documentation. I understand this as the equivalent of running a uv sync if it wasn't synced before.

When doing it with a packages in a workspace, you have the possibility of running the same command if you are at the root and want to execute the root project. For example:
```bash
uv init --package albatros
uv run albatros # syncs the project because it wansn't done before
```

When defining workspace members inside a package project (such as albatros): 
```bash
uv init --package packages/bird-feeder
uv init --package packages/seeds
```
you can then run a package by 
```bash
uv sync --package seeds
```
which will sync the seeds environment from its pyproject.toml and run it using
```bash
uv run seeds
```
However, seeing the [docs](https://docs.astral.sh/uv/reference/cli/#uv-run--package:~:text=default%2Dgroups.-,%2D%2Dpackage%20package,workspace%20member%20does%20not%20exist%2C%20uv%20will%20exit%20with%20an%20error.,-%2D%2Dprerelease%20prerelease), I would have expected to be able to directly run the command: 
```bash
uv run --package seeds
```
which would sync the environment and run the script. But this doesn't work. Instead it returns the error: 
```txt
Installed 1 package in 3ms
Provide a command or script to invoke with `uv run <command>` or `uv run <script>.py`.

The following commands are available in the environment:

- albatros
- python
- python3
- python3.11
- seeds

See `uv run --help` for more information.
```
Even though the command failed we can see that it still installed the environment. 
I thus assume that the error comes from --package that doesn't work well with defined scripts in the pyproject.toml.

For example, in the albatros pyproject.toml we have the following script defined:
```toml
[project.scripts]
albatros = "albatros:main"
```
and running `uv run --package albatros` returns the same error mentionned above.

# How to reproduce
```bash
uv init --package albatros
```
```bash
cd albatros
```
```bash
uv init --package packages/bird-feeder
uv init --package packages/seeds
```
```bash
uv run --package seeds
```


### Platform

Linux 6.8.0-1029-azure x86_64 GNU/Linux

### Version

uv 0.8.5

### Python version

Python 3.11.12

---

_Label `bug` added by @sebadevo on 2025-08-07 11:54_

---

_Comment by @sebadevo on 2025-08-07 20:48_

I realised later on that the reason the command wasn't working is simply because it was missing the command to be executed. meaning that in the above example the fix is just to add a "second" seeds to the command.

```bash
uv run --package seeds seeds
```
The environment corresponding to the seeds package is first built, and then the seeds command is ran. So the initial issue I had is Solved.

Do you think it would be an expected feature that if no script command is passed it would try and find a script with the same name as the environment and try to run it? Or do you want this to still continue returning an error of missing script command ?

This would mean that in the above example, it would not be necessary to pass the second seeds argument to run its script, but if one would want to run the another script in that environment, they would be expected to pass that script name. 

---

_Comment by @zanieb on 2025-08-07 20:51_

> Do you think it would be an expected feature that if no script command is passed it would try and find a script with the same

This is intentionally not supported today.

I think it's possible we should have `uv run` execute a command with the same name as the package by default, but I'm not sure if it's worth the confusion it might add. e.g., then `uv run` might run `foo` but then should `uv run bar` run `foo bar` or `bar`? For example, the cargo ecosystem would run `foo bar`.

---

_Label `bug` removed by @zanieb on 2025-08-07 20:51_

---

_Label `question` added by @zanieb on 2025-08-07 20:51_

---

_Comment by @sebadevo on 2025-08-09 13:58_

Thank you for your response. Indeed, the confusion it might bring to behave like cargo might outweigh the benefits.

I will then close the issue.

---

_Closed by @sebadevo on 2025-08-09 13:58_

---
