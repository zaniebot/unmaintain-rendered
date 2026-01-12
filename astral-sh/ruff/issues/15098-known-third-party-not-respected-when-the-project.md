```yaml
number: 15098
title: known-third-party not respected when the project root (local folder) contains a subfolder with the same name as a declared import
type: issue
state: closed
author: Dev-iL
labels: []
assignees: []
created_at: 2024-12-22T15:57:05Z
updated_at: 2024-12-24T08:37:24Z
url: https://github.com/astral-sh/ruff/issues/15098
synced_at: 2026-01-12T15:54:54Z
```

# known-third-party not respected when the project root (local folder) contains a subfolder with the same name as a declared import

---

_@Dev-iL_

## Background
I have a project with the following structure:

```none
📂 my_project
┣━━ 📂 airflow    <- possibly an implicit namespace package, see note at the end
┃   ┗━━ 📂 dags
┃       ┣━━ 🐍 __init__.py
┃       ┣━━ 🐍 dag_1.py
┃       ┣━━ ...
┃       ┗━━ 🐍 dag_n.py
┣━━ 📂 src
┃   ┗━━ 📂 my_lib
┃       ┣━━ 🐍 __init__.py
┃       ┗━━ ...
┗━━ 📄 pyproject.toml
```

In my `pyproject.toml` I have the following sections related to import sorting:

```toml
[tool.ruff.lint.isort]
known-third-party = ["airflow"]
section-order = ["future", "standard-library", "third-party", "company-libs", "first-party", "local-folder"]

[tool.ruff.lint.isort.sections]
company-libs = ["my_other_lib"]
```

## Observed behavior
Whenever I have imports of the form
```python
from airflow import DAG  # This refers to the 3rd-party library; I never import from the top-level `airflow` folder

from my_other_lib import foo
```

Ruff issues an [I001](https://docs.astral.sh/ruff/rules/unsorted-imports/) and rearranges them to 
```python
from my_other_lib import foo

from airflow import DAG
```

## Expected behavior
The imports should remain unchanged, per the `pyproject.toml` configurations.

## Additional notes
- isort handles this as expected.
- Deleting `my_project/airflow/dags/__init__.py` stops this behavior from happening.
- I might have oversimplified the example for sharing. If the issue is unreproducible - I'll revise the post.

---

_Comment by @dhruvmanila on 2024-12-24 05:51_

Thanks for providing all the details.

I tried to reproduce this using the project structure but unable to do so. Can you provide some additional details?
* Which file should I put the source code? Should it be in `airflow/dags/dag_1.py`?
* How are you running `ruff`? Are you using the CLI or the editor? If it's the CLI, what's the command that you used for invocation?
* Why does the import say `my_other_lib` but the tree mentions `my_lib`? Is that expected or a typo?
* Can you provide the Ruff version used here?

Currently, I've the following structure:
```
.
├── airflow
│   └── dags
│       ├── __init__.py
│       └── dag_1.py
├── pyproject.toml
└── src
    └── my_lib
        └── __init__.py
```

With the same config in `pyproject.toml` and the source code in `airflow/dags/dag_1.py`, I run `ruff check --select=I .` from the project root with `ruff 0.8.4`.

---

_Label `needs-mre` added by @dhruvmanila on 2024-12-24 05:52_

---

_Comment by @Dev-iL on 2024-12-24 06:06_

@dhruvmanila Thank you for your comment. I will provide a zipped MRE in a few hours. Re your questions:

1. The source code should be under my_lib.
2. Editor integrations (pycharm + vscode) are causing this for sure, I'm not sure about the precommit hook, probably yes.
3. Note the pyproject.toml - my_other_lib is defined there as an example that should end up underneath third-party.
4. It happens for as long as remember, including in 0.8.4.

---

_Comment by @Dev-iL on 2024-12-24 08:24_

While trying to create the MRE, I narrowed the issue down. Turns out it's related to a difference between the **PyCharm Ruff plugin** and Ruff itself, as discussed [here](https://github.com/koxudaxi/ruff-pycharm-plugin/issues/422). Here's what it looks like:

![Image](https://github.com/user-attachments/assets/21ec59b1-6ac5-40f7-a4ef-1c9327bb28f8)

Closing since this doesn't seem to be a Ruff issue. Thanks again, and sorry about the false alarm!

---

_Closed by @Dev-iL on 2024-12-24 08:24_

---

_Comment by @dhruvmanila on 2024-12-24 08:37_

No worries, thank you for taking some additional time to try to reproduce it.

---

_Label `needs-mre` removed by @dhruvmanila on 2024-12-24 08:37_

---
