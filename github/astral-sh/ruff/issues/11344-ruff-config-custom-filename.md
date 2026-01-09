---
number: 11344
title: ruff config custom filename
type: issue
state: closed
author: padrepitufo
labels: []
assignees: []
created_at: 2024-05-08T19:37:06Z
updated_at: 2024-05-10T13:38:18Z
url: https://github.com/astral-sh/ruff/issues/11344
synced_at: 2026-01-07T13:12:15-06:00
---

# ruff config custom filename

---

_Issue opened by @padrepitufo on 2024-05-08 19:37_

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

When using the `--config` option in `.pre-commit-config.yaml` like so:

```
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.4.3
    hooks:
      - id: ruff
        name: pycln api
        files: ^api/
        args: ["--config=./pyproject.ruff.toml"]
```

or when running the command locally with:

```
ruff --config pyproject.ruff.toml
```

 and supplying a pyproject formatted file with a name other than `pyproject.toml` (as shown above), ruff will *not* recognize the file as a pyproject formatted file (it will assume it is a malformed `.ruff.toml` file) and give errors like so:

```
  Cause: TOML parse error at line 1, column 1
  |
1 | [tool.ruff]
  | ^
unknown field `tool`
```

### Use Case
Why oh why am I splitting up my pyproject.toml files? The team I'm working with generally does this when the section gets long within `pyproject.toml`.   While it is nice to keep all the configurations in one file, it is also nice to keep all configurations in the same format and that is what I'm aiming for.

Is this something that could be supported by the ruff community?


### Context
command: `ruff --config pyproject.ruff.toml`
ruff: v0.4.3
pre-commit: 3.7.0


---

_Comment by @zanieb on 2024-05-08 19:40_

Is this conformant to the `pyproject.toml` specification? (I think probably not?) Why not just use the `ruff.toml` syntax?

---

_Comment by @padrepitufo on 2024-05-08 19:52_

@zanieb its a great question - I've been looking to see if there is somewhere in the spec that specifies that the name must be `pyproject.toml` or that it is just convention.  The pattern in use does work with [other tools](https://ichard26-testblackdocs.readthedocs.io/en/refactor_docs/pyproject_toml.html#where-black-looks-for-the-file); however, it's possible that it only *happens* to work because they aren't enforcing the aforementioned potentially existing spec.

The main reason for not adopting `ruff.toml` syntax is to keep these config files in the same format -- pyproject.toml format.

---

_Comment by @padrepitufo on 2024-05-08 20:15_

the closest I'm finding is where the name is required [when declaring `build system dependencies`](https://peps.python.org/pep-0518/#file-format).  Does that mean it is not required in other cases?

---

_Comment by @zanieb on 2024-05-08 21:18_

I'm not strongly opposed to looking for a `[tool.ruff]` top-level section in a `ruff.toml` but I don't know if this case is particularly motivating — it adds complexity for other users and the implementation. We support the same configuration, it's just a matter of being present in the top-level or a nested section.

The name is defined in [PEP 518](https://peps.python.org/pep-0518/#file-format), but there's not much else about it afaik.

---

_Comment by @charliermarsh on 2024-05-10 13:38_

Yeah, `pyproject.toml` is special and I don't think we should try to parse other TOML files as if they are `pyproject.toml`. It's helpful in this specific case, but it would make it harder to give good error messages and break other assumptions.

---

_Comment by @charliermarsh on 2024-05-10 13:38_

Going to close for now -- I suggest using `ruff.toml` here or naming the file `pyproject.toml`.

---

_Closed by @charliermarsh on 2024-05-10 13:38_

---
