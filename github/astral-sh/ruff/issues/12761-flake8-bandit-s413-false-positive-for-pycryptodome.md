---
number: 12761
title: "[`flake8-bandit`]: S413 false positive for PyCryptodome "
type: issue
state: open
author: trim21
labels:
  - documentation
assignees: []
created_at: 2024-08-08T17:05:35Z
updated_at: 2024-08-09T01:55:54Z
url: https://github.com/astral-sh/ruff/issues/12761
synced_at: 2026-01-07T13:12:15-06:00
---

# [`flake8-bandit`]: S413 false positive for PyCryptodome 

---

_Issue opened by @trim21 on 2024-08-08 17:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

ruff 0.5.7 with preview enabled.

PyCryptodome is a fork of PyCrypto and have same import name `Crypto`.

It trigger S413 `pycrypto` library is known to have publicly disclosed buffer overflow vulnerability



---

_Comment by @MichaReiser on 2024-08-08 17:24_

Thanks for reporting this. Do you have a link to where it states that the vulnerability is fixed?

---

_Comment by @trim21 on 2024-08-08 17:46_

it didn't state, but it doesn't re-produce [CVE-2013-7459](https://github.com/advisories/GHSA-cq27-v7xp-c356)

---

_Comment by @MichaReiser on 2024-08-08 20:12_

Hmm. I don't think there's much we can do when both packages use the same name other than documenting that this rule doesn't apply to PyCryptodome

---

_Renamed from "[`flake8-bandit`]: S413 false positive for " to "[`flake8-bandit`]: S413 false positive for PyCryptodome " by @trim21 on 2024-08-08 20:40_

---

_Comment by @trim21 on 2024-08-08 22:49_

> Hmm. I don't think there's much we can do when both packages use the same name other than documenting that this rule doesn't apply to PyCryptodome

read pyproject.toml and parse deps list maybe？

I understand ruff can't know for sure which package this is, add docs is OK for me.

---

_Comment by @charliermarsh on 2024-08-09 01:30_

Happy to document it but it would be helpful to have a clear source to cite. How did you learn that it doesn't reproduce that CVE, for example?


---

_Label `documentation` added by @charliermarsh on 2024-08-09 01:30_

---

_Comment by @trim21 on 2024-08-09 01:52_

> Happy to document it but it would be helpful to have a clear source to cite. How did you learn that it doesn't reproduce that CVE, for example?

source repo: https://github.com/Legrandin/pycryptodome

https://github.com/pycrypto/pycrypto/issues/176 give a example code and it can be executed with PyCryptodome without python crash

---
