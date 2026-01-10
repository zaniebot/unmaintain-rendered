---
number: 10543
title: "How to use the parent directory `venv` within subfolders?"
type: issue
state: open
author: badlydrawnrob
labels:
  - question
assignees: []
created_at: 2025-01-12T17:39:48Z
updated_at: 2025-01-19T13:29:43Z
url: https://github.com/astral-sh/uv/issues/10543
synced_at: 2026-01-10T01:24:54Z
---

# How to use the parent directory `venv` within subfolders?

---

_Issue opened by @badlydrawnrob on 2025-01-12 17:39_

You can call a subfolders file with `uv run uvicorn src.main:app --reload` from the parent directory (the `venv` is stored in this parent directory). How might you run the virtual environment `uv` command from within the subfolder itself? Is that even possible?

If I try that I get the error:

```
warning: `VIRTUAL_ENV=/Users/rob/project/build-fast-api` does not match the project environment path `/Users/rob/project/.venv` and will be ignored
error: No interpreter found for executable name `build-fast-api` in managed installations or search path
```

---

_Comment by @zanieb on 2025-01-12 18:12_

It looks like you have `UV_PYTHON` or the `.python-version` file set to `build-fast-api`? Is there a reason you're changing the environment name to something other than `.venv`? Perhaps that's just the case in your subfolder?

---

_Label `question` added by @zanieb on 2025-01-12 18:12_

---

_Comment by @badlydrawnrob on 2025-01-12 18:41_

@zanieb I set it up with `uv venv build-fast-api` for a named virtual environment. The parent directory has that `.python-version` file (which I'm assuming tells `uv add` to add packages to the correct virtual env). I did try to add a version file to subdirectories to see if it'd allow me to run `uv run` within child folders. That didn't work.

A couple of gotchas I had to double check:

1. Make sure `. "$HOME/.local/bin/env"` is in your `.zshrc` file (Mac) — seems like `uv` added this automatically
2. Make sure you've `source venv-name/bin/activate` from the _parent_ directory

For instance, if I have a file `hello.py` in a subfolder `chapter_01` when I've got `(build-fast-api)` virtualenv running ...

```python
def main():
    print("Hello from building-with-fast-api!")


if __name__ == "__main__":
    main()
```

 And try to `uv run hello.py` from the subfolder (when virtual env is in parent folder) I get this error:

```
warning: `VIRTUAL_ENV=/Users/rob/project/build-fast-api` does not match the project environment path `/Users/rob/project/.venv` and will be ignored
error: No interpreter found for executable name `build-fast-api` in managed installations or search path
```

If I try to run from parent directory with `uv run chapter_01.hello.py` I get this error:

```
error: Failed to spawn: `chapter_01.hello.py`
  Caused by: No such file or directory (os error 2)
```

So I don't know what I'm doing wrong. Deactivating and running `python3 -m chapter_01.hello` works fine. So perhaps it's something to do with global setup or the virtual environment.

#### Running `uv run hello.py` file when in root folder

When I'm running the command with a file in the root folder, it works, but I still get that warning:

```
(build-fast-api) rob@MacBook-Pro building-with-fast-api % uv run hello.py
warning: `VIRTUAL_ENV=build-fast-api` does not match the project environment path `.venv` and will be ignored
Using CPython 3.13.1 interpreter at: build-fast-api/bin/python3
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Installed 38 packages in 284ms
Hello from building-with-fast-api!
```

#### Other package managers etc

`npm run [script]` will run from any subfolder. I expected the same from `uv` but perhaps it's my misunderstanding of how things are done in Python?

---

_Comment by @zanieb on 2025-01-12 21:54_

> The parent directory has that .python-version file (which I'm assuming tells uv add to add packages to the correct virtual env). I did try to add a version file to subdirectories to see if it'd allow me to run uv run within child folders

Ah this sounds like a possible bug where we don't resolve the directory relative to the `.python-version` file, e.g.,

```
❯ uv venv foo
❯ uv python pin foo
Updated `.python-version` from `3.12` -> `foo`
❯ uv python find
/Users/zb/workspace/uv/example/foo/bin/python3
❯ mkdir bar
❯ cd bar
❯ uv python find
error: No interpreter found for executable name `foo` in virtual environments, managed installations, or search path
```

---

_Comment by @zanieb on 2025-01-12 21:55_

I can look into fixing that, but... I would not recommend using directory names in `.python-version` files or using a custom virtual environment name in a project.

---

_Referenced in [astral-sh/uv#10548](../../astral-sh/uv/issues/10548.md) on 2025-01-12 21:56_

---

_Comment by @badlydrawnrob on 2025-01-13 14:37_

@zanieb Yeah that could be it.

The last time I used Python was about 9 years ago, and at that point [Pyenv](https://github.com/pyenv/pyenv) and [Pyenv Virtual Env](https://github.com/pyenv/pyenv-virtualenv) was the thing to use. As far as I remember the named virtual environment was stored outside your working directory, so it was very helpful to name them. `pyenv virtualenv-init` was also handy as it auto-activated your virtualenv.

#### 1. Creating a sandboxed `venv` without a name

Am I right in thinking that running `uv venv` from within a directory (regardless of if it was created with `uv init`) creates a sandbox for ALL project packages etc? It seems the `venv/bin/python` is symlinked to the `uv python install` location, but that's the only thing that lives outside the `.venv` folder?

#### 2. Separate environments

If I do `uv init project1` and `uv init project2` then `uv venv` in both of them, the naming of the virtualenv doesn't matter as they're two different and distinct sandboxes?

#### 3. Python environment file

So if they're unnamed and isolated `.venv` instances, you no longer need a `.python-version` file?

#### 4. Installing packages

Will `uv add` just work, and pick the correct environment so long as you're in the project folder?

#### 5. Uninstalling virtual environments

Is there a command for this? Do you just [`rm -rf named-virtualenv/`](https://stackoverflow.com/questions/11005457/how-do-i-remove-delete-a-virtualenv) delete the folder? And the `.venv/` folder?

I think the documentation is a bit confusing, as it splits the `uv` centered commands from the "[The Pip Interface](https://docs.astral.sh/uv/pip/environments/)" (the old school way) so i wasn't sure which commands I should be using. If named virtual environments are discouraged, there should be a very obvious note there to warn against it.

Compared to [Elm Lang](https://elm-lang.org/) setup is a pain in a lot of other languages (especially on the backend), so it's good to see Python is making progress! For students you want something simple and painless, like [Thonny](https://thonny.org/).

---

_Comment by @zanieb on 2025-01-13 18:31_

1. Yeah, basically. Packages are hard-linked into the environment from the cache.
2. Yes. You don't need to run `uv venv` either, we'll create it on `uv sync` or `uv run`.
3. Sort of. The `.python-version` file is used here to ensure a consistent Python version for each developer of your project. Otherwise, each developer would get the first Python version on their `PATH` (that is compatible with the project). I don't think using them to target specific environments is ideal.
4. Yes.
5. There isn't a command. You can `rm -rf <dir>` yes.

I think #5200 would help. However, it's really hard for us to explain a transition from existing tooling since there are _so_ many workflows. The guides in the documentation are intended to "just work" without prior knowledge.

---

_Comment by @badlydrawnrob on 2025-01-19 13:29_

Thanks for clarifying those for me. Seems straightforward enough. I don't really need any migration guides as I'm starting from scratch. I guess you'd probably only want to support a subset of those workflows anyway, sounds like a big job!

---
