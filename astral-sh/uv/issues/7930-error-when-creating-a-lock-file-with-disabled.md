```yaml
number: 7930
title: Error when creating a lock file with disabled package management mode
type: issue
state: open
author: nightblure
labels:
  - bug
assignees: []
created_at: 2024-10-04T17:08:32Z
updated_at: 2024-10-04T17:54:48Z
url: https://github.com/astral-sh/uv/issues/7930
synced_at: 2026-01-10T04:45:10Z
```

# Error when creating a lock file with disabled package management mode

---

_Issue opened by @nightblure on 2024-10-04 17:08_

Hi! I finally took the time to try using this amazing tool in one of the working services but found the following problem. When I try to create a lock file with **disabled package control mode** (`package = false` in _tool.uv_ section), I get an error:
```
DEBUG uv 0.4.18 (Homebrew 2024-10-01)
DEBUG Found workspace root: `/Users/ivan/Desktop/work/pixi`
DEBUG Adding current workspace member: `/Users/ivan/Desktop/work/pixi`
DEBUG The virtual environment's Python version satisfies `Python ==3.11.*`
DEBUG Using request timeout of 30s
DEBUG Starting clean resolution
DEBUG No static `pyproject.toml` available for: pixi @ file:///Users/ivan/Desktop/work/pixi (PyprojectToml(FieldNotFound("version")))
DEBUG Acquired lock for `/Users/ivan/Library/Caches/uv/sdists-v4/path/0ecbaf84fa935616`
WARN Failed to read metadata for file: No such file or directory (os error 2)
DEBUG Preparing metadata for: pixi @ file:///Users/ivan/Desktop/work/pixi
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.11.9
DEBUG Solving with target Python version: >=3.11.9
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==75.1.0 [compatible] (setuptools-75.1.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ff/ae/f19306b5a221f6a436d8f2238d5b80925004093fa3edea59835b514d9057/setuptools-75.1.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG Split specific environment resolution took 0.002s
DEBUG Installing in setuptools==75.1.0 in /Users/ivan/Library/Caches/uv/builds-v0/.tmp3pbodG
DEBUG Requirement already cached: setuptools==75.1.0
DEBUG Installing build requirement: setuptools==75.1.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Traceback (most recent call last):
DEBUG   File "<string>", line 14, in <module>
DEBUG   File "/Users/ivan/Library/Caches/uv/builds-v0/.tmp3pbodG/lib/python3.11/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
DEBUG     return self._get_build_requires(config_settings, requirements=[])
DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "/Users/ivan/Library/Caches/uv/builds-v0/.tmp3pbodG/lib/python3.11/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
DEBUG     self.run_setup()
DEBUG   File "/Users/ivan/Library/Caches/uv/builds-v0/.tmp3pbodG/lib/python3.11/site-packages/setuptools/build_meta.py", line 503, in run_setup
DEBUG     super().run_setup(setup_script=setup_script)
DEBUG   File "/Users/ivan/Library/Caches/uv/builds-v0/.tmp3pbodG/lib/python3.11/site-packages/setuptools/build_meta.py", line 318, in run_setup
DEBUG     exec(code, locals())
DEBUG   File "<string>", line 23, in <module>
DEBUG   File "<string>", line 18, in parse_requirements
DEBUG   File "<string>", line 13, in read_file
DEBUG   File "<frozen codecs>", line 918, in open
DEBUG FileNotFoundError: [Errno 2] No such file or directory: '/Users/ivan/Desktop/work/pixi/requirements.txt'
DEBUG Released lock at `/Users/ivan/Library/Caches/uv/sdists-v4/path/0ecbaf84fa935616/.lock`
error: Failed to build: `pixi @ file:///Users/ivan/Desktop/work/pixi`
  Caused by: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/ivan/Library/Caches/uv/builds-v0/.tmp3pbodG/lib/python3.11/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/ivan/Library/Caches/uv/builds-v0/.tmp3pbodG/lib/python3.11/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/Users/ivan/Library/Caches/uv/builds-v0/.tmp3pbodG/lib/python3.11/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/ivan/Library/Caches/uv/builds-v0/.tmp3pbodG/lib/python3.11/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 23, in <module>
  File "<string>", line 18, in parse_requirements
  File "<string>", line 13, in read_file
  File "<frozen codecs>", line 918, in open
FileNotFoundError: [Errno 2] No such file or directory: '/Users/ivan/Desktop/work/pixi/requirements.txt'
---
```

I think this is due to the fact that I **have a setup.py** file in the current directory that may be accessing a requirements.txt file that does not exist. here is part of the code from setup.py:
```python3
...

def parse_requirements():
    requirements_lines = [
        line.strip() for line in read_file('requirements.txt').strip().split('\n')
    ]
    return [line for line in requirements_lines if line and str.isalpha(line[0])]


REQUIREMENTS = parse_requirements()

...
```

And actually what prompted me to create this is that I specified in pyproject.toml the option to disable managing the project as a package, I assume that uv would not try to build the package, and therefore would not look into setup.py at all. My pyproject.toml:
```
[tool.ruff]
target-version = "py311"
extend-exclude = []
line-length = 88

[tool.ruff.format]
quote-style = "single"

select = [
    # RULES: https://docs.astral.sh/ruff/rules/
    "A",     # flake8-builtins
    "B",     # flake8-bugbear
    "C",     # flake8-comprehensions
    "E",     # pycodestyle errors
    "G",     # flake8-logging-format
    "F",     # pyflakes
    "I",     # isort
    "N",     # PEP8 naming
    "S",     # flake8-bandit
    "W",     # pycodestyle warnings
    "T20",   # flake8-print
    "C4",    # flake8-comprehensions
    "EM",    # flake8-errmsg
    "UP",    # pyupgrade
    "PL",    # Pylint
    "PT",    # flake8-pytest-style
    "ISC",   # flake8-implicit-str-concat
    "ICN",   # flake8-import-conventions
    "ARG",   # flake8-unused-arguments
    "COM",   # flake8-commas
    "FBT",   # flake8-boolean-trap
    "LOG",   # flake8-logging
    "SIM",   # flake8-simplify
    "TRY",   # tryceratops
    "PIE",   # flake8-pie
    "RUF",   # Ruff-specific rules
    "ASYNC", # flake8-async
]

[tool.ruff.lint.flake8-bugbear]
extend-immutable-calls = [
    "fastapi.Depends",
    "fastapi.params.Depends",
    "fastapi.Query",
    "fastapi.params.Query",
    "fastapi.Body",
    "fastapi.params.Body",
]

[tool.pdm]
distribution = false

[tool.pytest.ini_options]
addopts = "--asyncio-mode=auto"

[project]
name = "pixi"
dependencies = [
    "pillow==10.2.0",
    "requests==2.32.3",
    "schedule==1.2.1",
    "psycopg2-binary==2.9.9",
    "pymongo==4.6.2",
    "pymilvus==2.4.1",
    "SQLAlchemy==2.0.31",
    "fastapi==0.109.2",
    "pydantic==2.6.1",
    "pydantic-settings==2.1.0",
    "loguru==0.7.2",
    "alembic==1.13.1",
    "pydash==7.0.7",
    "pandas==2.2.1",
    "uvicorn==0.29.0",
    "PyYAML==6.0.1",
    "httpx==0.27.0",
    "dependency-injector==4.41.0",
]
requires-python = "==3.11.*"

[tool.uv]
package = false
dev-dependencies = [
    "pytest==8.1.1",
    "pytest-cov==5.0.0",
    "pytest-asyncio==0.23.6",
    "testcontainers==4.7.2",
    "coverage==7.4.4",
    "bump2version==1.*",
    "pre-commit==3.8.*",
    "Faker==25.8.0",
]
```
---
* The command you invoked: `uv lock --verbose`
* The current uv platform: macOS Sequoia 15.0 (M1 cpu)
* The current uv version (`uv --version`): `uv 0.4.18 (Homebrew 2024-10-01)`


---

_Comment by @zanieb on 2024-10-04 17:25_

The hint is here

> DEBUG No static `pyproject.toml` available for: pixi @ file:///Users/ivan/Desktop/work/pixi (PyprojectToml(FieldNotFound("version")))

Since we can't find a `version` field in your project, we need to ask a build backend to retrieve that metadata for us.

We should error or something here though since `package = false` is set.


---

_Label `bug` added by @zanieb on 2024-10-04 17:25_

---

_Comment by @nightblure on 2024-10-04 17:52_

upd

I deleted the setup.py file and got this message in its clear form:
```
ValueError: invalid pyproject.toml config: `project`.
configuration error: `project` must contain ['version'] properties
```

Could a good way out of this be a logic where uv would not ask for the version when we get option `package = false`? We may not require a version at all, or we may already manage versioning (not of a package, but of a web service in my case) using bump2version or any other utility. In PDM this is exactly how it works, because now I'm trying to switch from it to uv



---
