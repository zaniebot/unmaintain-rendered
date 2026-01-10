---
number: 6418
title: B905 incorrectly raised on older python versions
type: issue
state: closed
author: danielpatrickdotdev
labels:
  - needs-info
assignees: []
created_at: 2023-08-08T08:39:06Z
updated_at: 2023-08-10T17:17:14Z
url: https://github.com/astral-sh/ruff/issues/6418
synced_at: 2026-01-10T01:22:45Z
---

# B905 incorrectly raised on older python versions

---

_Issue opened by @danielpatrickdotdev on 2023-08-08 08:39_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

In versions of python prior to 3.10, B905 "`zip()` without an explicit `strict=` parameter" shouldn't be raised. The parameter was not available in versions 3.9 and lower.

ruff version: `0.0.282`
command: `ruff check .`
minimal config: `select = ["B"]`
code example:

```python
for n, c in zip([0, 1, 2], ["A", "B", "C"]):
    print(n, c)
```



---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-08 18:47_

---

_Label `bug` added by @charliermarsh on 2023-08-08 18:47_

---

_Comment by @charliermarsh on 2023-08-08 18:53_

I believe we have this gated correctly. Are you setting a [`target-version`](https://beta.ruff.rs/docs/settings/#target-version), or do you have a `requires-python` in your project? 

Example: `cargo run -p ruff_cli -- check foo.py --select B905 --target-version py38 --isolated` yields no errors on the above.

---

_Label `bug` removed by @charliermarsh on 2023-08-08 18:53_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-08-08 18:54_

---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-08 18:54_

---

_Comment by @charliermarsh on 2023-08-08 20:54_

(Closing as "working as intended", but happy to follow-up if you're still seeing issues when specifying a minimum supported version. We also changed 3.8 to the default minimum version in the latest release, which may resolve this for you.)

---

_Closed by @charliermarsh on 2023-08-08 20:54_

---

_Comment by @danielpatrickdotdev on 2023-08-10 16:13_

Thank you for pointing me in the right direction to resolve this one!

I wouldn't have thought to try setting target-version as it's documented as "The minimum Python version to target". This application targets specifically 3.8 and no higher at present, and the issue with B905 is it's only relevant to _higher_ versions than 3.9.

I'm curious whether there is (or will be in future) any way for ruff to detect the python version without hardcoding it? We'll now be defining it in three places in three different formats - once for pyenv, once for poetry, and now also for ruff.

---

_Comment by @zanieb on 2023-08-10 16:27_

@danielpatrickdotdev we'll infer `target-version` from `requires-python` in a `pyproject.toml` which is a standard central place to specify such things.

We can improve the documentation for `target-version` though (https://github.com/astral-sh/ruff/pull/6482)

> I'm curious whether there is (or will be in future) any way for ruff to detect the python version without hardcoding it?

I'm pretty interested in this but it increases the likelihood of different Ruff results for people working on a shared project.

---

_Comment by @danielpatrickdotdev on 2023-08-10 17:04_

Oh, using requires-python would be perfect!

I've just tried this but it's still raising on B905

```toml
[project]
requires-python = ">=3.8.1,<3.9"
```

---

_Comment by @zanieb on 2023-08-10 17:10_

There are some caveats described in https://beta.ruff.rs/docs/settings/#target-version — are you using a `ruff.toml` file?

---

_Comment by @danielpatrickdotdev on 2023-08-10 17:17_

Riiight that makes sense. Yeah, we try to keep all our configs separate as we optimise pre-commit to only run when config files or files under lint/test have changed. For example, ruff only runs when ruff.toml or python code has changed. Thinking about it, we should probably run ruff when pyproject.toml changes too, but if we keep ruff/black/isort/etc config out of there, that'll change less often.

Looks like we might have to think about abandoning that approach!

---
