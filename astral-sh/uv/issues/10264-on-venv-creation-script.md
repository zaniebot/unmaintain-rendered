```yaml
number: 10264
title: "\"on venv creation\" script"
type: issue
state: open
author: majidaldo
labels: []
assignees: []
created_at: 2025-01-01T17:25:02Z
updated_at: 2025-06-11T23:17:10Z
url: https://github.com/astral-sh/uv/issues/10264
synced_at: 2026-01-12T16:00:09Z
```

# "on venv creation" script

---

_@majidaldo_

close to https://github.com/astral-sh/uv/issues/10211.

Use cases: `pre-commit install` and environment variable setting.

---

_Comment by @zanieb on 2025-01-01 17:33_

On activate as in `source .venv/bin/activate` or during `uv run`? I think I need more details here.

---

_Renamed from ""on venv activate" script" to ""on venv creation" script" by @majidaldo on 2025-01-02 02:31_

---

_Comment by @majidaldo on 2025-01-02 02:35_

> On activate as in `source .venv/bin/activate` or during `uv run`? I think I need more details here.

errr sorry changed the issue title. `uv run` variations can be handled but there is no 'hook' to do something on env creation with the driving use case being a place to put `pre-commit install`.

---

_Comment by @sayandipdutta on 2025-01-02 15:45_

+1
Such a `on venv creation` hook will also let me set the `venv --prompt` to something other than `.venv`.

---

_Comment by @brunomjm on 2025-02-06 15:52_

+1
If I may rephrase: a way to include custom actions in the `uv sync` process (my use case is also `pre-commit install`).
Benefits:
- Less boilerplate / manual steps until a developer can start working on a fresh clone or after a pull where pre-commits where changed,
- Less risk of developers bypassing or forgetting about required pre-commit hooks in a project.

@majidaldo maybe mention `uv sync` in the title?

---

_Comment by @majidaldo on 2025-02-06 17:24_

> +1 If I may rephrase: a way to include custom actions in the `uv sync` process (my use case is also `pre-commit install`). Benefits:
> 
> * Less boilerplate / manual steps until a developer can start working on a fresh clone or after a pull where pre-commits where changed,
> * Less risk of developers bypassing or forgetting about required pre-commit hooks in a project.
> 
> [@majidaldo](https://github.com/majidaldo) maybe mention `uv sync` in the title?

my suggestion isn't on `uv sync` but on venv activation though. let `uv sync` just be about creating the env (not activating it).

---

_Comment by @zanieb on 2025-02-06 19:53_

> Such a on venv creation hook will also let me set the venv --prompt to something other than .venv.

Why not just set the prompt when you create the venv? Is this when the venv is created via a specific command or.. ?

---

_Comment by @sayandipdutta on 2025-02-07 03:53_

No, not using the explicit command. For things like `uv sync`, `uv run`, nd `uv add`. I am thinking more along the lines of a hook.

---

_Comment by @zanieb on 2025-02-07 15:58_

But... with those commands the `--prompt` should be the name of the directory / project not `.venv`?

---

_Comment by @sayandipdutta on 2025-02-07 17:39_

Well... I checked, and this is embarrassing 😅

Either this wasn't the default behavior in some previous version, or I had messed up somewhere. I remember scouring the issues for such a request. And I ended up writing a custom script to do this. Although I have been seeing the correct prompt ever since, I figured this was solely due to my script, and didn't bother to check thoroughly.

Sorry for the trouble! Thanks a lot!

---

_Comment by @zanieb on 2025-02-07 17:41_

Haha thanks for following up, that's helpful. Just trying to understand the use-cases :)

---

_Comment by @majidaldo on 2025-02-07 18:14_

since we're developing python, i can get something functionally close if i hook stuff into a `__init__.py`, but such code shouldn't really be in distributed code:
```python
if dev(): # figure out if in dev mode
   # setup steps
   ...

```
something more involved would be to hook into a build script for some 'dev' package.

---

_Comment by @brunomjm on 2025-02-14 14:20_

> my suggestion isn't on `uv sync` but on venv activation though. let `uv sync` just be about creating the env (not activating it).

Not sure I understand, since uv abstracts away venv activation no? But I don't wanna hijack your issue 😄 Maybe I will create a separate one.

@zanieb I would be curious to hear your opinion on the intent behind `uv sync` though: is it strictly meant for syncing dependencies in a virtual environment or could it be extended to a broader definition of "environment" (i.e. prepare the environment for development) which could include installing pre-commit hooks?

---

_Comment by @majidaldo on 2025-02-14 19:43_

> > my suggestion isn't on `uv sync` but on venv activation though. let `uv sync` just be about creating the env (not activating it).
> 
> Not sure I understand, since uv abstracts away venv activation no? But I don't wanna hijack your issue 😄 Maybe I will create a separate one.
> 
> [@zanieb](https://github.com/zanieb) I would be curious to hear your opinion on the intent behind `uv sync` though: is it strictly meant for syncing dependencies in a virtual environment or could it be extended to a broader definition of "environment" (i.e. prepare the environment for development) which could include installing pre-commit hooks?

that would be `uv run`.

---

_Comment by @Avasam on 2025-06-11 23:17_

On venv creation is close enough to a pre-install script that I could use it to automatically install os-level requirements and configs. Replacing https://github.com/Toufool/AutoSplit/blob/9d1104b07e6b6d87e42ea7b386255badfdf51851/scripts/install.ps1 with only `uv sync`/`uv run`. At least until if/when https://peps.python.org/pep-0725/ ever lands.

---
