---
number: 8719
title: uv tree in workspaces including packages in dependency groups
type: issue
state: closed
author: jackvreeken
labels:
  - bug
assignees: []
created_at: 2024-10-31T10:55:25Z
updated_at: 2024-10-31T20:09:37Z
url: https://github.com/astral-sh/uv/issues/8719
synced_at: 2026-01-07T13:12:18-06:00
---

# uv tree in workspaces including packages in dependency groups

---

_Issue opened by @jackvreeken on 2024-10-31 10:55_

Running uv version `uv 0.4.28 (debe67ffd 2024-10-28)`

I have two questions:
- Why do I need an exclude for `["src/*.egg-info"]`
- Why is `uv tree --no-dev` including packages of the workspace

Also generally not quite sure how to structure my project; essentially a AWS CDK project with a bunch of Lambdas that need to be packages (each with only their own dependencies). https://github.com/jackvreeken/uv-export-test/tree/e0af8ba899a58f1ec326796f9fce36355301c5c2  

My goals are to
- Have a single `uv sync` install all the dependencies, including those required to mock-run the lambdas locally
- Make it easy to export the requirements.txt for a specific lambda (need this format for CDK) Simplified setup of that here: 


### Project structure 

```
.
├── pyproject.toml
├── src
│   ├── lambda_a
│   │   ├── __init__.py
│   │   ├── handler_a.py
│   │   └── pyproject.toml
│   └── lambda_b
│       ├── __init__.py
│       ├── handler_b.py
│       └── pyproject.toml
└── uv.lock
```

### root pyproject.toml
EDIT: Included the `[project]` section.
```
[project]
name = "uv-export-test"
dynamic = ["version"]
requires-python = "~=3.12"

[dependency-groups]
dev = [
    "pre-commit >= 4.0.1",
    "lambda-a",
    "lambda-b",
]

[tool.uv.sources]
lambda-a = { workspace = true }
lambda-b = { workspace = true }

[tool.uv.workspace]
members = ["src/*"]
exclude = ["src/*.egg-info"]
```

### The issue
This results in `uv sync` installing all the dependencies, so that's great.
`uv tree --no-dev` includes the `lambda-a` and `lambda-b` subtrees, which I don't understand. Leaving out the `--no-dev` option adds the dep tree for `pre-commit` at the bottom.

```
uv-export-test v0.0.0
├── lambda-a v0.0.0 (group: dev)
│   ├── boto3 v1.35.52
│   │   ├── botocore v1.35.52
│   │   │   ├── jmespath v1.0.1
│   │   │   ├── python-dateutil v2.9.0.post0
│   │   │   │   └── six v1.16.0
│   │   │   └── urllib3 v2.2.3
│   │   ├── jmespath v1.0.1
│   │   └── s3transfer v0.10.3
│   │       └── botocore v1.35.52 (*)
│   ├── pydantic v2.9.2
│   │   ├── annotated-types v0.7.0
│   │   ├── pydantic-core v2.23.4
│   │   │   └── typing-extensions v4.12.2
│   │   └── typing-extensions v4.12.2
│   └── urllib3 v2.2.3
└── lambda-b v0.0.0 (group: dev)
    └── boto3 v1.35.52 (*)
```

Note that without `lambda-a` and `lambda-b` in the dev group, the output of `uv tree` shows `lambda-a` and `lambda-b` at the root level, which feels a little confusing.

```
lambda-a v0.0.0
├── boto3 v1.35.52
...
lambda-b v0.0.0
└── boto3 v1.35.52 (*)
uv-export-test v0.0.0
└── pre-commit v4.0.1 (group: dev)
...
```

Aside from that, I also wonder why I need the `exclude = ["src/*.egg-info"]`. For some reason a `uv_export_test.egg-info` folder is made in `src/`


---

_Comment by @charliermarsh on 2024-10-31 12:55_

To clarify, is that the entire `pyproject.toml`? The tree below suggests you have more content in it, like a `[project]` table.

---

_Label `question` added by @charliermarsh on 2024-10-31 12:59_

---

_Comment by @jackvreeken on 2024-10-31 14:10_

Totally right! Also some other sections (`tool.ruff`), but didn't want to clutter too much. Apparently also deleted the `[project]` section in the process 🙈

---

_Comment by @charliermarsh on 2024-10-31 16:11_

I totally missed that you included a repro link. Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-31 16:11_

---

_Label `question` removed by @charliermarsh on 2024-10-31 16:12_

---

_Label `bug` added by @charliermarsh on 2024-10-31 16:12_

---

_Referenced in [astral-sh/uv#8730](../../astral-sh/uv/pulls/8730.md) on 2024-10-31 17:35_

---

_Comment by @charliermarsh on 2024-10-31 17:36_

Ok, I've fixed it in https://github.com/astral-sh/uv/pull/8730. Though it's correct to still show `lambda-a` and `lambda-b` at the top-level. It's a little confusing, but `uv tree` shows you the entire workspace. So, in my test case, where `project` depends on `child` as a dev dependency, I get the following with `--no-dev`:

```
child v0.1.0
└── iniconfig v2.0.0
project v0.1.0
└── anyio v4.3.0
    ├── idna v3.6
    └── sniffio v1.3.1
```

If you want to see _just_ the tree for project, you need to pass `--project project` (or `--project child`, etc.).


---

_Closed by @charliermarsh on 2024-10-31 20:09_

---
