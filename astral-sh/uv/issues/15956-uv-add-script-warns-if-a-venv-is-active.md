```yaml
number: 15956
title: "`uv add --script` warns if a venv is active"
type: issue
state: open
author: konstin
labels:
  - cli
assignees: []
created_at: 2025-09-19T21:14:28Z
updated_at: 2025-09-25T16:29:58Z
url: https://github.com/astral-sh/uv/issues/15956
synced_at: 2026-01-12T16:02:20Z
```

# `uv add --script` warns if a venv is active

---

_@konstin_

`uv add --script` shows a warning that we're not matching the script environment path, but we shouldn't activate that environment, we should `uv run` the script.

```console
$ uv venv -c -q -p 3.13
$ . .venv/bin/activate
$ uv init --script main.py
Initialized script at `main.py`
$ uv add --script main.py tqdm
warning: `VIRTUAL_ENV=.venv` does not match the script environment path `/home/konsti/.cache/uv/environments-v2/main-6ad169614700d74a` and will be ignored; use `--active` to target the active environment instead
```

We can show a note that the script is isolated from the currently active venv, but that doesn't need to be a warning.

---

_Label `bug` added by @konstin on 2025-09-19 21:14_

---

_Comment by @zanieb on 2025-09-20 02:17_

I'm not sure if I agree here. This is the same warning we show for a project environment. In both cases, we're not suggesting that people activate an environment, we're indicating that `VIRTUAL_ENV` is set but will not be mutated (as people have seemed to expect previously).

---

_Label `needs-decision` added by @zanieb on 2025-09-20 02:17_

---

_Comment by @konstin on 2025-09-20 11:53_

Should we be showing the path to script environment in the cache here? I know that IDEs will need them as venv for the script, but it doesn't seem like something that has an immediate connection to using `--script main.py`.

---

_Comment by @zanieb on 2025-09-20 17:49_

I think it's okay to show the path, though I'm not opposed to hiding it if you think it's confusing.

---

_Comment by @konstin on 2025-09-23 12:20_

I want to specialize the error message for scripts specifically. When I'm working on a project, the warning is usually a good reminder that I've activated the wrong venv, but if I'm working on a script in a project, it's generally fine if the project environment is different than the isolated script environment-

---

_Comment by @zanieb on 2025-09-23 17:03_

It's not warning you that the project environment is different, it's warning you that the activated environment is different. I'm not sure I understand? Could you say what you want the message to be?

---

_Comment by @konstin on 2025-09-25 10:14_

Say I'm in a project, I have the project's venv activated and now I'm writing a script that's isolated from the project itself, say something for deployment or building or code generation:

```
(.venv) ~/projects/foo $ uv init --script deploy.py
Initialized script at `deploy.py`
(.venv) ~/projects/foo $ uv add --script deploy.py httpx
warning: `VIRTUAL_ENV=.venv` does not match the script environment path `/home/konsti/.cache/uv/environments-v2/deploy-531da86ca5805d9e` and will be ignored; use `--active` to target the active environment instead
Updated `deploy.py`
```

In this case, I don't want to use the project environment, I'm intentionally using a separate script. For this case it's correct and intentional, so I'm surprised we're showing a warning with no option to turn it off when we're usually careful not to show non-actionable warnings (`--active` is what I don't want here, it breaks the separation why I'm using the script). I'd hide at least `/home/konsti/.cache/uv/environments-v2/deploy-531da86ca5805d9e` because that's an implementation detail.

---

_Comment by @zanieb on 2025-09-25 12:45_

Yeah but why are you activating the project environment? How can we know you have a strong enough grasp of our concepts to understand we won't reuse that activated environment for a script? What if it was some arbitrary environment instead?

The warning can be turned off, e.g., with `--no-active`, or by deactivating your environment — it's not inactionable.

---

_Comment by @konstin on 2025-09-25 16:17_

> Yeah but why are you activating the project environment? How can we know you have a strong enough grasp of our concepts to understand we won't reuse that activated environment for a script? What if it was some arbitrary environment instead?

Usually it's already activated by the IDE in the IDE terminal, PyCharm does it by default and it seems VS Code and Cursor too. There's also a lot of inertia in people activating the venv to run plain `python` in it. While I'd like to move more towards `uv run`, activating the venv is still very common.

When the active venv and the current project venv are mismatching, usually something has gone wrong, like you `cd`'d into a different project with the previous venv still active, and now `python` and `uv pip install` don't use the venv you'd expect it to use. For PEP 723 scripts, not using the active venv and instead hiding the isolated environment is a feature, and it feels strange that this creates a warning.

The warning feels inconsistent, as `uv add --script deploy.py` warns about this, but `uv run deploy.py` does not, even though both use the same isolated environment:

```console
$ uv add --script deploy.py httpx
warning: `VIRTUAL_ENV=.venv` does not match the script environment path `/home/konsti/.cache/uv/environments-v2/deploy-531da86ca5805d9e` and will be ignored; use `--active` to target the active environment instead
Updated `deploy.py`
```

```console
$ uv run deploy.py 
Hello from deploy.py!
```

Unlike `uv run` in a project, which does warn:

```console
$ cd /other/project
$ . .venv/bin/activate
$ cd -
$ uv run python
warning: `VIRTUAL_ENV=/other/project/.venv` does not match the project environment path `.venv` and will be ignored; use `--active` to target the active environment instead
Python 3.13.2 (main, Mar 17 2025, 21:02:54) [Clang 20.1.0 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

> The warning can be turned off, e.g., with `--no-active`, or by deactivating your environment — it's not inactionable.

Usually we have "[...] to disable this warning." in these messages, e.g. for preview features and , or instructions such as "set `extra-index-url` in a `uv.toml` or `pyproject.toml` file." or "use `--link-mode=copy` to suppress this warning." in the message itself.

---

_Comment by @zanieb on 2025-09-25 16:27_

> Usually we have "[...] to disable this warning." in these messages, 

That just hasn't happened yet because it's hard to succinctly explain, but feel free to do so...

---

_Comment by @zanieb on 2025-09-25 16:29_

We can just remove the warning entirely for scripts and consider adding something again if we get feedback that there's confusion.

---

_Label `bug` removed by @zanieb on 2025-09-25 16:29_

---

_Label `needs-decision` removed by @zanieb on 2025-09-25 16:29_

---

_Label `cli` added by @zanieb on 2025-09-25 16:29_

---
