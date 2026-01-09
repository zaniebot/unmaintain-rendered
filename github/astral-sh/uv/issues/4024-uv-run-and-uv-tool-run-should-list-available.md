---
number: 4024
title: "`uv run` and `uv tool run` should list available scripts and executables"
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - help wanted
assignees: []
created_at: 2024-06-04T21:50:08Z
updated_at: 2024-10-08T19:34:51Z
url: https://github.com/astral-sh/uv/issues/4024
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv run` and `uv tool run` should list available scripts and executables

---

_Issue opened by @charliermarsh on 2024-06-04 21:50_

Rye has this neat feature: https://rye.astral.sh/guide/commands/run/

---

_Label `preview` added by @charliermarsh on 2024-06-04 21:50_

---

_Comment by @Ben-Epstein on 2024-06-10 01:30_

@charliermarsh what is the difference between `uv run` and `uv tool run`? I can't seem to find it in the docs, and they seem to have different behavior.

---

_Comment by @charliermarsh on 2024-06-10 01:34_

Neither is documented right now nor intended to be used -- they're both under active development. But `uv run` is "run a command within this project's environment", while `uv tool run` is "run a globally-installed tool" (similar to `pipx`).

---

_Comment by @zanieb on 2024-06-10 02:39_

`uv tool run` will also run a tool in an ephemeral environment, it's like `pipx run` or `npx`.

---

_Comment by @charliermarsh on 2024-07-09 15:57_

I actually don't know if we need this, we have `uv tool list`. Do we want `uv tool run` to fall back to this?

---

_Comment by @zanieb on 2024-07-09 16:01_

It probably makes more sense than starting a `python` REPL (which I think we should remove?)

---

_Comment by @charliermarsh on 2024-07-09 16:02_

I think `uv tool run` and `uv run` just show help.

---

_Label `needs-design` added by @konstin on 2024-07-09 16:54_

---

_Referenced in [astral-sh/uv#5184](../../astral-sh/uv/issues/5184.md) on 2024-07-18 13:57_

---

_Comment by @charliermarsh on 2024-07-19 23:37_

I think `uv tool run` should list the available executables, to start. That seems straightforward.

Not sure about `uv run`, it's a little less obvious what should happen there, since (unlike Rye) we don't have "scripts". We could list entrypoints, but, IDK.

---

_Label `needs-design` removed by @charliermarsh on 2024-07-19 23:37_

---

_Label `help wanted` added by @charliermarsh on 2024-07-19 23:37_

---

_Comment by @charliermarsh on 2024-07-19 23:38_

The `uv tool run` part should be doable now, at least.

---

_Label `good first issue` added by @charliermarsh on 2024-07-19 23:38_

---

_Referenced in [astral-sh/uv#5553](../../astral-sh/uv/pulls/5553.md) on 2024-07-29 08:16_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Comment by @zanieb on 2024-08-28 15:26_

Gosh, I'd sure love this. The error message here, e.g., when I haven't installed `django` yet is not helpful:

```

❯ uv run django-admin startproject mysite
Using Python 3.11.7
Creating virtualenv at: .venv
error: Failed to spawn: `django-admin`
  Caused by: No such file or directory (os error 2)
```

I think we can at least list things in the virtual environment bin?

---

_Comment by @charliermarsh on 2024-08-28 15:29_

Agreed. Let's start with that. Help wanted!

---

_Comment by @CharlesB2 on 2024-09-06 09:01_

Looks like this got implemented as fallback to `uv tool list` when `uv tool run` has no arg, but I find it really unintuitive
```
$ uv --version
uv 0.4.6 (84f25e8cf 2024-09-05)
$ uv tool run
pytest v8.3.2
- py.test
- pytest
```
At least add a message before like `you didn't specify any command to run, here are the ones you have`

---

_Comment by @Aditya-PS-05 on 2024-09-13 10:48_

Hello team, 
I am willing to contribute to this issue, 
According to me:
1.) ```uv tool run``` should list all the globally installed tools 
2.) Not sure about the ```uv run``` command and what is the entry-points you discussed earlier?

Can you assign this task to me?

---

_Comment by @zanieb on 2024-09-13 13:29_

`uv tool run` already does this.

We're not sure what we we want to do for `uv run`, but I think we'd want to either list things in `.venv/bin` or the entrypoints of all the packages in the environment.

https://packaging.python.org/en/latest/specifications/entry-points/

https://github.com/astral-sh/uv/blob/4f2349119cf341eedf738d06a50ed136a5f207db/crates/uv/src/commands/tool/run.rs#L244-L258

---

_Referenced in [astral-sh/uv#7641](../../astral-sh/uv/pulls/7641.md) on 2024-09-25 11:40_

---

_Referenced in [astral-sh/uv#7687](../../astral-sh/uv/pulls/7687.md) on 2024-09-25 15:46_

---

_Closed by @zanieb on 2024-10-08 19:34_

---
