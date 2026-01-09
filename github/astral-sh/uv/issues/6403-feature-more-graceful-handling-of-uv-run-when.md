---
number: 6403
title: "Feature: more graceful handling of `uv run` when CLIs aren't well exposed."
type: issue
state: open
author: strangemonad
labels:
  - error messages
  - cli
  - uv tool
assignees: []
created_at: 2024-08-22T03:14:21Z
updated_at: 2025-03-27T11:49:15Z
url: https://github.com/astral-sh/uv/issues/6403
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature: more graceful handling of `uv run` when CLIs aren't well exposed.

---

_Issue opened by @strangemonad on 2024-08-22 03:14_

I don't have a great solution but wanted to call out some uvx rough edges. Some popular python packages don't expose conventionally named CLI entrypoints or rely on entrypoints from a transitive dependency.

Some examples:

`jupyterlab` doesn't provide meaningful info of how to get to a solution.

```
❯ uvx jupyterlab
Installed 91 packages in 226ms
The executable `jupyterlab` was not found.
warning: An executable named `jupyterlab` is not provided by package `jupyterlab`.
The following executables are provided by `jupyterlab`:
- jlpm
- jupyter-lab
- jupyter-labextension
- jupyter-labhub

~ 3s
```

`jupyter` does seem to work but gives a confusing warning

```
❯ uvx jupyter
Installed 99 packages in 254ms
warning: An executable named `jupyter` is not provided by package `jupyter` but is available via the dependency `jupyter-core`. Consider using `uvx --from jupyter-core jupyter` instead.
... # Successful run otherwise
```

`dbt` You sort of have to know that the warning means use `--from`

```
❯ uvx dbt-core
The executable `dbt-core` was not found.
warning: An executable named `dbt-core` is not provided by package `dbt-core`.
The following executables are provided by `dbt-core`:
- dbt

~
❯ uvx --from dbt-core dbt
... # Successful run otherwise
```

---

_Comment by @charliermarsh on 2024-08-22 03:34_

Thanks, these are great examples!

The last one seems easiest to fix (we can recommend `--from` there with a command you can copy-paste).

The Jupyter one is somewhat rough. I think the correct invocation is: `uvx --with jupyter --from jupyter-core jupyter notebook`. Because `jupyter-core` ships the executable, and then dynamically figures out which commands are enabled -- so you _do_ need to install `jupyter` in that environment, but it doesn't contain the executable.

---

_Comment by @charliermarsh on 2024-08-22 03:34_

\cc @zanieb 

---

_Label `cli` added by @charliermarsh on 2024-08-22 03:34_

---

_Comment by @zanieb on 2024-08-22 04:01_

Thank you!

I wonder what the `pipx` invocation for Jupyter is? 

---

_Label `error messages` added by @zanieb on 2024-08-22 04:02_

---

_Label `uv tool` added by @zanieb on 2024-08-22 04:02_

---

_Comment by @strangemonad on 2024-08-22 15:13_

@zanieb it's equally convoluted for pipx, which I know really comes down to how the package authors choose to expose things. What I've done in the past is something like

```
pipx install --include-deps jupyter # to grap `jupyter` from `jupyter-core`
pipx inject jupyter --include-apps --include-deps jupyter-lab
```

That ends up installing too many entrypoints but at least it's been reliable across the last few versions.

---

_Comment by @zanieb on 2024-08-22 16:07_

😬 Yeah that's pretty wild too. Thanks for sharing!

---

_Comment by @zanieb on 2024-08-22 16:08_

Seems like Jupyter should ship a CLI package that does something reasonable.. haha

---

_Referenced in [astral-sh/uv#6922](../../astral-sh/uv/issues/6922.md) on 2024-09-02 18:11_

---

_Comment by @johalnes on 2025-03-23 06:05_

@zanieb I'm struggling a bit with this now regarding dbt-core, and is thinking about contributing to the project. Could you please write some lines on what one have to do to defined what you would call a reasonable CLI, and what to avoid? Any guides or sources I could read up on? 😄 

---

_Comment by @strangemonad on 2025-03-27 02:14_

@johalnes I solve dbt-core differently for my data eng teams / practitioners. I haven't seen this widely adopted but it's very fool proof.

1. You need some way to bootstrap installing uv (eg a readme doc or whatever tooling you use to bootstrap a dev env)
2. make the dbt project also be a uv project with a pyproject.toml
3. add things like the pinned version of dbt and sql fluff as dev dependencies.

Optionally, you can expose shortcuts for familiar commands like run, build, docs, test as script entrypoints (or in our case we use `just` as a task runner). This also means it you want to have any dbt python models that depend on python code elsewhere in your codebase, you can add that as a pyproject dep and `uv sync` takes care of things.


`pyproject.toml` (sibling to `dbt_project.yml`)
```toml
[project]
name = "dbt"
version = "0.1.0"
description = "Dev env dependencies for the instance dbt project"
readme = "README.md"
requires-python = ">=3.11"

[tool.sqlfluff.core]
dialect = "snowflake"

dependencies = []

[dependency-groups]
dev = [
    # Pinning DBT related versions. Upgrades require coordination.
    "dbt-core==1.9.1",
    "dbt-snowflake>=1.8.4",
    "snowflake-connector-python[secure-local-storage]",
    "sqlfluff",
]
```


`justfile`
```just
import '../../../../tools/common_python_uv_tasks.just'

set allow-duplicate-recipes

run *args: sync
  uv run dbt {{args}}

alias dbt := run

fmt: sync
  uv run sqlfluff format

# Note: generate-sources still needs some love
generate-sources: sync
  poetry run dbt --quiet run-operation generate_source --args '{ \
            "database_name": "instance_raw", \
            "schema_name": "firestore", \
            "generate_columns": true, \
            "include_data_types": true, \
            "table_names": [ \
                "accounts_raw_latest", \
                "customer_orders_raw_latest", \
                "dna_pool_orders_raw_latest", \
                "groups_raw_latest", \
                "oligo_libraries_raw_latest", \
                "runs_raw_latest", \
                "sub_pool_run_plans__subpools__encoded_genes_raw_latest", \
                "sub_pool_run_plans__subpools_raw_latest", \
                "sub_pool_run_plans_raw_latest", \
                "sub_pool_runs__subpool_seq_oa_test_data_raw_latest", \
                "sub_pool_runs_raw_latest", \
                "task_attempts_raw_latest" \
            ] \
         }' > models/sources/firestore.yml.gen && \
         mv models/sources/firestore.yml.gen models/sources/firestore.yml
```

---

_Comment by @johalnes on 2025-03-27 11:49_

Thanks for sharing @strangemonad! Really appreciate it, and will try something similar 😄 For now I've used the old `uv pip install dbt-databricks` which makes dbt cli works as usual. 

---
