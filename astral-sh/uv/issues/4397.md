```yaml
number: 4397
title: uv toolchain python not in interpreter search path
type: issue
state: closed
author: fynnsu
labels:
  - question
  - preview
assignees: []
created_at: 2024-06-18T18:36:03Z
updated_at: 2024-06-18T19:01:47Z
url: https://github.com/astral-sh/uv/issues/4397
synced_at: 2026-01-10T05:31:37Z
```

# uv toolchain python not in interpreter search path

---

_Issue opened by @fynnsu on 2024-06-18 18:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Thank you for implementing the `uv toolchain` functionality, it's a very helpful feature.

That being said, I noticed a couple sharp edges when trying it out, the main one being that the installed interpreters are not found by `uv venv --python `.

```
❯ uv toolchain list
warning: `uv toolchain list` is experimental and may change without warning.
...
cpython-3.8.19-linux-x86_64-gnu /home/fynnsu/.local/share/uv/toolchains/cpython-3.8.19-linux-x86_64-gnu/install/bin/python3
...

❯ uv venv -p 3.8
  × No interpreter found for Python 3.8 in search path

❯ uv venv -p cpython-3.8.19-linux-x86_64-gnu
  × No interpreter found key cpython-3.8.19-linux-x86_64-gnu in search path

❯ uv venv -p /home/fynnsu/.local/share/uv/toolchains/cpython-3.8.19-linux-x86_64-gnu/install/bin/python3
Using Python 3.8.19 interpreter at: /home/fynnsu/.local/share/uv/toolchains/cpython-3.8.19-linux-x86_64-gnu/install/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

Presumably, they would be found if I added `/home/fynnsu/.local/share/uv/toolchains/` to my path but this seems like something that should work without doing so.

The other small thing I would mention is I initially tried `uv toolchain install py38` and `cp38` before eventually working out that I needed `3.8` or `cpython3.8`. It would be helpful if there was a recommended format, either in the error message or the subcommand help.





---

_Comment by @zanieb on 2024-06-18 18:40_

We discover managed toolchains in `uv venv` with the `--preview` flag. It's not available by default yet because the behavior is still experimental. You can also globally opt in with `UV_PREVIEW=1` if you're using our preview features in general, this will also drop the nag from preview commands like `uv toolchain`.

> The other small thing I would mention is I initially tried uv toolchain install py38 and cp38 before eventually working out that I needed 3.8 or cpython3.8.

Interesting. We're working on more documentation around this, but we could also add support for that syntax. What's that based on?

---

_Label `question` added by @zanieb on 2024-06-18 18:41_

---

_Label `preview` added by @zanieb on 2024-06-18 18:41_

---

_Assigned to @zanieb by @zanieb on 2024-06-18 18:41_

---

_Comment by @fynnsu on 2024-06-18 18:53_

> Interesting. We're working on more documentation around this, but we could also add support for that syntax. What's that based on?

Hmm not entirely sure where I got this from. I think the `cp38` is likely from python package name conventions (ex. https://download.pytorch.org/whl/test/torch/)

I also think `py38` is a fairly common way to refer to python versions. A notable example of this being the `ruff` config settings: https://docs.astral.sh/ruff/settings/#target-version.

---

_Comment by @zanieb on 2024-06-18 18:57_

Ah like wheel platform compatibility tags: https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/

Yeah we can support that. Tracking at https://github.com/astral-sh/uv/issues/4399

---

_Closed by @zanieb on 2024-06-18 18:57_

---

_Comment by @zanieb on 2024-06-18 18:58_

And we'll track the documentation request at https://github.com/astral-sh/uv/issues/4400

---

_Comment by @fynnsu on 2024-06-18 19:00_

> Ah like wheel platform compatibility tags: https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/

Yes exactly, thanks

---

_Comment by @zanieb on 2024-06-18 19:01_

And https://github.com/astral-sh/uv/issues/4401 :)


---
