---
number: 12321
title: linter shows errors for local file imports after switching from poetry to uv
type: issue
state: closed
author: hem9984
labels:
  - question
assignees: []
created_at: 2025-03-19T18:15:24Z
updated_at: 2025-03-20T15:11:56Z
url: https://github.com/astral-sh/uv/issues/12321
synced_at: 2026-01-10T01:25:18Z
---

# linter shows errors for local file imports after switching from poetry to uv

---

_Issue opened by @hem9984 on 2025-03-19 18:15_

### Summary

After switching, the linter shows these errors which all pertain to local file imports. And when I switch back to the poetry environment, they go away. What am I doing wrong? why are local file imports coming up as lint errors?

here is my current pyproject.toml:
```# ./backend/pyproject.toml

[project]
name = "azlon"
version = "0.0.1"
description = "autonomous coding project solver"
readme = "readme.md"
authors = [
    { name = "Harrison E. Muchnic", email = "hem9984@nyu.edu" },
]
requires-python = ">=3.10,<4.0"
dependencies = [
    "restack-ai >=0.0.85, <1.0.0",
    "pydantic >=2.10.3, <3.0.0",
    "fastapi==0.115.4",
    "uvicorn >=0.22.0, <1.0.0",
    "baml-py >=0.78.0, <1.0.0",
    "kaggle >=1.6.17, <2.0.0",
    "mem0ai >=0.1.56, <1.0.0",
    "e2b >=1.1.0, <2.0.0",
    "boto3 >=1.37.6, <2.0.0",
    "python-multipart >=0.0.20, <1.0.0",
    "python-dotenv",
    "botocore>=1.37.15",
    "pytest>=7.4.4",
]

[project.scripts]
services = "src.services:run_services"
schedule = "schedule_workflow:run_schedule_workflow"

[dependency-groups]
dev = [
    "pytest >=7.0.0, <8.0.0",
    "pytest-asyncio >=0.21.1, <1.0.0",
    "pytest-cov >=4.1.0, <5.0.0",
    "autoflake >=2.2.1, <3.0.0",
    "black >=23.7.0, <24.0.0",
    "isort >=5.12.0, <6.0.0",
    "mypy >=1.5.1, <2.0.0",
]

[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

# Configure setuptools to use the "src" directory for package discovery.
[tool.setuptools]
package-dir = { "" = "src" }

[tool.setuptools.packages.find]
where = ["src"]

[tool.uv]
default-groups = []

# Code style tools configuration
[tool.autoflake]
remove-all-unused-imports = true
remove-unused-variables = true
remove-duplicate-keys = true
in-place = true
recursive = true
expand-star-imports = true
check = false

[tool.black]
line-length = 100
target-version = ["py310"]

[tool.isort]
profile = "black"
line_length = 100

[tool.pytest.ini_options]
asyncio_mode = "auto"
```
### Here is the linter error for local imports:
[{
	"resource": "/home/harrison/AzlonBackend/backend/src/agents/main_agent.py",
	"owner": "python",
	"code": {
		"value": "reportMissingImports",
		"target": {
			"$mid": 1,
			"path": "/v1.28.0/configuration/config-files/",
			"scheme": "https",
			"authority": "docs.basedpyright.com",
			"fragment": "reportMissingImports"
		}
	},
	"severity": 8,
	"message": "Import \"src.workflows.workflow\" could not be resolved",
	"source": "windsurfPyright",
	"startLineNumber": 9,
	"startColumn": 6,
	"endLineNumber": 9,
	"endColumn": 28
},{
	"resource": "/home/harrison/AzlonBackend/backend/src/agents/main_agent.py",
	"owner": "python",
	"code": {
		"value": "reportMissingImports",
		"target": {
			"$mid": 1,
			"path": "/v1.28.0/configuration/config-files/",
			"scheme": "https",
			"authority": "docs.basedpyright.com",
			"fragment": "reportMissingImports"
		}
	},
	"severity": 8,
	"message": "Import \"src.functions.cloud_storage_functions\" could not be resolved",
	"source": "windsurfPyright",
	"startLineNumber": 14,
	"startColumn": 10,
	"endLineNumber": 14,
	"endColumn": 47
},{
	"resource": "/home/harrison/AzlonBackend/backend/src/functions/functions.py",
	"owner": "python",
	"code": {
		"value": "reportMissingImports",
		"target": {
			"$mid": 1,
			"path": "/v1.28.0/configuration/config-files/",
			"scheme": "https",
			"authority": "docs.basedpyright.com",
			"fragment": "reportMissingImports"
		}
	},
	"severity": 8,
	"message": "Import \"src.baml_client.async_client\" could not be resolved",
	"source": "windsurfPyright",
	"startLineNumber": 8,
	"startColumn": 6,
	"endLineNumber": 8,
	"endColumn": 34
},{
	"resource": "/home/harrison/AzlonBackend/backend/src/functions/functions.py",
	"owner": "python",
	"code": {
		"value": "reportMissingImports",
		"target": {
			"$mid": 1,
			"path": "/v1.28.0/configuration/config-files/",
			"scheme": "https",
			"authority": "docs.basedpyright.com",
			"fragment": "reportMissingImports"
		}
	},
	"severity": 8,
	"message": "Import \"src.baml_client.types\" could not be resolved",
	"source": "windsurfPyright",
	"startLineNumber": 9,
	"startColumn": 6,
	"endLineNumber": 9,
	"endColumn": 27
},{
	"resource": "/home/harrison/AzlonBackend/backend/src/functions/functions.py",
	"owner": "python",
	"code": {
		"value": "reportMissingImports",
		"target": {
			"$mid": 1,
			"path": "/v1.28.0/configuration/config-files/",
			"scheme": "https",
			"authority": "docs.basedpyright.com",
			"fragment": "reportMissingImports"
		}
	},
	"severity": 8,
	"message": "Import \"src.functions.cloud_storage_functions\" could not be resolved",
	"source": "windsurfPyright",
	"startLineNumber": 15,
	"startColumn": 6,
	"endLineNumber": 15,
	"endColumn": 43
},{
	"resource": "/home/harrison/AzlonBackend/backend/src/functions/functions.py",
	"owner": "python",
	"code": {
		"value": "reportMissingImports",
		"target": {
			"$mid": 1,
			"path": "/v1.28.0/configuration/config-files/",
			"scheme": "https",
			"authority": "docs.basedpyright.com",
			"fragment": "reportMissingImports"
		}
	},
	"severity": 8,
	"message": "Import \"src.utils.e2b_functions\" could not be resolved",
	"source": "windsurfPyright",
	"startLineNumber": 16,
	"startColumn": 6,
	"endLineNumber": 16,
	"endColumn": 29
},{
	"resource": "/home/harrison/AzlonBackend/backend/src/functions/functions.py",
	"owner": "python",
	"code": {
		"value": "reportMissingImports",
		"target": {
			"$mid": 1,
			"path": "/v1.28.0/configuration/config-files/",
			"scheme": "https",
			"authority": "docs.basedpyright.com",
			"fragment": "reportMissingImports"
		}
	},
	"severity": 8,
	"message": "Import \"src.utils.file_handling\" could not be resolved",
	"source": "windsurfPyright",
	"startLineNumber": 17,
	"startColumn": 6,
	"endLineNumber": 17,
	"endColumn": 29
},{
	"resource": "/home/harrison/AzlonBackend/backend/src/functions/functions.py",
	"owner": "python",
	"code": {
		"value": "reportMissingImports",
		"target": {
			"$mid": 1,
			"path": "/v1.28.0/configuration/config-files/",
			"scheme": "https",
			"authority": "docs.basedpyright.com",
			"fragment": "reportMissingImports"
		}
	},
	"severity": 8,
	"message": "Import \"src.utils.memory_manager\" could not be resolved",
	"source": "windsurfPyright",
	"startLineNumber": 18,
	"startColumn": 6,
	"endLineNumber": 18,
	"endColumn": 30
},{
	"resource": "/home/harrison/AzlonBackend/backend/src/utils/e2b_functions.py",
	"owner": "python",
	"code": {
		"value": "reportMissingImports",
		"target": {
			"$mid": 1,
			"path": "/v1.28.0/configuration/config-files/",
			"scheme": "https",
			"authority": "docs.basedpyright.com",
			"fragment": "reportMissingImports"
		}
	},
	"severity": 8,
	"message": "Import \"src.functions.cloud_storage_functions\" could not be resolved",
	"source": "windsurfPyright",
	"startLineNumber": 14,
	"startColumn": 6,
	"endLineNumber": 14,
	"endColumn": 43
}]

### Platform

Ubuntu 24

### Version

0.68

### Python version

Python 3.12.3


.
├── backend
│   ├── baml_src
│   │   ├── clients.baml
│   │   ├── generate_code.baml
│   │   ├── generators.baml
│   │   ├── resume.baml
│   │   ├── schema.baml
│   │   └── validate_output.baml
│   ├── dist
│   │   ├── azlon-0.0.1-py3-none-any.whl
│   │   └── azlon-0.0.1.tar.gz
│   ├── Dockerfile
│   ├── __init__.py
│   ├── LICENSE
│   ├── main.py
│   ├── Makefile
│   ├── __pycache__
│   │   ├── __init__.cpython-311.pyc
│   │   ├── __init__.cpython-312.pyc
│   │   ├── main.cpython-312.pyc
│   │   └── schedule_workflow.cpython-312.pyc
│   ├── pyproject.toml
│   ├── readme.md
│   ├── schedule_workflow.py
│   ├── src
│   │   ├── agents
│   │   │   ├── __init__.py
│   │   │   └── main_agent.py
│   │   ├── azlon.egg-info
│   │   │   ├── dependency_links.txt
│   │   │   ├── entry_points.txt
│   │   │   ├── PKG-INFO
│   │   │   ├── requires.txt
│   │   │   ├── SOURCES.txt
│   │   │   └── top_level.txt
│   │   ├── baml_client
│   │   │   ├── async_client.py
│   │   │   ├── globals.py
│   │   │   ├── __init__.py
│   │   │   ├── inlinedbaml.py
│   │   │   ├── partial_types.py
│   │   │   ├── __pycache__
│   │   │   │   ├── async_client.cpython-311.pyc
│   │   │   │   ├── async_client.cpython-312.pyc
│   │   │   │   ├── globals.cpython-311.pyc
│   │   │   │   ├── globals.cpython-312.pyc
│   │   │   │   ├── __init__.cpython-311.pyc
│   │   │   │   ├── __init__.cpython-312.pyc
│   │   │   │   ├── inlinedbaml.cpython-311.pyc
│   │   │   │   ├── inlinedbaml.cpython-312.pyc
│   │   │   │   ├── partial_types.cpython-311.pyc
│   │   │   │   ├── partial_types.cpython-312.pyc
│   │   │   │   ├── tracing.cpython-311.pyc
│   │   │   │   ├── tracing.cpython-312.pyc
│   │   │   │   ├── type_builder.cpython-311.pyc
│   │   │   │   ├── type_builder.cpython-312.pyc
│   │   │   │   ├── types.cpython-311.pyc
│   │   │   │   └── types.cpython-312.pyc
│   │   │   ├── sync_client.py
│   │   │   ├── tracing.py
│   │   │   ├── type_builder.py
│   │   │   └── types.py
│   │   ├── client.py
│   │   ├── functions
│   │   │   ├── cloud_storage_functions.py
│   │   │   ├── functions.py
│   │   │   ├── __init__.py
│   │   │   └── __pycache__
│   │   │       ├── cloud_storage_functions.cpython-311.pyc
│   │   │       ├── cloud_storage_functions.cpython-312.pyc
│   │   │       ├── functions.cpython-311.pyc
│   │   │       ├── functions.cpython-312.pyc
│   │   │       ├── __init__.cpython-311.pyc
│   │   │       ├── __init__.cpython-312.pyc
│   │   │       └── minio_functions.cpython-312.pyc
│   │   ├── __init__.py
│   │   ├── __pycache__
│   │   │   ├── client.cpython-311.pyc
│   │   │   ├── client.cpython-312.pyc
│   │   │   ├── e2b_functions.cpython-312.pyc
│   │   │   ├── file_server.cpython-312.pyc
│   │   │   ├── __init__.cpython-311.pyc
│   │   │   ├── __init__.cpython-312.pyc
│   │   │   ├── memory_manager.cpython-311.pyc
│   │   │   ├── memory_manager.cpython-312.pyc
│   │   │   └── prompts.cpython-312.pyc
│   │   ├── services.py
│   │   ├── utils
│   │   │   ├── e2b_functions.py
│   │   │   ├── file_handling.py
│   │   │   ├── file_handling.py.bak
│   │   │   ├── __init__.py
│   │   │   ├── memory_manager.py
│   │   │   └── __pycache__
│   │   │       ├── e2b_functions.cpython-311.pyc
│   │   │       ├── e2b_functions.cpython-312.pyc
│   │   │       ├── file_handling.cpython-311.pyc
│   │   │       ├── file_handling.cpython-312.pyc
│   │   │       ├── __init__.cpython-311.pyc
│   │   │       ├── __init__.cpython-312.pyc
│   │   │       ├── memory_manager.cpython-311.pyc
│   │   │       └── memory_manager.cpython-312.pyc
│   │   └── workflows
│   │       ├── __init__.py
│   │       ├── __pycache__
│   │       │   ├── __init__.cpython-311.pyc
│   │       │   ├── __init__.cpython-312.pyc
│   │       │   ├── workflow.cpython-311.pyc
│   │       │   └── workflow.cpython-312.pyc
│   │       └── workflow.py
│   ├── tests
│   │   ├── conftest.py
│   │   ├── kaggle
│   │   │   └── scrape_kaggle.py
│   │   ├── __pycache__
│   │   │   ├── conftest.cpython-311-pytest-8.3.5.pyc
│   │   │   ├── conftest.cpython-312-pytest-7.4.4.pyc
│   │   │   ├── minio_config_tester.cpython-312.pyc
│   │   │   ├── tailscale_diagnostics.cpython-312.pyc
│   │   │   ├── tailscale_funnel_config.cpython-312.pyc
│   │   │   ├── test_cloud_storage_functions.cpython-311-pytest-8.3.5.pyc
│   │   │   ├── test_cloud_storage_functions.cpython-312-pytest-7.4.4.pyc
│   │   │   ├── test_direct_s3.cpython-312.pyc
│   │   │   ├── test_e2b_functions.cpython-311-pytest-8.3.5.pyc
│   │   │   ├── test_e2b_functions.cpython-312.pyc
│   │   │   ├── test_e2b_functions.cpython-312-pytest-7.4.4.pyc
│   │   │   ├── test_file_handling.cpython-311-pytest-8.3.5.pyc
│   │   │   ├── test_file_handling.cpython-312-pytest-7.4.4.pyc
│   │   │   ├── test_file_server.cpython-312-pytest-7.4.4.pyc
│   │   │   ├── test_functions.cpython-311-pytest-8.3.5.pyc
│   │   │   ├── test_functions.cpython-312-pytest-7.4.4.pyc
│   │   │   ├── test_live_gcs.cpython-311-pytest-8.3.5.pyc
│   │   │   ├── test_live_gcs.cpython-312-pytest-7.4.4.pyc
│   │   │   ├── test_live_minio.cpython-312.pyc
│   │   │   ├── test_main.cpython-312-pytest-7.4.4.pyc
│   │   │   ├── test_tailscale_minio.cpython-312.pyc
│   │   │   ├── test_workflow.cpython-311-pytest-8.3.5.pyc
│   │   │   └── test_workflow.cpython-312-pytest-7.4.4.pyc
│   │   ├── test_cloud_storage_functions.py
│   │   ├── test_e2b_functions.py
│   │   ├── test_file_handling.py
│   │   ├── test_functions.py
│   │   ├── test_live_gcs.py
│   │   └── test_workflow.py
│   └── uv.lock
├── cleanup.sh
├── docker-compose.yml
├── __init__.py
├── LICENSE
├── llm-output
│   ├── test-local
│   └── test-local-direct
├── old_README.md
├── plan.md
├── __pycache__
│   └── __init__.cpython-311.pyc
├── simple_test.sh
└── start_server.sh

---

_Label `bug` added by @hem9984 on 2025-03-19 18:15_

---

_Comment by @zanieb on 2025-03-19 18:32_

Could you use backticks for code blocks (```)?

It would also be helpful to minimize your example as discussed in #9452 / https://docs.astral.sh/uv/reference/troubleshooting/reproducible-examples/

---

_Comment by @hem9984 on 2025-03-19 18:36_

> Could you use backticks for code blocks (```)?
> 
> It would also be helpful to minimize your example as discussed in [#9452](https://github.com/astral-sh/uv/issues/9452) / https://docs.astral.sh/uv/reference/troubleshooting/reproducible-examples/

I revised the markdown formatting as requested.

---

_Comment by @zanieb on 2025-03-19 18:37_

How are you invoking the linter?

---

_Comment by @hem9984 on 2025-03-19 18:38_

> How are you invoking the linter?

uv sync. Vscode is just showing it under "problems". when I switch back to poetry env they go away. when I switch back to uv env they come back.

---

_Comment by @zanieb on 2025-03-19 18:50_

Are you changing VSCode to use the uv `.venv`?

---

_Comment by @hem9984 on 2025-03-19 18:56_

> Are you changing VSCode to use the uv `.venv`?

yes. as stated in my previous comment, it only happens when I change to the uv venv. I am suspecting now that it is because the venv is not in the root directory (see directory tree). Would that be the issue? then why does it work in poetry but not uv?

---

_Comment by @zanieb on 2025-03-19 19:00_

Ah, that'd be a case of #9637 which seems like a VS Code bug. I think VS Code has "special" support for Poetry? The linked issue has some workarounds I think. It's unclear to me when the VS Code team will fix it.


---

_Label `bug` removed by @zanieb on 2025-03-19 21:30_

---

_Label `question` added by @zanieb on 2025-03-19 21:30_

---

_Comment by @hem9984 on 2025-03-20 15:11_

conclusion is that the uv venv needs to be in root directory for linter to work properly

---

_Closed by @hem9984 on 2025-03-20 15:11_

---
