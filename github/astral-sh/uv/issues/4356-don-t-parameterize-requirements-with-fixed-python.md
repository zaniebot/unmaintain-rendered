---
number: 4356
title: "Don't parameterize requirements with fixed python version"
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2024-06-17T12:06:58Z
updated_at: 2024-07-01T08:54:10Z
url: https://github.com/astral-sh/uv/issues/4356
synced_at: 2026-01-07T13:12:17-06:00
---

# Don't parameterize requirements with fixed python version

---

_Issue opened by @konstin on 2024-06-17 12:06_

When debugging `uv pip install 'jsonschema>=3.0.0' 'reana-client>=0.8.0'` on python 3.12 from https://github.com/astral-sh/uv/issues/4333, the logs show that we parameterize some requirements on `python_version >= '3.12'`. We should only have/show `snakemake==7.9.0` instead of `snakemake{python_version >= '3.12'}==7.9.0`, given that the python version is fixed to 3.12 for this resolution and not a forked marker or similar parameter.

```
DEBUG Adding transitive dependency for reana-commons[snakemake]==0.9.4: snakemake{python_version >= '3.12'}==7.9.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9a/87/cff3c63ebe067ec9a7cc1948c379b8a16e7990c29bd5baf77c0a1dbd03c0/lxml-5.2.2-cp312-cp312-manylinux_2_28_x86_64.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/tabulate/
DEBUG Found fresh response for: https://pypi.org/simple/snakemake/
DEBUG Searching for a compatible version of snakemake{python_version >= '3.12'} (==7.9.0)
DEBUG Selecting: snakemake{python_version >= '3.12'}==7.9.0 (snakemake-7.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of snakemake{python_version >= '3.12'} (==7.9.0)
DEBUG Selecting: snakemake{python_version >= '3.12'}==7.9.0 (snakemake-7.9.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/92/4e/e5a13fdb3e6f81ce11893523ff289870c87c8f1f289a7369fb0e9840c3bb/tabulate-0.8.10-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/27/75/f826abdebd05f6986c141e05ab59a3e5b58a7686290b2cdac5e4cb54ac79/snakemake-7.9.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for snakemake{python_version >= '3.12'}==7.9.0: wrapt*
DEBUG Adding transitive dependency for snakemake{python_version >= '3.12'}==7.9.0: requests*
DEBUG Adding transitive dependency for snakemake{python_version >= '3.12'}==7.9.0: ratelimiter*
DEBUG Adding transitive dependency for snakemake{python_version >= '3.12'}==7.9.0: pyyaml*
DEBUG Adding transitive dependency for snakemake{python_version >= '3.12'}==7.9.0: configargparse*
DEBUG Adding transitive dependency for snakemake{python_version >= '3.12'}==7.9.0: appdirs*
DEBUG Adding transitive dependency for snakemake{python_version >= '3.12'}==7.9.0: datrie*
DEBUG Adding transitive dependency for snakemake{python_version >= '3.12'}==7.9.0: jsonschema*
```

---

_Comment by @charliermarsh on 2024-06-26 00:56_

I think `python_version >= '3.12'` is the `requires-python` we infer from the interpreter, right?

---

_Comment by @konstin on 2024-07-01 08:54_

We now correctly log markers once at the beginning and the end of a fork and otherwise only when they are distinct

---

_Closed by @konstin on 2024-07-01 08:54_

---
