---
number: 4549
title: flake8-todos false negatives
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
assignees: []
created_at: 2023-05-20T18:48:08Z
updated_at: 2023-05-25T14:51:47Z
url: https://github.com/astral-sh/ruff/issues/4549
synced_at: 2026-01-07T13:12:14-06:00
---

# flake8-todos false negatives

---

_Issue opened by @JonathanPlasse on 2023-05-20 18:48_

```python
# TODO
# ^^^^ Detect TODO errors
# foo # TODO
#       ^^^^ Do not detect TODO errors
```
On Ruff 0.0.269
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @charliermarsh on 2023-05-20 18:50_

This _might_ be intentional. Do you have an example of this in the wild?

---

_Label `question` added by @charliermarsh on 2023-05-20 18:51_

---

_Comment by @evanrittenhouse on 2023-05-20 18:52_

Yeah, I picked the regexes to match the [existing plugin's regex](https://github.com/orsinium-labs/flake8-todos/blob/master/flake8_todos/_rules.py#L12) to avoid confusion on first iteration. If we'd like to change it, that's something that we can easily change.

---

_Comment by @JonathanPlasse on 2023-05-20 18:52_

```python
class Foo:
    @property
    def id(  # noqa: A003 # TODO(jonathan): When doing BREAKING CHANGES rename to avoid shadowing builtin id
        self,
    ) -> str:
        pass

---

_Comment by @evanrittenhouse on 2023-05-20 18:55_

Ah, you know what? It looks like they use `re.search`, meaning that the first location of the pattern is returned. That's my bad, I just looked at the regex pattern as a standalone. @charliermarsh can you assign this to me please? I'll fix it

---

_Assigned to @evanrittenhouse by @charliermarsh on 2023-05-20 18:57_

---

_Label `question` removed by @charliermarsh on 2023-05-20 18:58_

---

_Label `bug` added by @charliermarsh on 2023-05-20 18:58_

---

_Referenced in [astral-sh/ruff#4558](../../astral-sh/ruff/pulls/4558.md) on 2023-05-21 03:59_

---

_Closed by @MichaReiser on 2023-05-25 14:51_

---
