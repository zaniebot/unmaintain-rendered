---
number: 5176
title: "`uv pip install` does not invoke a custom post-installation script"
type: issue
state: closed
author: rexledesma
labels: []
assignees: []
created_at: 2024-07-18T03:16:21Z
updated_at: 2024-07-18T03:55:43Z
url: https://github.com/astral-sh/uv/issues/5176
synced_at: 2026-01-10T01:23:46Z
---

# `uv pip install` does not invoke a custom post-installation script

---

_Issue opened by @rexledesma on 2024-07-18 03:16_

## Summary
Title says it all. See the `setup.py` for [`sdf-cli`](https://pypi.org/project/sdf-cli/0.3.9/#files) or [this StackOverflow post](https://stackoverflow.com/a/36902139) for an implementation of a Python package's post-installation script.

## Version

```
dagster on  master 🐍 (dagster) 
❯ uv --version                               
uv 0.2.9 (e9fc99e62 2024-06-06)
```

## Reproduction 

**Installation with `uv pip install`**:
```
dagster on  master 🐍 (dagster) 
❯ sdf
zsh: command not found: sdf

dagster on  master 🐍 (dagster) 
❯ uv pip install sdf-cli
Resolved 2 packages in 8ms
Installed 1 package in 2ms
 + sdf-cli==0.3.9

dagster on  master 🐍 (dagster) 
❯ sdf
zsh: command not found: sdf
```

**Installation with `pip install`**:
```
dagster on  master 🐍 (dagster) 
❯ pip install --upgrade --force-reinstall --no-cache-dir sdf-cli
Collecting sdf-cli
  Downloading sdf_cli-0.3.9.tar.gz (3.4 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting setuptools>=61.2 (from sdf-cli)
  Downloading setuptools-71.0.1-py3-none-any.whl.metadata (6.5 kB)
Downloading setuptools-71.0.1-py3-none-any.whl (2.2 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.2/2.2 MB 5.4 MB/s eta 0:00:00
Building wheels for collected packages: sdf-cli
  Building wheel for sdf-cli (pyproject.toml) ... done
  Created wheel for sdf-cli: filename=sdf_cli-0.3.9-py3-none-any.whl size=3480 sha256=67ccbaf725a0d2ebb1fbcb611ca2989762d971e27eb38b7f5959deb4866f78a4
  Stored in directory: /private/var/folders/c1/glmlq29s7k56h5stbslqg0100000gn/T/pip-ephem-wheel-cache-29err12w/wheels/a4/62/4c/f96e6ec1eca4b47887ad2ab8c41fa5df2709faec6bbb70167a
Successfully built sdf-cli
Installing collected packages: setuptools, sdf-cli
  Attempting uninstall: setuptools
    Found existing installation: setuptools 71.0.1
    Uninstalling setuptools-71.0.1:
      Successfully uninstalled setuptools-71.0.1
  Attempting uninstall: sdf-cli
    Found existing installation: sdf-cli 0.3.9
    Uninstalling sdf-cli-0.3.9:
      Successfully uninstalled sdf-cli-0.3.9
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
dagster-open-platform 0.0.1 requires dagster-insights, which is not installed.
dagster-databricks 1!0+dev requires databricks-sdk<0.9, but you have databricks-sdk 0.17.0 which is incompatible.
dagster-open-platform 0.0.1 requires pydantic>2, but you have pydantic 1.10.15 which is incompatible.
dlt 0.4.7 requires orjson<=3.9.10,>=3.6.7; platform_python_implementation != "PyPy", but you have orjson 3.10.3 which is incompatible.
Successfully installed sdf-cli-0.3.9 setuptools-71.0.1

[notice] A new release of pip is available: 24.0 -> 24.1.2
[notice] To update, run: python3.10 -m pip install --upgrade pip

dagster on  master 🐍 (dagster) took 12s 
❯ sdf
SDF: A fast SQL compiler, local development framework, and in-memory analytical database

Usage: sdf [OPTIONS] <COMMAND>

Commands:
  new      Create a new sdf workspace
  clean    Remove artifacts that sdf has generated in the past
  compile  Compile models
  run      Run models
  test     Test your models
  stats    Statistics for your data
  report   Report code quality
  check    Check code quality
  lineage  Display lineage for a given table and/or column
  push     Push a local workspace to the SDF Service
  system   System maintenance, install and update
  auth     Authenticate CLI to services like SDF, AWS, OpenAI, etc
  man      Display reference material, like the CLI, dialect specific functions, schemas for authoring and interchange
  dbt      Initialize an sdf workspace from an existing dbt project
  help     Print this message or the help of the given subcommand(s)

Options:
      --log-level <LOG_LEVEL>  Set log level [possible values: trace, debug, info, warn, error]
      --log-file <LOG_FILE>    Creates or replaces the log file
      --log-form <LOG_FORM>    Set the log format; default flat [possible values: flat, nested, pretty]
  -h, --help                   Print help (see more with '--help')
  -V, --version                Print version
```


---

_Comment by @rexledesma on 2024-07-18 03:40_

Looks like this is solved by using `uv pip install --no-build-isolation`!

---

_Closed by @rexledesma on 2024-07-18 03:40_

---

_Comment by @zanieb on 2024-07-18 03:55_

ref #2252 

---
