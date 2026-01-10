```yaml
number: 4945
title: "Putting \"T\" in ignore does not ignore flake8-fixme (T)"
type: issue
state: closed
author: ColemanDunn
labels: []
assignees: []
created_at: 2023-06-07T23:17:16Z
updated_at: 2023-06-07T23:25:22Z
url: https://github.com/astral-sh/ruff/issues/4945
synced_at: 2026-01-10T11:09:47Z
```

# Putting "T" in ignore does not ignore flake8-fixme (T)

---

_Issue opened by @ColemanDunn on 2023-06-07 23:17_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
# TODO: this is not getting ignored
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
ruff check .
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
- using select = ["ALL"] and ignore = ["T"]
* The current Ruff version (`ruff --version`).
- 0.0.271
-->
* A minimal code snippet that reproduces the bug.

   `# TODO: this is not getting ignored`

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

   ruff check .

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

   using select = ["ALL"] and ignore = ["T"]

* The current Ruff version (`ruff --version`).

   0.0.271

getting the following:
<img width="242" alt="Screenshot 2023-06-07 at 5 15 23 PM" src="https://github.com/charliermarsh/ruff/assets/42652642/16eff95a-d28d-4243-b761-8ae50a19b971">

Putting the explicit rule ("T002") happens to work. Luckily there are only 4 of them. But "T" without any numbers is not working. 

Tagging @evanrittenhouse in case you know anything about this. 

Thanks!

---

_Comment by @charliermarsh on 2023-06-07 23:19_

Hey sorry — this is fixed in the next release, which I plan to cut tonight, but there’s some info on temporary workarounds here: https://github.com/charliermarsh/ruff/issues/4916

---

_Closed by @charliermarsh on 2023-06-07 23:19_

---

_Comment by @ColemanDunn on 2023-06-07 23:25_

Thanks!

---
