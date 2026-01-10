```yaml
number: 10983
title: "Formatter: include files that were formatted in the output"
type: issue
state: closed
author: AdrianB-sovo
labels:
  - cli
  - formatter
assignees: []
created_at: 2024-04-16T20:46:46Z
updated_at: 2024-04-17T17:32:59Z
url: https://github.com/astral-sh/ruff/issues/10983
synced_at: 2026-01-10T11:09:53Z
```

# Formatter: include files that were formatted in the output

---

_Issue opened by @AdrianB-sovo on 2024-04-16 20:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
formatter, CI, filenames, files
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
It would be useful to have the files that were formatted when running `ruff-format`.
**Usecase:** I use the pre-commit config to run Ruff locally before each commit, but also on the CI **for all files**.
- Locally, I want to automatically change the files with the formatter on a commit.
- In the CI, I want to see what files are not correctly formatted if the format checking fails.

So, to be able to do this with the same config, I use `ruff-format` **without** the `--check` option, but then I don't get the list of formatted files.
With the `--check` option:
```
ruff.....................................................................Passed
ruff-format..............................................................Failed
- hook id: ruff-format
- exit code: 1

Would reformat: src/api/v1/endpoints/customers.py
1 file would be reformatted, 317 files already formatted
```

Without the `--check` option:
```
ruff.....................................................................Passed
ruff-format..............................................................Failed
- hook id: ruff-format
- files were modified by this hook

1 file reformatted, 317 files left unchanged
```

It's not a big problem, since I can run the formatter locally on all files, but I think it would still be nice to have the list of formatted files, either by default or by opting-in with an option.

I'm using ruff v0.3.6.

---

_Renamed from "Formatter: include files that were formatted" to "Formatter: include files that were formatted in the output" by @AdrianB-sovo on 2024-04-16 20:47_

---

_Label `cli` added by @AlexWaygood on 2024-04-16 21:14_

---

_Label `formatter` added by @AlexWaygood on 2024-04-16 21:14_

---

_Comment by @charliermarsh on 2024-04-17 17:32_

I believe this is the same as https://github.com/astral-sh/ruff/issues/8953.

---

_Closed by @charliermarsh on 2024-04-17 17:32_

---
