```yaml
number: 10289
title: "error: Chinese characters in .ipynb cause an error"
type: issue
state: closed
author: zhuxining
labels:
  - bug
assignees: []
created_at: 2024-03-08T03:04:32Z
updated_at: 2024-03-08T03:24:04Z
url: https://github.com/astral-sh/ruff/issues/10289
synced_at: 2026-01-12T15:54:50Z
```

# error: Chinese characters in .ipynb cause an error

---

_@zhuxining_


* A minimal code snippet that reproduces the bug.

```
df_dict_org = pd.read_excel(
    "字典/dict-机构编码.xlsx", 
    index_col=None,
    na_values=["NA"],
    dtype=str,
).dropna(how="all")

```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

```
panicked at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/str/mod.rs:660:13:
byte index 2 is not a char boundary; it is inside '字' (bytes 1..4) of `"字典/dict-机构编码.xlsx",`
Backtrace:    0: _rust_eh_personality
   1: _main
   2: _rust_eh_personality
   3: _rust_eh_personality
   4: _rust_eh_personality
   5: _rust_eh_personality
   6: __rjem_je_witnesses_cleanup
   7: _main
   8: __rjem_je_witnesses_cleanup
   9: _main
  10: _main
  11: _main
  12: _main
  13: _main
  14: _main
  15: _main
  16: _main
  17: __mh_execute_header
  18: __mh_execute_header
  19: _main

```

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

```
[project]
name = "playground"
version = "0.1.0"
description = "Add your description here"
dependencies = [
    "pandas>=2.2.1",
    "openpyxl>=3.1.2",
    "charset_normalizer>=3.3.2",
    "polars>=0.20.11",
]
readme = "README.md"
requires-python = ">= 3.12"

[project.scripts]
hello = "playground:hello"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.rye]
managed = true
dev-dependencies = ["ipykernel>=6.29.3", "pyarrow>=15.0.1"]

[tool.hatch.metadata]
allow-direct-references = true

[tool.hatch.build.targets.wheel]
packages = ["src/playground"]

```

* The current Ruff version (`ruff --version`).
```
ruff 0.1.8
```


---

_Comment by @dhruvmanila on 2024-03-08 03:14_

Hey, thanks for providing all the details for the issue you're facing. This was fixed in `v0.1.9` with https://github.com/astral-sh/ruff/pull/9146.

---

_Closed by @dhruvmanila on 2024-03-08 03:14_

---

_Label `bug` added by @charliermarsh on 2024-03-08 03:24_

---
