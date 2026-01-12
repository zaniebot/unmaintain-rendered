```yaml
number: 8350
title: "Byte strings aren't docstrings"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: byte-strings-arent-docstrings
created_at: 2023-10-30T09:27:01Z
updated_at: 2023-10-30T09:58:35Z
url: https://github.com/astral-sh/ruff/pull/8350
synced_at: 2026-01-12T15:55:26Z
```

# Byte strings aren't docstrings

---

_@konstin_

We previously incorrectly treated byte strings in docstring position as docstrings because black does so (https://github.com/astral-sh/ruff/pull/8283#discussion_r1375682931, https://github.com/psf/black/issues/4002), even CPython doesn't recognize them:

```console
$ python3.12
Python 3.12.0 (main, Oct  6 2023, 17:57:44) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> def f():
...     b""" a"""
...
>>> print(str(f.__doc__))
None
```

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->


---

_Label `formatter` added by @MichaReiser on 2023-10-30 09:28_

---

_@MichaReiser approved on 2023-10-30 09:29_

LGTM. Let's see if the ecosystem check finds any usages 😆 

---

_Comment by @github-actions[bot] on 2023-10-30 09:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no format changes.



---

_Merged by @konstin on 2023-10-30 09:58_

---

_Closed by @konstin on 2023-10-30 09:58_

---

_Branch deleted on 2023-10-30 09:58_

---
