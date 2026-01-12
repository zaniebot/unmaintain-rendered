```yaml
number: 15951
title: "Behavior of `uv run` with no `pyproject.toml`"
type: issue
state: open
author: albertotb
labels:
  - question
assignees: []
created_at: 2025-09-19T16:18:09Z
updated_at: 2025-09-19T16:18:28Z
url: https://github.com/astral-sh/uv/issues/15951
synced_at: 2026-01-12T16:02:20Z
```

# Behavior of `uv run` with no `pyproject.toml`

---

_@albertotb_

### Question

I got so used to using `uv run` when working with projects, that I tried using it also when there is no `pyproject.toml` and got confused with the behavior. Steps to reproduce this:

- Create venv: `uv venv --python 3.9`
- Install some dependencies: `uv pip install -r requirements.txt`

The `uv pip install` seems to automatically pick up the previously created environment, es per the rules https://docs.astral.sh/uv/pip/environments/#using-arbitrary-python-environments One annoying thing is that if the base conda env is activated, this installs into that environment instead. It can be fixed with: `uv pip install --python .venv -r requirements.txt`

Now, if I do `uv run script.py`, it runs inside the previously created environment. However, I noticed that if I have a global Python version pinned with:

```
uv python pin --global 3.12
```

suddenly `uv run script.py` inside the same directory as before no longer picks up the venv in `.venv` and instead runs with the globally pinned Python version. You can confirm this by removing the pin with (is there any other way?):

`rm $HOME/.config/uv/.python-version`

and `uv run` works again. 

I understand that `uv run` is designed to be used with a projects workflow in mind, but I was wondering where can I find the rules that `uv run` uses to find the Python binary. As per https://docs.astral.sh/uv/pip/environments/#using-arbitrary-python-environments, `uv run` is a command that does not mutate the environment, so it points to https://docs.astral.sh/uv/concepts/python-versions/#discovery-of-python-versions. However, that does not seem correct since the Python in my `PATH` is a completely different version to the one in the venv, and `uv run` correctly picks up the venv if no global Python version is pinned by uv.

Summary and my expectations: even when no `pyproject.toml` is present, I would expect (maybe naively) that `uv run` uses the Python binary in the venv if a `.venv` directory is present. There are probably many reasons why it does not work this way, but at least I would like to understand the rules behind this behavior, because it seems that sort of works sometimes.

### Platform

Ubuntu 22.04.5 LTS

### Version

0.8.18

---

_Label `question` added by @albertotb on 2025-09-19 16:18_

---

_Renamed from "Behavior of `uv run` outside a project" to "Behavior of `uv run` with no `pyproject.toml`" by @albertotb on 2025-09-19 16:18_

---
