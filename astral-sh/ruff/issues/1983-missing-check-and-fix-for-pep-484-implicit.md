```yaml
number: 1983
title: Missing check and fix for pep 484 implicit optional
type: issue
state: closed
author: rytilahti
labels:
  - fixes
  - typing
assignees: []
created_at: 2023-01-18T23:47:58Z
updated_at: 2023-06-12T18:12:12Z
url: https://github.com/astral-sh/ruff/issues/1983
synced_at: 2026-01-10T11:09:44Z
```

# Missing check and fix for pep 484 implicit optional

---

_Issue opened by @rytilahti on 2023-01-18 23:47_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->
Recent mypy versions have started to warn about implicit optionals on function signatures, which has been prohibited by pep 484 since https://github.com/python/peps/pull/689.
It would be great if ruff would spot and fix this automatically for you, considering how easy it is to forget to do this while coding.

Minimal code example:
```
def implicit_optional(optional: str = None) -> None:
    pass
```

`mypy` output:
```
test.py:1: error: Incompatible default for argument "optional" (default has type "None", argument has type "str")  [assignment]
test.py:1: note: PEP 484 prohibits implicit Optional. Accordingly, mypy has changed its default to no_implicit_optional=True
test.py:1: note: Use https://github.com/hauntsaninja/no_implicit_optional to automatically upgrade your codebase
```

The fix for python3.10 and newer would be modifying the definition to be:
```
def implicit_optional(optional: str | None = None) -> None:
    pass
```

On the implementation side, I think this would belong to flake8-annotations but as it isn't checking for this, I'm wondering what would be the correct place to implement this?

For older Python versions the fix would require either `from __future__ import annotations` or `from typing import Optional` and using `optional: Optional[str]`, both of which might be too invasive for automated fixing.

---

_Comment by @charliermarsh on 2023-01-18 23:51_

Yeah I'd love to do this. I want to add a couple autofixes for typing-related stuff and this is a good candidate.


---

_Label `autofix` added by @charliermarsh on 2023-01-18 23:51_

---

_Label `typing` added by @charliermarsh on 2023-01-18 23:51_

---

_Comment by @dhruvmanila on 2023-05-23 04:44_

I think this is a good rule to add along with the auto-fix ability. Should this be added under the `RUF` or `ANN` category? I'm leaning towards `RUF`. I can take this on.

---

_Comment by @dhruvmanila on 2023-06-02 06:44_

I've started working on this

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-06-10 16:37_

---

_Closed by @charliermarsh on 2023-06-12 18:12_

---
