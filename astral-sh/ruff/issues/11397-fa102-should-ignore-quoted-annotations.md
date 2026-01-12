```yaml
number: 11397
title: FA102 should ignore quoted annotations
type: issue
state: closed
author: inoa-jboliveira
labels:
  - bug
assignees: []
created_at: 2024-05-13T04:15:59Z
updated_at: 2024-05-21T18:57:14Z
url: https://github.com/astral-sh/ruff/issues/11397
synced_at: 2026-01-12T15:54:51Z
```

# FA102 should ignore quoted annotations

---

_@inoa-jboliveira_

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
ruff 0.4.4 (3e8878a1c 2024-05-09)

We have an application that targets Python 3.9+, but instead of `from __future__ import annotations` on all files that is kinda ugly and unnecessary most of the time, whenever people need PEP 604 syntax they put the annotation within quotes which is effectively the same as the future import does except when someones accesses said annotations in run time (we don't).

See excerpt below:

```
setup\split_code_files.py:96:34: FA102 Missing `from __future__ import annotations`, but uses PEP 604 union
   |
94 |         self.nodes = self.get_ast_nodes()
95 |
96 |     def get_yaml_nodes(self) -> 'dict[str, str] | None':
   |                                  ^^^^^^^^^^^^^^^^^^^^^ FA102
97 |         try:
98 |             with open(self.yaml_file) as yaml_file:
   |
   = help: Add `from __future__ import annotations`
```

I believe FA102 should just ignore the annotation if it is within quotes. It should not mark this as violation because the code is indeed compatible with Python 3.9.

---

_Label `bug` added by @charliermarsh on 2024-05-13 17:02_

---

_Comment by @charliermarsh on 2024-05-13 17:02_

This sounds right to me.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-13 17:17_

---

_Comment by @AlexWaygood on 2024-05-13 17:28_

I'm not sure, this feels like intentional behaviour to me (https://github.com/astral-sh/ruff/pull/11414#issuecomment-2108336700) -- one of the motivating reasons for FA102 is to enable UP037 to automatically remove quotes from your annotations for you. You say you find the `from __future__ import annotations` import ugly -- I suppose it's a matter of opinion, but I find quoted annotations much more ugly personally :-)

---

_Comment by @AlexWaygood on 2024-05-13 17:29_

@inoa-jboliveira, what's your reason for enabling FA102 in the first place if you find the `from __future__ import annotations` import ugly? The purpose of this check isn't to ensure that your code is compatible with Python 3.9

---

_Comment by @inoa-jboliveira on 2024-05-13 18:19_

Hi @AlexWaygood , thank you for the reply. I have some considerations

> what's your reason for enabling FA102 in the first place if you find the `from __future__ import annotations` import ugly? The purpose of this check isn't to ensure that your code is compatible with Python 3.9

Of course the purpose is to check if the code is compatible  older verions, it says so in the docs: 

> Checks for uses of PEP 585- and PEP 604-style type annotations in Python modules that lack the required from __future__ import annotations import for compatibility with older Python versions.
> https://docs.astral.sh/ruff/rules/future-required-type-annotation/

I want the check, **I just don't want the proposed fix**. For us is quite important if the code is invalid, but there are multiple ways to solve this problem.

I don't like UP037 because it is an opinonated fix that is meant for when a class references itself before it is fully created. There is a lot more depth around the annotation issues and "from __future__ import annotations" have been postponed multiple times because people cannot agree yet on it. I think it might some day become the right approach, but until so I might not want to use it.


From python docs:

> https://docs.python.org/3/library/__future__.html#id2
> from __future__ import annotations was previously scheduled to become mandatory in Python 3.10, but the Python Steering Council twice decided to delay the change ([announcement for Python 3.10](https://mail.python.org/archives/list/python-dev@python.org/message/CLVXXPQ2T2LQ5MP2Y53VVQFCXYWQJHKZ/); [announcement for Python 3.11](https://mail.python.org/archives/list/python-dev@python.org/message/VIZEBX5EYMSYIJNDBF6DMUMZOCWHARSO/)). No final decision has been made yet. See also [PEP 563](https://peps.python.org/pep-0563/) and [PEP 649](https://peps.python.org/pep-0649/).



---

_Comment by @AlexWaygood on 2024-05-13 18:23_

> Of course the purpose is to check if the code is compatible older verions, it says so in the docs:

yeah, sorry, I got quite mixed up here 🙃 -- https://github.com/astral-sh/ruff/pull/11414#issuecomment-2108517695. I'll go ahead and blame it on some significant jetlag :(

---

_Closed by @charliermarsh on 2024-05-21 18:57_

---
