```yaml
number: 8902
title: "uv tree & uv pip tree don't show the correct package names."
type: issue
state: closed
author: dsully
labels:
  - question
assignees: []
created_at: 2024-11-07T23:04:07Z
updated_at: 2024-11-09T02:17:30Z
url: https://github.com/astral-sh/uv/issues/8902
synced_at: 2026-01-10T04:36:20Z
```

# uv tree & uv pip tree don't show the correct package names.

---

_Issue opened by @dsully on 2024-11-07 23:04_

`uv tree` and `uv pip tree` don't show "official" package names from an installed `METADATA` file.

Output of `uv tree`:

```shell
$ uv tree
Resolved 22 packages in 9ms
lipy-version v5.0.19
├── jinja2 v3.1.4
│   └── markupsafe v3.0.2
├── typing-extensions v4.12.2
├── ruff v0.7.0 (extra: optional)
├── flake8 v7.1.1 (group: dev)
│   ├── mccabe v0.7.0
│   ├── pycodestyle v2.12.1
│   └── pyflakes v3.2.0
├── isort v5.13.2 (group: dev)
├── mypy v1.12.0 (group: dev)
│   ├── mypy-extensions v1.0.0
│   └── typing-extensions v4.12.2
├── pytest v8.3.3 (group: dev)
│   ├── iniconfig v2.0.0
│   ├── packaging v24.1
│   └── pluggy v1.5.0
├── pytest-cov v5.0.0 (group: dev)
│   ├── coverage[toml] v7.6.2
│   └── pytest v8.3.3 (*)
└── sybil v3.0.1 (group: dev)
(*) Package tree already displayed
```

vs the output of `pipdeptree`:

```shell
$ pipdeptree
flake8==7.1.1
├── mccabe [required: >=0.7.0,<0.8.0, installed: 0.7.0]
├── pycodestyle [required: >=2.12.0,<2.13.0, installed: 2.12.1]
└── pyflakes [required: >=3.2.0,<3.3.0, installed: 3.2.0]
isort==5.13.2
Jinja2==3.1.4
└── MarkupSafe [required: >=2.0, installed: 3.0.2]
mypy==1.12.0
├── mypy_extensions [required: >=1.0.0, installed: 1.0.0]
└── typing_extensions [required: >=4.6.0, installed: 4.12.2]
pipdeptree==2.23.4
├── packaging [required: >=24.1, installed: 24.1]
└── pip [required: >=24.2, installed: 24.3.1]
pytest-cov==5.0.0
├── coverage [required: >=5.2.1, installed: 7.6.2]
└── pytest [required: >=4.6, installed: 8.3.3]
    ├── iniconfig [required: Any, installed: 2.0.0]
    ├── packaging [required: Any, installed: 24.1]
    └── pluggy [required: >=1.5,<2, installed: 1.5.0]
sybil==3.0.1
```

Notice both `Jinja2` and `MarkupSafe` have the correct capitalization.

```shell
$ grep Name: MarkupSafe-3.0.2.dist-info/METADATA
Name: MarkupSafe
```

---

_Comment by @zanieb on 2024-11-07 23:06_

Yeah I think we use normalized names ~everywhere. It'd probably be reasonable to preserve the original names, but it sounds pretty painful to implement.

---

_Comment by @charliermarsh on 2024-11-07 23:07_

Yeah this is intentional.

---

_Comment by @dsully on 2024-11-07 23:29_

Ok.. we track dependency relationships via an internal service (language independent), and for it to work properly, we need the names that are canonical from each index.

---

_Comment by @notatallshaw on 2024-11-07 23:36_

If you know all packages that are involved you could create a mapping of normalized to canonical names yourself, the rules are quite simple: https://packaging.python.org/en/latest/specifications/name-normalization/

---

_Comment by @charliermarsh on 2024-11-07 23:37_

Why not use the spec-compliant normalized names? There’s no ambiguity to them, unlike the names uploaded to the index.

---

_Comment by @dsully on 2024-11-07 23:48_

I agree, but there's an unfortunate amount of legacy (Gradle, Ivy, pain) that I have to deal with.

---

_Label `question` added by @charliermarsh on 2024-11-09 02:17_

---

_Comment by @charliermarsh on 2024-11-09 02:17_

I'm going to close as I don't currently plan to change these. Sorry about that :(

---

_Closed by @charliermarsh on 2024-11-09 02:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-09 02:17_

---
