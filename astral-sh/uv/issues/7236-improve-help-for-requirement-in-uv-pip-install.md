```yaml
number: 7236
title: "Improve help for `--requirement` in `uv pip install`"
type: issue
state: open
author: AdrianB-sovo
labels:
  - documentation
assignees: []
created_at: 2024-09-10T00:10:16Z
updated_at: 2024-09-10T02:47:19Z
url: https://github.com/astral-sh/uv/issues/7236
synced_at: 2026-01-12T15:59:11Z
```

# Improve help for `--requirement` in `uv pip install`

---

_@AdrianB-sovo_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Using `uv v0.4.8`, when I do `uv pip install --help`, I see the following:
```text
Options:
  -r, --requirement <REQUIREMENT>            Install all packages listed in the given `requirements.txt` files
  ...
```

The complete description for the option is:
```rust
    /// Install all packages listed in the given `requirements.txt` files.
    ///
    /// If a `pyproject.toml`, `setup.py`, or `setup.cfg` file is provided, uv will
    /// extract the requirements for the relevant project.
    ///
    /// If `-` is provided, then requirements will be read from stdin.
    #[arg(long, short, group = "sources", value_parser = parse_file_path)]
    pub requirement: Vec<PathBuf>,
```

With just the short description, it's difficult to know that it also supports other types of files (which I was looking for).

Maybe the short description could be updated to the following?
```text
Options:
  -r, --requirement <REQUIREMENT>            Install packages listed in the given file(s) (`requirements.txt`, `pyproject.toml`, `setup.py`, or `setup.cfg`)
  ...
```

---

_Comment by @zanieb on 2024-09-10 02:47_

Well the idea is that short description is short so I'm not sure I want to list all the formats there but yeah we should update this since we support more file formats here now.

---

_Label `documentation` added by @zanieb on 2024-09-10 02:47_

---

_Assigned to @zanieb by @zanieb on 2024-09-10 02:47_

---

_Comment by @zanieb on 2024-09-10 02:47_

Thanks for filing an issue!

---
