```yaml
number: 11431
title: Multiple .ruff_cache directories are created
type: issue
state: closed
author: mbyrnepr2
labels: []
assignees: []
created_at: 2024-05-15T08:47:17Z
updated_at: 2024-05-22T10:15:53Z
url: https://github.com/astral-sh/ruff/issues/11431
synced_at: 2026-01-10T11:09:53Z
```

# Multiple .ruff_cache directories are created

---

_Issue opened by @mbyrnepr2 on 2024-05-15 08:47_

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

Hi folks!

I've noticed that two `.ruff_cache` directories are created when running ruff in a particular situation.
Note that this doesn't occur if the `--isolated` flag is provided.

**To reproduce**
Run `ruff check .` or `ruff format .` on a **Windows** machine from the repository root.

```python
ruff --version
ruff 0.4.4
```

The repository contains a directory with curly braces.
A real-world example of where this could happen is a [repository using cookiecutter](https://github.com/audreyfeldroy/cookiecutter-pypackage/tree/master/%7B%7Bcookiecutter.project_slug%7D%7D).

An example structure:
```python
my_repo
├── module.py
└── {{cookie}}
    └── module.py
```

A `.ruff_cache` is created in the repository root ([expected](https://docs.astral.sh/ruff/settings/#cache-dir)).
A `.ruff_cache` is created under `{{cookie}}` (not expected - by me anyway :)).

**Extra note**
As an extra note, I'm not 100% sure if this next point is related to the above, but I'll share for context and it is after digging into this that the above was noticed.
A colleague of mine encountered the following (slightly adjusted here) output when running ruff on a similar scenario to the above:
```python
ruff failed
  4 files left unchanged
  4 files left unchanged
  Cause: Failed to rename temporary cache file to C:\path\to\repo\{{cookie}}\.ruff_cache\0.3.5\...
  Cause: failed to persist temporary file: Access is denied. (os error 5)
  Cause: Access is denied. (os error 5)
```

While I haven't been able to reproduce the cache issue here, I thought it was nevertheless curious to see multiple cache directories and hence the report.

Thanks for your time!

---

_Comment by @mbyrnepr2 on 2024-05-15 09:09_

Oh silly me. I didn't notice that in the `{cookie}` directory there was also a pyproject.toml file with a `[tool.ruff]` section. I guess that explains why the second .ruff_cache directory appears.

---

_Closed by @mbyrnepr2 on 2024-05-15 09:09_

---

_Closed by @mbyrnepr2 on 2024-05-15 09:09_

---

_Comment by @charliermarsh on 2024-05-15 13:01_

Thank you for the clear issue.

---

_Comment by @jayanthkoushik on 2024-05-22 02:55_

@mbyrnepr2 Any details/updates about the error in your extra note? I have been occasionally encountering that on Windows builds with GitHub actions, but haven't been able to reproduce it consistently. I don't believe it is related to the multiple `pyproject.toml`s though (at least not directly), since the issue is only on Windows.

---

_Comment by @mbyrnepr2 on 2024-05-22 10:15_

@jayanthkoushik unfortunately at this moment I can't shed more light on this. I'll talk to my colleagues and see if we can reproduce it (although I've been told that the behavior appears randomly or only occasionally).

---
