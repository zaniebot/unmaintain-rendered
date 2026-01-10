---
number: 11452
title: False Positive conflicting URLs for Git Package Dependencies with private packages
type: issue
state: open
author: keidikapllani
labels:
  - needs-mre
assignees: []
created_at: 2025-02-12T16:15:40Z
updated_at: 2025-05-28T13:04:55Z
url: https://github.com/astral-sh/uv/issues/11452
synced_at: 2026-01-10T01:25:05Z
---

# False Positive conflicting URLs for Git Package Dependencies with private packages

---

_Issue opened by @keidikapllani on 2025-02-12 16:15_

### Summary

## Environment Information
- UV Version: 0.5.30 (Homebrew 2025-02-10)
- Operating System: macOS (darwin 23.6.0)
- Python Version: 3.12.1

## Issue Description
When attempting to install dependencies, UV reports conflicting URLs for the `kernel-research-lib`, which is both a direct dependency, and a transitive dependency of package `pipeline-definitions` in the Python environment split `python_full_version >= '3.13' and platform_machine == 'x86_64' and sys_platform == 'darwin'`.

Both dependencies are internal private git packages, specified using `[tool.uv.sources]

The error occurs with identical URLs:
```
error: Requirements contain conflicting URLs for package `kernel-research-lib` in split `python_full_version >= '3.13' and platform_machine == 'x86_64' and sys_platform == 'darwin'`:
- git+https://github.com/kernlai/kernel-research-lib.git@v0.142.0
- git+https://github.com/kernlai/kernel-research-lib.git@v0.142.0
```

## Steps to Reproduce
1. Have a project with Git dependencies in pyproject.toml
2. The project includes dependencies on lib-a and lib-b where lib-b depends on lib-a
3. Run UV to install dependencies
4. Error occurs during dependency resolution

## Expected Behavior
UV should recognize that the URLs are identical and not treat them as conflicting requirements.

## Actual Behavior
UV reports an error about conflicting URLs even though they are exactly the same URL pointing to the same version (v0.142.0) of the package.

## Additional Context
- The error occurs during dependency resolution phase
## pyproject.toml
```toml
[project]
name = "kernel-research"
version = "0.1.0"
description = "The main repository for research work"
requires-python = ">=3.12"
authors = [
    { name = "Primer Team", email = "team@primerapp.com" },
]
dependencies = [
     "boto3>=1.36.16",
     "docling>=2.21.0",
     "kernel-research-lib",
     "pipeline-definitions",
     "pybars3>=0.9.7",
     "unidecode>=1.3.8",
]


[dependency-groups]
dev = [
    "autoapi>=2.0.1",
    "bandit>=1.8.0",
    "mypy>=1.13.0",
    "pre-commit>=4.0.1",
    "pytest>=8.3.4",
    "ruff>=0.8.3",
    "sphinx>=8.1.3",
    "pytest-asyncio>=0.25.0",
    "sphinx-rtd-theme>=3.0.2",
    "sphinx-autoapi>=3.4.0",
    "ipykernel>=6.29.5",
    "types-boto3>=1.0.2",
]

[tool.pytest.ini_options]
testpaths = "tests"

[tool.bandit]
exclude_dirs = ["tests"]



[tool.uv.sources.kernel-research-lib]
git = "https://github.com/kernlai/kernel-research-lib.git"
tag = "v0.142.0"

[tool.uv.sources.pipeline-definitions]
git = "https://github.com/kenrlai/pipeline-definitions.git"
tag = "v0.3.2"


```

## Verbose Logs

```bash
DEBUG uv 0.5.30 (Homebrew 2025-02-10)
DEBUG Found project root: `/Users/ruggerogargiulo/kernelai/research-env/repos/kernel-research`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/ruggerogargiulo/kernelai/research-env/repos/kernel-research`
DEBUG Reading Python requests from version file at `/Users/ruggerogargiulo/kernelai/research-env/repos/kernel-research/.python-version`
DEBUG Using Python request `3.12.1` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.12.1`
DEBUG Released lock at `/var/folders/55/2wd6g6b90bb6qnyszdntl4fr0000gn/T/uv-599b27c41021955f.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: kernel-research @ file:///Users/ruggerogargiulo/kernelai/research-env/repos/kernel-research
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched requirements for: `kernel-research==0.1.0`
  Requested: {Requirement { name: PackageName("boto3"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "1.36.16" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("docling"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "2.21.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("kernel-research-lib"), extras: [], groups: [], marker: true, source: Git { repository: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/kernel-research-lib.git", query: None, fragment: None }, reference: Tag("v0.142.0"), precise: None, subdirectory: None, url: VerbatimUrl { url: Url { scheme: "git+https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/kernel-research-lib.git@v0.142.0", query: None, fragment: None }, given: None } }, origin: None }, Requirement { name: PackageName("pipeline-definitions"), extras: [], groups: [], marker: true, source: Git { repository: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/pipeline-definitions.git", query: None, fragment: None }, reference: Tag("v0.3.2"), precise: None, subdirectory: None, url: VerbatimUrl { url: Url { scheme: "git+https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/pipeline-definitions.git@v0.3.2", query: None, fragment: None }, given: None } }, origin: None }, Requirement { name: PackageName("pybars3"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "0.9.7" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("unidecode"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "1.3.8" }]), index: None, conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("boto3"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "1.36.16" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("docling"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "2.21.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("kernel-research-lib"), extras: [], groups: [], marker: true, source: Git { repository: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/kernel-research-lib.git", query: None, fragment: None }, reference: Tag("v0.140.0"), precise: None, subdirectory: None, url: VerbatimUrl { url: Url { scheme: "git+https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/kernel-research-lib.git@v0.140.0", query: None, fragment: None }, given: None } }, origin: None }, Requirement { name: PackageName("pipeline-definitions"), extras: [], groups: [], marker: true, source: Git { repository: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/pipeline-definitions.git", query: None, fragment: None }, reference: Tag("v0.3.0"), precise: None, subdirectory: None, url: VerbatimUrl { url: Url { scheme: "git+https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/pipeline-definitions.git@v0.3.0", query: None, fragment: None }, given: None } }, origin: None }, Requirement { name: PackageName("pybars3"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "0.9.7" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("unidecode"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "1.3.8" }]), index: None, conflict: None }, origin: None }}
DEBUG Inserting Git reference into resolver: `RepositoryReference { url: RepositoryUrl(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/kernel-pipeline-python-client", query: None, fragment: None }), reference: BranchOrTag("v0.18.0") }` at `7c55ff3b3a4c4f0dd8fd496dcc4ee9753776d48f`
DEBUG Inserting Git reference into resolver: `RepositoryReference { url: RepositoryUrl(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/kernel-research-lib", query: None, fragment: None }), reference: BranchOrTag("v0.140.0") }` at `57ab4869d922a782a48ca4404fefc9129e047c0d`
DEBUG Inserting Git reference into resolver: `RepositoryReference { url: RepositoryUrl(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/kernlai/pipeline-definitions", query: None, fragment: None }), reference: Tag("v0.3.0") }` at `275938e3a393968e9477dcfa97d083d7c91c9277`
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/kernlai/kernel-research-lib/commits/v0.142.0
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/kernlai/pipeline-definitions/commits/v0.3.2
DEBUG No netrc file found
DEBUG GitHub API request failed for: https://api.github.com/repos/kernlai/pipeline-definitions/commits/v0.3.2 (404 Not Found)
DEBUG Fetching source distribution from Git: https://github.com/kernlai/pipeline-definitions.git
DEBUG Acquired lock for `https://github.com/kernlai/pipeline-definitions`
DEBUG GitHub API request failed for: https://api.github.com/repos/kernlai/kernel-research-lib/commits/v0.142.0 (404 Not Found)
DEBUG Fetching source distribution from Git: https://github.com/kernlai/kernel-research-lib.git
DEBUG Acquired lock for `https://github.com/kernlai/kernel-research-lib`
DEBUG Updating Git source `https://github.com/kernlai/pipeline-definitions.git`
DEBUG Updating Git source `https://github.com/kernlai/kernel-research-lib.git`
   Updating https://github.com/kernlai/pipeline-definitions.git (v0.3.2)
   Updating https://github.com/kernlai/kernel-research-lib.git (v0.142.0)
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/kernlai/pipeline-definitions/commits/v0.3.2
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/kernlai/kernel-research-lib/commits/v0.142.0
DEBUG Failed to check GitHub HTTP status client error (404 Not Found) for url (https://api.github.com/repos/kernlai/pipeline-definitions/commits/v0.3.2)
DEBUG Performing a Git fetch for: https://github.com/kernlai/pipeline-definitions.git
DEBUG Failed to check GitHub HTTP status client error (404 Not Found) for url (https://api.github.com/repos/kernlai/kernel-research-lib/commits/v0.142.0)
DEBUG Performing a Git fetch for: https://github.com/kernlai/kernel-research-lib.git
    Updated https://github.com/kernlai/pipeline-definitions.git (1aa60c13d9849c9f8f828b9084db0b10d1f4bea3)
DEBUG Released lock at `/Users/ruggerogargiulo/.cache/uv/git-v0/locks/7ff6e9059deddef4`
DEBUG Acquired lock for `/Users/ruggerogargiulo/.cache/uv/sdists-v7/git/3682453e23df8bfd/1aa60c13d9849c9f`
DEBUG Found static `pyproject.toml` for: pipeline-definitions @ git+https://github.com/kernlai/pipeline-definitions.git@v0.3.2
DEBUG No workspace root found, using project root
DEBUG Released lock at `/Users/ruggerogargiulo/.cache/uv/sdists-v7/git/3682453e23df8bfd/1aa60c13d9849c9f/.lock`
    Updated https://github.com/kernlai/kernel-research-lib.git (040e177de2ba50d7327d28c36ef83387ef66a5c4)
DEBUG Released lock at `/Users/ruggerogargiulo/.cache/uv/git-v0/locks/b5896dca0b574471`
DEBUG Acquired lock for `/Users/ruggerogargiulo/.cache/uv/sdists-v7/git/97ba592223cc42da/040e177de2ba50d7`
DEBUG Found static `pyproject.toml` for: kernel-research-lib @ git+https://github.com/kernlai/kernel-research-lib.git@v0.142.0
DEBUG No workspace root found, using project root
DEBUG Released lock at `/Users/ruggerogargiulo/.cache/uv/sdists-v7/git/97ba592223cc42da/040e177de2ba50d7/.lock`
DEBUG Fetching source distribution from Git: https://github.com/kernlai/kernel-pipeline-python-client.git
DEBUG Acquired lock for `https://github.com/kernlai/kernel-pipeline-python-client`
DEBUG Using existing Git source `https://github.com/kernlai/kernel-pipeline-python-client.git`
DEBUG Released lock at `/Users/ruggerogargiulo/.cache/uv/git-v0/locks/ff817853deb7d8c8`
DEBUG Acquired lock for `/Users/ruggerogargiulo/.cache/uv/sdists-v7/git/85ad866b08eedb22/7c55ff3b3a4c4f0d`
DEBUG No static `pyproject.toml` available for: kernel-pipeline-client @ git+https://github.com/kernlai/kernel-pipeline-python-client.git@v0.18.0#subdirectory=sync (PyprojectToml(FieldNotFound("project")))
DEBUG No static `PKG-INFO` available for: kernel-pipeline-client @ git+https://github.com/kernlai/kernel-pipeline-python-client.git@v0.18.0#subdirectory=sync (MissingPkgInfo)
DEBUG Found static `egg-info` for: kernel-pipeline-client @ git+https://github.com/kernlai/kernel-pipeline-python-client.git@v0.18.0#subdirectory=sync
DEBUG Released lock at `/Users/ruggerogargiulo/.cache/uv/sdists-v7/git/85ad866b08eedb22/7c55ff3b3a4c4f0d/.lock`
DEBUG Solving with installed Python version: 3.12.1
DEBUG Solving with target Python version: >=3.12
DEBUG Narrowed `requires-python` bound to: >=3.12, <3.12.4
DEBUG Narrowed `requires-python` bound to: >=3.12, <3.12.4
DEBUG Narrowed `requires-python` bound to: >=3.12, <3.12.4
DEBUG Narrowed `requires-python` bound to: >=3.12.4, <3.13
DEBUG Narrowed `requires-python` bound to: >=3.12.4, <3.13
DEBUG Narrowed `requires-python` bound to: >=3.12.4, <3.13
DEBUG Narrowed `requires-python` bound to: >=3.13
DEBUG Narrowed `requires-python` bound to: >=3.13
DEBUG Narrowed `requires-python` bound to: >=3.13
DEBUG Narrowed `requires-python` bound to: >=3.12, <3.12.4
DEBUG Narrowed `requires-python` bound to: >=3.12.4, <3.13
DEBUG Narrowed `requires-python` bound to: >=3.13
DEBUG Solving split (python_full_version >= '3.13' and platform_machine == 'x86_64' and sys_platform == 'darwin') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.13" }]), range: RequiresPythonRange(LowerBound(Included("3.13")), UpperBound(Unbounded)) })
DEBUG Adding direct dependency: kernel-research*
DEBUG Adding direct dependency: kernel-research:dev*
DEBUG Searching for a compatible version of kernel-research @ file:///Users/ruggerogargiulo/kernelai/research-env/repos/kernel-research (*)
DEBUG Adding direct dependency: boto3>=1.36.16
DEBUG Adding direct dependency: docling>=2.21.0
DEBUG Adding direct dependency: kernel-research-lib*
DEBUG Adding direct dependency: pipeline-definitions*
DEBUG Adding direct dependency: pybars3>=0.9.7
DEBUG Adding direct dependency: unidecode>=1.3.8
DEBUG Searching for a compatible version of kernel-research @ file:///Users/ruggerogargiulo/kernelai/research-env/repos/kernel-research (*)
DEBUG Adding direct dependency: kernel-research:dev==0.1.0
DEBUG Searching for a compatible version of kernel-research @ file:///Users/ruggerogargiulo/kernelai/research-env/repos/kernel-research (==0.1.0)
DEBUG Adding direct dependency: autoapi>=2.0.1
DEBUG Adding direct dependency: bandit>=1.8.0
DEBUG Adding direct dependency: ipykernel>=6.29.5
DEBUG Adding direct dependency: mypy>=1.13.0
DEBUG Adding direct dependency: pre-commit>=4.0.1
DEBUG Adding direct dependency: pytest>=8.3.4
DEBUG Adding direct dependency: pytest-asyncio>=0.25.0
DEBUG Adding direct dependency: ruff>=0.8.3
DEBUG Adding direct dependency: sphinx>=8.1.3
DEBUG Adding direct dependency: sphinx-autoapi>=3.4.0
DEBUG Adding direct dependency: sphinx-rtd-theme>=3.0.2
DEBUG Adding direct dependency: types-boto3>=1.0.2
DEBUG Searching for a compatible version of kernel-research-lib @ git+https://github.com/kernlai/kernel-research-lib.git@v0.142.0 (*)
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: aisuite>=0.1.6
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: boto3>=1.35.95
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: braintrust>=0.0.174
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: braintrust-api>=0.6.0, <0.6.0+
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: google-api-python-client>=2.0.0
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: google-auth>=2.0.0
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: google-auth-httplib2>=0.1.0
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: google-auth-oauthlib>=0.4.0
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: humanloop>=0.8.8
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: json-repair>=0.30.2
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: jsonpath-ng>=1.7.0
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: kernel-pipeline-client*
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: langchain>=0.3.9
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: mjml>=0.11.0
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: pandas>=2.2.1, <2.2.1+
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: pybars3>=0.9.7
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: pydantic>=2.10.4
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: pypdf2>=3.0.1
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: python-dotenv>=1.0.1
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: retry>=0.9.2
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: rich>=13.9.4
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: snowflake-sqlalchemy>=1.4.1
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: structlog>=24.4.0
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: tiktoken>=0.7.0
DEBUG Adding transitive dependency for kernel-research-lib==0.128.0: unidecode>=1.3.8
DEBUG Searching for a compatible version of pipeline-definitions @ git+https://github.com/kernlai/pipeline-definitions.git@v0.3.2 (*)
DEBUG Found fresh response for: https://pypi.org/simple/docling/
DEBUG Found fresh response for: https://pypi.org/simple/unidecode/
DEBUG Found fresh response for: https://pypi.org/simple/bandit/
DEBUG Found fresh response for: https://pypi.org/simple/autoapi/
DEBUG Found fresh response for: https://pypi.org/simple/pybars3/
DEBUG Acquired lock for `/Users/ruggerogargiulo/.cache/uv/sdists-v7/pypi/autoapi/2.0.1`
DEBUG Found fresh response for: https://pypi.org/simple/pytest/
DEBUG Found fresh response for: https://pypi.org/simple/boto3/
DEBUG Acquired lock for `/Users/ruggerogargiulo/.cache/uv/sdists-v7/pypi/pybars3/0.9.7`
DEBUG Found fresh response for: https://pypi.org/simple/pytest-asyncio/
DEBUG Found fresh response for: https://pypi.org/simple/sphinx-autoapi/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1c/c1/991a7a1404626558cc7db0cc34243e13e5e336eba053bf6979e9fd6006f7/bandit-1.8.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/types-boto3/
DEBUG Found fresh response for: https://pypi.org/simple/sphinx-rtd-theme/
DEBUG Found fresh response for: https://pypi.org/simple/ipykernel/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c6/48/90f6eb9b00bb29c2df02cd2ed50d76b7892f29b398693e4cca187cc4eade/docling-2.21.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/sphinx/
DEBUG Found fresh response for: https://pypi.org/simple/aisuite/
DEBUG Found fresh response for: https://pypi.org/simple/pre-commit/
DEBUG Found fresh response for: https://pypi.org/simple/braintrust-api/
DEBUG Found fresh response for: https://pypi.org/simple/google-auth-httplib2/
DEBUG Found fresh response for: https://pypi.org/simple/braintrust/
DEBUG Found fresh response for: https://pypi.org/simple/google-auth-oauthlib/
DEBUG Found fresh response for: https://pypi.org/simple/json-repair/
DEBUG Found fresh response for: https://pypi.org/simple/google-auth/
DEBUG Found fresh response for: https://pypi.org/simple/humanloop/
DEBUG Found fresh response for: https://pypi.org/simple/jsonpath-ng/
DEBUG Found fresh response for: https://pypi.org/simple/langchain/
DEBUG Found fresh response for: https://pypi.org/simple/pypdf2/
DEBUG Found fresh response for: https://pypi.org/simple/pydantic/
DEBUG Found fresh response for: https://pypi.org/simple/mjml/
DEBUG Found fresh response for: https://pypi.org/simple/snowflake-sqlalchemy/
DEBUG Found fresh response for: https://pypi.org/simple/retry/
DEBUG Found fresh response for: https://pypi.org/simple/python-dotenv/
DEBUG Found fresh response for: https://pypi.org/simple/rich/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/84/b7/6ec57841fb67c98f52fc8e4a2d96df60059637cba077edc569a302a8ffc7/Unidecode-1.3.8-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/structlog/
DEBUG Found fresh response for: https://pypi.org/simple/tiktoken/
DEBUG Found fresh response for: https://pypi.org/simple/pandas/
DEBUG Found fresh response for: https://pypi.org/simple/ruff/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/05/c0/27f55ac0c5b99861cca008cdc890316e864e99090db6005dcad4d0c601aa/autoapi-2.0.1.tar.gz
DEBUG Found fresh response for: https://pypi.org/simple/google-api-python-client/
DEBUG Found fresh response for: https://pypi.org/simple/mypy/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/11/92/76a1c94d3afee238333bc0a42b82935dd8f9cf8ce9e336ff87ee14d9e1cf/pytest-8.3.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ec/1a/2fb847db017f9f89ab8519d96e35fb3dacb6170a0643fddba3b366af0af1/pybars3-0.9.7.tar.gz
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/0d/782cb55b66256a6d05e40e5cc9c6be2045010ad206d10bf7c9d269f9aa70/boto3-1.36.18-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/67/17/3493c5624e48fd97156ebaec380dcaafee9506d7e2c46218ceebbb57d7de/pytest_asyncio-0.25.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9e/f7/6fb9c56af4533058649741c59b43f7d8cdd7d41b4867f6246f41a5a689fd/sphinx_autoapi-3.5.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0a/d7/56b84b72bb81c4e489822dbe5c7661e940321e74bea36d64f23e7aa268f3/types_boto3-1.36.18-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/85/77/46e3bac77b82b4df5bb5b61f2de98637724f246b4966cfc34bc5895d852a/sphinx_rtd_theme-3.0.2-py2.py3-none-any.whl.metadata
DEBUG No static `pyproject.toml` available for: autoapi==2.0.1 (MissingPyprojectToml)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/94/5c/368ae6c01c7628438358e6d337c19b05425727fbb221d2a3c4303c372f42/ipykernel-6.29.5-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/60/1ddff83a56d33aaf6f10ec8ce84b4c007d9368b21008876fceda7e7381ef/sphinx-8.1.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cf/4c/a276fa131b3ecbef19e68e8956015966dbdef9c2aac3c33f1a1867a84f67/aisuite-0.1.9-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/43/b3/df14c580d82b9627d173ceea305ba898dca135feb360b6d84019d0803d3b/pre_commit-4.1.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5c/42/b3cbdeda7c6843f6804587a06404e8ddde28e9a3b384fc9222c54dbb5215/braintrust_api-0.6.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/be/8a/fe34d2f3f9470a27b01c9e76226965863f153d5fbe276f83608562e49c04/google_auth_httplib2-0.2.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d5/f2/0152f9c2957f8f52c4b8636cffa1eac0352e1a9a125126c0c447e2888cf4/braintrust-0.0.184-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1a/8e/22a28dfbd218033e4eeaf3a0533b2b54852b6530da0c0fe934f0cc494b29/google_auth_oauthlib-1.2.1-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/93/20/c85b5c395ce0def844347d80054ec5157d70e884f36c06fc344a3c101a36/json_repair-0.36.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9d/47/603554949a37bca5b7f894d51896a9c534b9eab808e2520a748e081669d0/google_auth-2.38.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/73/22/529fc10b59e43aaa8a7ea8a5adb634867c7e0af2783b6f6079c50705eace/humanloop-0.8.24-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/5a/73ecb3d82f8615f32ccdadeb9356726d6cae3a4bbc840b437ceb95708063/jsonpath_ng-1.7.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/93/83/a4b41a1cf8b22fd708104d50edf98b720aa28647d3083d83b8348927a786/langchain-0.3.18-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8e/5e/c86a5643653825d3c913719e788e41386bee415c2b87b4f955432f2de6b2/pypdf2-3.0.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f4/3c/8cc1cc84deffa6e25d2d0c688ebb80635dfdbf1dbea3e30c541c8cf4d860/pydantic-2.10.6-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/62/ff/90cd7decb346b3a6e400565271e6eabb6dba3231c24bbd16f03492e4078a/mjml-0.11.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0e/a0/932d33434ca7ba01c80cd884337fb7b240758d90f3acfe7263e9e458f7a0/snowflake_sqlalchemy-1.7.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4b/0d/53aea75710af4528a25ed6837d71d117602b01946b307a3912cb3cfcbcba/retry-0.9.2-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6a/3e/b68c118422ec867fa7ab88444e1274aa40681c606d59ac27de5a5588f082/python_dotenv-1.0.1-py3-none-any.whl.metadata
DEBUG No static `pyproject.toml` available for: pybars3==0.9.7 (MissingPyprojectToml)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c1/14/e9aed6c820fa166e8a19a19e663e98bd5538004d68a70c5752458ca3656e/structlog-25.1.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c1/22/34b2e136a6f4af186b6640cbfd6f93400783c9ef6cd550d9eab80628d9de/tiktoken-0.8.0-cp312-cp312-macosx_10_13_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ed/b9/660353ce2b1bd5b6e0f5c992836d91909c0da1ccb59c16565ad0a37e839d/pandas-2.2.1-cp312-cp312-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/76/e3/3d2c022e687e18cf5d93d6bfa2722d46afc64eaa438c7fbbdd603b3597be/ruff-0.9.6-py3-none-linux_armv6l.whl.metadata
DEBUG No static `PKG-INFO` available for: autoapi==2.0.1 (PkgInfo(UnsupportedMetadataVersion("1.1")))
DEBUG No static `egg-info` available for: autoapi==2.0.1 (MissingEggInfo)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/49/35/41623ac3b581781169eed7f5fcd24bc114c774dc491fab5c05d8eb81af36/google_api_python_client-2.160.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/98/3a/03c74331c5eb8bd025734e04c9840532226775c47a2c39b56a0c8d4f128d/mypy-1.15.0-cp312-cp312-macosx_10_13_x86_64.whl.metadata
DEBUG No static `PKG-INFO` available for: pybars3==0.9.7 (PkgInfo(UnsupportedMetadataVersion("1.1")))
DEBUG Using cached metadata for: autoapi==2.0.1
DEBUG Released lock at `/Users/ruggerogargiulo/.cache/uv/sdists-v7/pypi/autoapi/2.0.1/.lock`
DEBUG Found static `egg-info` for: pybars3==0.9.7
DEBUG Released lock at `/Users/ruggerogargiulo/.cache/uv/sdists-v7/pypi/pybars3/0.9.7/.lock`
error: Requirements contain conflicting URLs for package `kernel-research-lib` in split `python_full_version >= '3.13' and platform_machine == 'x86_64' and sys_platform == 'darwin'`:
- git+https://github.com/kernlai/kernel-research-lib.git@v0.142.0
- git+https://github.com/kernlai/kernel-research-lib.git@v0.142.0

```

## Related Issues
No related issues found.

### Platform

macOS 14.6.1

### Version

uv 0.5.30 (Homebrew 2025-02-10)

### Python version

Python 3.12.1

---

_Label `bug` added by @keidikapllani on 2025-02-12 16:15_

---

_Comment by @charliermarsh on 2025-02-12 17:16_

Can you please create a public Git repository that we can use to reproduce the issue?

---

_Label `bug` removed by @charliermarsh on 2025-02-12 17:16_

---

_Label `needs-mre` added by @charliermarsh on 2025-02-12 17:16_

---

_Comment by @ships on 2025-02-27 17:11_

I am facing this issue as well, and have created a minimal reproduction. In my case, it is not about public vs private git repos, but rather the intermediate repository uses poetry whereas the consumer uses uv.

Like so: See my repro repos linked:
[Root - consumes A and B - (uses uv)](https://github.com/ships/uv_git_deps_bug_root)
[B - consumes A - (uses poetry)](https://github.com/ships/uv_git_deps_bug_b)
[A - (uses uv)](https://github.com/ships/uv_git_deps_bug_a)

To demonstrate, `git clone https://github.com/ships/uv_git_deps_bug_root root && cd root && uv lock --upgrade`.

In the production case where I face this issue, this is also the setup where we consume upstream dependencies that do not consistently use `uv` for their packaging. I think there is a way to resolve this where upstream declares dependencies in a non-poetryish way (`project.dependencies`), but it's still surprising that `uv` does correctly detect the URLs but presents this bug as described. Besides, we do not always control upstream.

---

_Comment by @kubotty on 2025-03-17 11:15_

I faced the same issue. Strangely, it seems to be resolved well by using `rev`.

---

_Comment by @PepijnB on 2025-05-24 18:47_

I have the same issue. I've created a minimum working example on [GitHub](https://github.com/PepijnB/uv-mwe1). The dependency tree is as following:
```shell
uv-mwe1
├── uv-mwe-mypackage1 @ https://github.com/PepijnB/uv-mwe-mypackage1
│   └── uv-mwe-mypackage2 @ https://github.com/PepijnB/uv-mwe-mypackage1
└── uv-mwe-mypackage2 @ https://github.com/PepijnB/uv-mwe-mypackage1 --tag v1.04
```
The dependency of `uv-mwe-mypackage1` on `uv-mwe-mypackage2` is without any tag. The dependency of `uv-mwe1` on `uv-mwe-mypackage2` is with a specific tag.

**Expectation**
I expect that if I don't specify a tag/branch/sha that _any_ will suffice. Since the direct dependency specifies 'v1.0.4` I expect this to take precedence and that this particular tag will be used.

**Reproduce**
```shell
git clone https://github.com/PepijnB/uv-mwe1
cd uv-mwe1
uv venv --python 3.13
source ./.venv/bin/activate

uv sync
```

**Error message**
```shell
$ uv sync
error: Requirements contain conflicting URLs for package `uv-mwe-mypackage2` in all marker environments:
- git+https://github.com/PepijnB/uv_mwe_mypackage2.git
- git+https://github.com/PepijnB/uv_mwe_mypackage2.git@v0.1.4
```

**Version**
I've tested with `uv 0.6.14` and `uv 0.7.8`

---

_Comment by @PepijnB on 2025-05-24 19:04_

I found another one, where the dependency tree is as following:

```shell
uv-mwe1
├── uv-mwe-mypackage1 @ https://github.com/PepijnB/uv-mwe-mypackage1
│   └── uv-mwe-mypackage2 @ https://github.com/PepijnB/uv-mwe-mypackage1
└── uv-mwe-mypackage2 @ git+ssh://github.com/PepijnB/uv-mwe-mypackage1
```

**Expectation**
I expect the ssh-URL and https URL to GitHub are threated as the same URL. 

**Reproduce**
```shell
git clone https://github.com/PepijnB/uv-mwe1
cd uv-mwe1
uv venv --python 3.13
source ./.venv/bin/activate

# The current pyproject.toml includes the failure as discussed [here](https://github.com/astral-sh/uv/issues/11452#issuecomment-2906973412). This has to be removed first.
uv add https://github.com/PepijnB/uv_mwe_mypackage2.git

# This should end up in a clean state. 'uv tree' should show this
# uv-mwe1
# ├── uv-mwe-mypackage1 v0.1.0
# │   └── uv-mwe-mypackage2 v0.1.0
# └── uv-mwe-mypackage2 v0.1.0
uv tree

# Now change the URL
uv add git+ssh://git@github.com/PepijnB/uv_mwe_mypackage2.git
```

**Error message**
```shell
$ uv add git+ssh://git@github.com/PepijnB/uv_mwe_mypackage2.git
error: Requirements contain conflicting URLs for package `uv-mwe-mypackage2` in all marker environments:
- git+https://github.com/PepijnB/uv_mwe_mypackage2.git
- git+ssh://git@github.com/PepijnB/uv_mwe_mypackage2.git
```

**Version**
I've tested with `uv 0.6.14` and `uv 0.7.8`

---

_Comment by @konstin on 2025-05-26 13:24_

@PepijnB For the first example, a Git dependency without further specifiers is considered as a dependency on main/master (the default branch of the repository), so it clashes with the explicit `v1.04` tag.

For the second example, it's true that they point to the same URL, but uv can't tell if you want it to use https or ssh for the package, so I wouldn't know how to resolve the conflict.

---

_Comment by @PepijnB on 2025-05-26 13:55_

@konstin Thanks for the clarification.

Regarding the first:
I understand what you mean, however imho the question is: is this desired behavior? At this moment, the top package can only use the same branch as is configured in its dependencies. In that sense its behavior is different from the bevahior with specifying versions. For example
```shell
uv add wrapt
```
means: 
> install **any** version of the wrapt package that satifies my (and all other) needs, and use the latest as default.
Why not do the same when specifying branches or tags?

Regarding the second:
for git it is possible to configure to replacee the URL by another. Example:
```shell
git config url."ssh://${GITHUB_PAT}@github.com/".insteadOf "https://github.com/"
```
Possibly this could be added to `pyproject.toml` (or `pip.conf`)

What do you think?

---

_Comment by @konstin on 2025-05-26 14:05_

> Regarding the first: I understand what you mean, however imho the question is: is this desired behavior? At this moment, the top package can only use the same branch as is configured in its dependencies. In that sense its behavior is different from the bevahior with specifying versions. For example
> 
> uv add wrapt
> 
> means:
> 
> > install **any** version of the wrapt package that satifies my (and all other) needs, and use the latest as default.
> Why not do the same when specifying branches or tags?

We inherit those semantics from Git: For registry dependencies, we have versions, which are ordered, and specifiers pointing to a subset of those. Not specifying a version is interpreter as an arbitrary specifier (think `>=0`). For Git, there's individual commits without order, and (changeable) pointers to those commits, with e.g. `v1.04` resolving to 55d13509633cd2ba2238be5f4857e1495afecbde. Not specifying a reference is interpreted as the top of the default branch, which atm resolves to a0257ea439d74264b8d282a139a93b04a0d6a5c5.

> Regarding the second: for git it is possible to configure to replacee the URL by another. Example:
> 
> git config url."ssh://${GITHUB_PAT}@github.com/".insteadOf "https://github.com/"
> 
> Possibly this could be added to `pyproject.toml` (or `pip.conf`)
> 
> What do you think?

That would be possible, though we probably want to configure this in the gitconfig and get this information from Git over defining this in our own config to stay aligned with the Git CLI as close as possible.

---

_Comment by @PepijnB on 2025-05-26 14:49_

@konstin 
> Not specifying a reference is interpreted as the top of the default branch, which ....

I expect, however, it is a choice to interpret a missing reference as the top of the default branch. This choice has the consequence that the default cannot be overruled (or can it?). I propose to use a different default:
> Prefer specific over generic: if nothing is specified, use the tip of the default-branch (current behavior), otherwise use the specified version.

In my (limited) view, such change is backward compatible, but will provide an option to have control over the used package version from the top-level (in specific conditions).

---

_Comment by @konstin on 2025-05-28 13:04_

I should clarify, this default comes from git itself, and we're trying to stick to the git semantics in uv.

---
