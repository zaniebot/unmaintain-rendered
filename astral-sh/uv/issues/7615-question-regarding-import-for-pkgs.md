---
number: 7615
title: Question regarding import for pkgs
type: issue
state: closed
author: AnirudhG07
labels: []
assignees: []
created_at: 2024-09-21T19:23:59Z
updated_at: 2024-10-08T06:53:41Z
url: https://github.com/astral-sh/uv/issues/7615
synced_at: 2026-01-10T01:24:17Z
---

# Question regarding import for pkgs

---

_Issue opened by @AnirudhG07 on 2024-09-21 19:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Hi, thanks for the amazing tool `uv`. I am making a python package [bept](https://github.com/IISc-Software-iGEM/bept) for some project.
I am new to python packages and I wanted to know that in my [src/bept/main.py](https://github.com/IISc-Software-iGEM/bept/blob/main/src/bept/main.py), I am using `bept.something.somefile`, but is there a way i can do `src.bept.something`, that way I would be able to access some files outside of `src`. (or is there any other way for it? main goal: have test directory use codes in src.)
I know its a bit case specific, but some help would be really appreciated!

Current version of uv = 0.4.15

---

_Comment by @danielhollas on 2024-09-22 10:20_

Hi @AnirudhG07,

I think this is usually done by installing your package in a virtual environment in an [editable mode](https://docs.astral.sh/uv/pip/packages/#editable-packages) (so that any changes that you make are immediately reflected), and then you can refer to your package and modules from your tests as you normally would (e.g. `bept.something`).

For example if you use pytest

```
uv venv
uv pip install --editable .
. .venv/bin/activate
pytest
```

If you use the more modern workflow that `uv` shipped in version 0.4, I think simply running your test command via `uv run` should take care of all that automatically. (but I don't have that much experience here)

(not affiliated with astral team, just passing by :-))

---

_Comment by @AnirudhG07 on 2024-09-22 10:29_

Alright. Thanks, ill try it out
But still is there a way i can change `bept.something` to `src.bept.something`? 

---

_Closed by @AnirudhG07 on 2024-10-08 06:53_

---
