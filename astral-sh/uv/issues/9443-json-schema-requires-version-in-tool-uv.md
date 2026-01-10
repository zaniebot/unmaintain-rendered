```yaml
number: 9443
title: "JSON schema requires `version` in `tool.uv.dependency-metadata`"
type: issue
state: closed
author: zeevro
labels:
  - bug
assignees: []
created_at: 2024-11-26T15:10:19Z
updated_at: 2024-11-27T13:26:05Z
url: https://github.com/astral-sh/uv/issues/9443
synced_at: 2026-01-10T04:36:21Z
```

# JSON schema requires `version` in `tool.uv.dependency-metadata`

---

_Issue opened by @zeevro on 2024-11-26 15:10_

```
$ uvx --from validate-pyproject[all,store] validate-pyproject pyproject.toml -vv
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.black` schema
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.cibuildwheel` schema
[INFO] validate_pyproject.api.load_builtin_plugin defines `tool.distutils` schema
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.hatch` schema
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.mypy` schema
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.pdm` schema
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.poetry` schema
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.pyright` schema
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.ruff` schema
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.scikit-build` schema
[INFO] validate_pyproject.api.load_builtin_plugin defines `tool.setuptools` schema
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.setuptools_scm` schema
[INFO] validate_pyproject_schema_store.schema.get_schema defines `tool.uv` schema
Invalid file: pyproject.toml
[ERROR] `tool.uv.dependency-metadata[1]` must contain ['version'] properties

DESCRIPTION:
    A subset of the Python Package Metadata 2.3 standard as specified in
    <https://packaging.python.org/specifications/core-metadata/>.

GIVEN VALUE:
    {
        "name": "dkimpy",
        "requires-dist": [
            "dnspython>=2.0.0"
        ]
    }

OFFENDING RULE: 'required'

DEFINITION:
    {
        "type": "object",
        "required": [
            "name",
            "version"
        ],
        "properties": {
            "name": {
                "description": "The normalized name of a package.\n\nConverts the name to lowercase and collapses runs of `-`, `_`, and `.` down to a single `-`. For example, `---`, `.`, and `__` are all converted to a single `-`.\n\nSee: <https://packaging.python.org/en/latest/specifications/name-normalization/>",
                "type": "string"
            },
            "provides-extras": {
                "default": [],
                "type": "array",
                "items": {
                    "description": "The normalized name of an extra dependency.\n\nConverts the name to lowercase and collapses runs of `-`, `_`, and `.` down to a single `-`. For example, `---`, `.`, and `__` are all converted to a single `-`.\n\nSee: - <https://peps.python.org/pep-0685/#specification/> - <https://packaging.python.org/en/latest/specifications/name-normalization/>",
                    "type": "string"
                }
            },
            "requires-dist": {
                "default": [],
                "type": "array",
                "items": {
                    "description": "A PEP 508 dependency specifier, e.g., `ruff >= 0.6.0`",
                    "type": "string"
                }
            },
            "requires-python": {
                "description": "PEP 508-style Python requirement, e.g., `>=3.10`",
                "type": [
                    "string",
                    "null"
                ]
            },
            "version": {
                "description": "PEP 440-style package version, e.g., `1.2.3`",
                "type": "string"
            }
        }
    }
```

---

_Label `bug` added by @konstin on 2024-11-27 13:09_

---

_Closed by @konstin on 2024-11-27 13:26_

---
