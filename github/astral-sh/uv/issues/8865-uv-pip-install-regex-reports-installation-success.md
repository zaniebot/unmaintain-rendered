---
number: 8865
title: "`uv pip install regex` reports installation success but library cannot be imported (pip works)"
type: issue
state: closed
author: ssbarnea
labels: []
assignees: []
created_at: 2024-11-06T16:08:19Z
updated_at: 2024-11-07T20:23:28Z
url: https://github.com/astral-sh/uv/issues/8865
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip install regex` reports installation success but library cannot be imported (pip works)

---

_Issue opened by @ssbarnea on 2024-11-06 16:08_

Apparently MacOS specific issues:

```
uv --version
uv 0.4.30

> python3.12 -m uv pip install --force-reinstall regex
...
> python3.12 -c "import regex"
ModuleNotFoundError: No module named 'regex._regex'
```

- [ ] python3.13t broken, fail to import
- [ ] python3.13 broken, fail to import
- [ ] python3.12 broken, fail to import
- [ ] python3.11 broken, fail to import
- [ ] python3.10 works!

If the package is reinstalled using pip itself, it will work.

References: https://github.com/mrabarnett/mrab-regex/issues/547

---

_Comment by @zanieb on 2024-11-06 16:21_

I cannot reproduce this

```
❯ uv run --python 3.12 --with regex==2024.9.11 python -c "import regex; print(regex)"
<module 'regex' from '/Users/zb/Library/Caches/uv/archive-v0/7fA06VFYeGmOUsVkY2G6i/lib/python3.12/site-packages/regex/__init__.py'>
❯ uv venv
Using CPython 3.12.7
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install regex==2024.9.11
Resolved 1 package in 3ms
Installed 1 package in 0.89ms
 + regex==2024.9.11
❯ .venv/bin/python -c "import regex"
```

---

_Comment by @ssbarnea on 2024-11-06 16:28_

Update: while I initially suspected this to be specific to macos-arm64, it seems to no to be true as I seen the exact same error on linux with intel architectures at https://github.com/ansible/tox-ansible/actions/runs/11705926832/job/32601848111?pr=389#step:9:91

So, it can happen on both arm64 and amd64, linux and macos and multiple versions of python.

---

_Renamed from "macos: `uv pip install regex` reports installation success but library cannot be imported (pip works)" to "`uv pip install regex` reports installation success but library cannot be imported (pip works)" by @ssbarnea on 2024-11-06 16:32_

---

_Comment by @ssbarnea on 2024-11-06 16:37_

@zanieb Thanks for the very fast reply. Apparently that failure is caused by something else. For sure is not a specific local setup issue because I was able to reproduce it on my macos and but also on the github runners.

I suspect it might be related to how python was compiled?

Using uv provided pythons seems to be working, by I am currently using asdf ones on mac and on github runners, I am not sure where github is taking the pythons from.

Any ideas about what else to invetigate?

---

_Comment by @ssbarnea on 2024-11-07 11:58_

I was able to find that uv installs wrong binary for regex. While using py312, I found that the installed package had `_regex.cpython-310-darwin.so`... so that is why python failed to import it... and found it:

```toml
[tool.uv.pip]
annotation-style = "line"
custom-compile-command = "tox run deps"
python-version = "3.10"
```

The last line makes uv install the wrong wheel, even if documentation states that it should do something different:
```
 --python-version <PYTHON_VERSION>      The minimum Python version that should be supported by the requirements (e.g., `3.7` or `3.7.9`)
```

I tried to use this config option in order to produce predictable lock file using `uv pip compile` but the side effect is that once you configure it, uv will start installing wrong binary wheel all the time.

This makes be believe that this is related to https://github.com/astral-sh/uv/issues/3883

---

_Comment by @ssbarnea on 2024-11-07 12:45_

Closing is as it seems to caused by #3883 

---

_Closed by @ssbarnea on 2024-11-07 12:45_

---

_Comment by @zanieb on 2024-11-07 20:23_

Thank you for following up!

---

_Referenced in [squidfunk/mkdocs-material#7676](../../squidfunk/mkdocs-material/issues/7676.md) on 2024-11-08 15:03_

---

_Referenced in [astral-sh/uv#3883](../../astral-sh/uv/issues/3883.md) on 2025-01-29 13:56_

---
