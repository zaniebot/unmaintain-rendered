---
number: 13358
title: uv add tries checks for package in both public and private package registry, then fails with private registry error despite the package being available in public registry
type: issue
state: open
author: Ark-kun
labels:
  - needs-mre
assignees: []
created_at: 2025-05-09T00:05:59Z
updated_at: 2025-10-24T22:35:44Z
url: https://github.com/astral-sh/uv/issues/13358
synced_at: 2026-01-10T01:25:32Z
---

# uv add tries checks for package in both public and private package registry, then fails with private registry error despite the package being available in public registry

---

_Issue opened by @Ark-kun on 2025-05-09 00:05_

### Summary

TL;DR; Despite `uv.lock` having only PyPI URLs, `uv` commands try both PyPI URLs (which succeed) and internal repository URLs (which fail) and then `uv` fails despite successful responses from PyPI.

Note that even though the included logs feature the `uv add` command, the failure is with packages that were already in `pyproject.toml` and `uv.lock` and the failure also happened with `uv run`.


I'm using uv for a very simple package.
Requirements in pyproject.toml:
```
dependencies = [
    "cloud-pipelines>=0.23.2.4",
    "typer>=0.15.3",
]
```
The `uv.lock` file uses PyPI URLs everywhere:
```
[[package]]
name = "typer"
version = "0.15.3"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "click" },
    { name = "rich" },
    { name = "shellingham" },
    { name = "typing-extensions" },
]
sdist = { url = "https://files.pythonhosted.org/packages/98/1a/5f36851f439884bcfe8539f6a20ff7516e7b60f319bbaf69a90dc35cc2eb/typer-0.15.3.tar.gz", hash = "sha256:818873625d0569653438316567861899f7e9972f2e6e0c16dab608345ced713c", size = 101641, upload-time = "2025-04-28T21:40:59.204Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/48/20/9d953de6f4367163d23ec823200eb3ecb0050a2609691e512c8b95827a9b/typer-0.15.3-py3-none-any.whl", hash = "sha256:c86a65ad77ca531f03de08d1b9cb67cd09ad02ddddf4b34745b5008f43b239bd", size = 45253, upload-time = "2025-04-28T21:40:56.269Z" },
]
```

The problematic lines:

```
          0.039154s   3ms DEBUG uv_client::cached_client Found stale response for: https://pkgs.shopify.io/basic/data/python/simple/typer/
          0.039156s   3ms DEBUG uv_client::cached_client Sending revalidation request for: https://pkgs.shopify.io/basic/data/python/simple/typer/
         uv_client::cached_client::revalidation_request url="https://pkgs.shopify.io/basic/data/python/simple/typer/"
          0.039170s   3ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.python.org/simple/typer/


    0.208392s DEBUG uv::commands::project::add Reverting changes to `pyproject.toml`
    0.208709s DEBUG uv::commands::project::add Reverting changes to `uv.lock`
error: Failed to fetch: `https://pkgs.shopify.io/basic/data/python/simple/typer/`
  Caused by: HTTP status client error (400 Bad Request) for url (https://pkgs.shopify.io/basic/data/python/simple/typer/)
```

**If there was a fresh successful response from `pypi.python.org`, why fail on bad response from `pkgs.shopify.io`?** (And if for some reason uv only wants response from `pkgs.shopify.io`, then why check `pypi.python.org` at all?)


```
oasis-cli % uv add GitPython -vvv
    0.000953s DEBUG uv uv 0.6.0 (Homebrew 2025-02-14)
    0.003790s DEBUG uv_workspace::workspace Found project root: `/Users/ark/src/github.com/Cloud-Pipelines/oasis-cli`
    0.003872s DEBUG uv_workspace::workspace No workspace root found, using project root
    0.005014s DEBUG uv_fs Acquired lock for `/Users/ark/src/github.com/Cloud-Pipelines/oasis-cli`
    0.006310s DEBUG uv_python::version_files Reading Python requests from version file at `/Users/ark/src/github.com/Cloud-Pipelines/oasis-cli/.python-version`
    0.006734s DEBUG uv::commands::project Using Python request `3.11` from version file at `.python-version`
    0.007457s DEBUG uv_python::environment Checking for Python environment at `.venv`
    0.009001s DEBUG uv::commands::project The virtual environment's Python version satisfies `3.11`
    0.009048s DEBUG uv_fs Released lock at `/var/folders/_9/dl_tfx9169q_wbnqf639bn6r0000gn/T/uv-8e9243df8e848a2d.lock`
 uv_requirements::specification::from_source source=GitPython
 uv_client::linehaul::linehaul 
    0.022036s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries 
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=shopify-oasis @ file:///Users/ark/src/github.com/Cloud-Pipelines/oasis-cli
    0.027120s   1ms DEBUG uv_distribution::source Found static `pyproject.toml` for: shopify-oasis @ file:///Users/ark/src/github.com/Cloud-Pipelines/oasis-cli
    0.028258s   2ms DEBUG uv_workspace::workspace No workspace root found, using project root
    0.028956s DEBUG uv::commands::project::lock Ignoring existing lockfile due to mismatched requirements for: `shopify-oasis==0.1.0`
   Requested: {Requirement { name: PackageName("cloud-pipelines"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "0.23.2.4" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("gitpython"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("typer"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "0.15.3" }]), index: None, conflict: None }, origin: None }}
   Existing: {Requirement { name: PackageName("cloud-pipelines"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "0.23.2.4" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("typer"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "0.15.3" }]), index: None, conflict: None }, origin: None }}
 uv_resolver::resolver::solve 
    0.032335s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.11.11
    0.032339s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.11
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.033427s   1ms DEBUG uv_resolver::resolver Adding direct dependency: shopify-oasis*
    0.033562s   1ms DEBUG uv_resolver::resolver Searching for a compatible version of shopify-oasis @ file:///Users/ark/src/github.com/Cloud-Pipelines/oasis-cli (*)
   uv_resolver::resolver::get_dependencies_forking package=shopify-oasis, version=0.1.0
     uv_resolver::resolver::get_dependencies package=shopify-oasis, version=0.1.0
    0.033706s   1ms DEBUG uv_resolver::resolver Adding direct dependency: cloud-pipelines>=0.23.2.4
 uv_resolver::resolver::process_request request=Versions cloud-pipelines
   uv_client::registry_client::simple_api package=cloud-pipelines
    0.033748s   1ms DEBUG uv_resolver::resolver Adding direct dependency: gitpython*
    0.033751s   1ms DEBUG uv_resolver::resolver Adding direct dependency: typer>=0.15.3
     uv_client::cached_client::get_cacheable_with_retry 
       uv_client::cached_client::get_cacheable 
         uv_client::cached_client::read_and_parse_cache file=/Users/ark/.cache/uv/simple-v15/index/ffcc87a41e1c03a1/cloud-pipelines.rkyv
     uv_client::cached_client::get_cacheable_with_retry 
       uv_client::cached_client::get_cacheable 
         uv_client::cached_client::read_and_parse_cache file=/Users/ark/.cache/uv/simple-v15/index/dcd4cb247a731f5c/cloud-pipelines.rkyv
 uv_client::cached_client::from_path_sync path="/Users/ark/.cache/uv/simple-v15/index/ffcc87a41e1c03a1/cloud-pipelines.rkyv"
 uv_resolver::resolver::process_request request=Versions gitpython
   uv_client::registry_client::simple_api package=gitpython
 uv_client::cached_client::from_path_sync path="/Users/ark/.cache/uv/simple-v15/index/dcd4cb247a731f5c/cloud-pipelines.rkyv"
     uv_client::cached_client::get_cacheable_with_retry 
       uv_client::cached_client::get_cacheable 
         uv_client::cached_client::read_and_parse_cache file=/Users/ark/.cache/uv/simple-v15/index/ffcc87a41e1c03a1/gitpython.rkyv
     uv_client::cached_client::get_cacheable_with_retry 
       uv_client::cached_client::get_cacheable 
         uv_client::cached_client::read_and_parse_cache file=/Users/ark/.cache/uv/simple-v15/index/dcd4cb247a731f5c/gitpython.rkyv
 uv_client::cached_client::from_path_sync path="/Users/ark/.cache/uv/simple-v15/index/ffcc87a41e1c03a1/gitpython.rkyv"
 uv_resolver::resolver::process_request request=Versions typer
   uv_client::registry_client::simple_api package=typer
     uv_client::cached_client::get_cacheable_with_retry 
       uv_client::cached_client::get_cacheable 
         uv_client::cached_client::read_and_parse_cache file=/Users/ark/.cache/uv/simple-v15/index/ffcc87a41e1c03a1/typer.rkyv
     uv_client::cached_client::get_cacheable_with_retry 
       uv_client::cached_client::get_cacheable 
         uv_client::cached_client::read_and_parse_cache file=/Users/ark/.cache/uv/simple-v15/index/dcd4cb247a731f5c/typer.rkyv
 uv_client::cached_client::from_path_sync path="/Users/ark/.cache/uv/simple-v15/index/ffcc87a41e1c03a1/typer.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/ark/.cache/uv/simple-v15/index/dcd4cb247a731f5c/gitpython.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/ark/.cache/uv/simple-v15/index/dcd4cb247a731f5c/typer.rkyv"
          0.036608s   1ms DEBUG uv_client::cached_client Found stale response for: https://pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/
          0.036623s   1ms DEBUG uv_client::cached_client Sending revalidation request for: https://pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/
         uv_client::cached_client::revalidation_request url="https://pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/"
          0.038432s   3ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.python.org/simple/cloud-pipelines/
          0.039026s   3ms DEBUG uv_client::cached_client Found stale response for: https://pkgs.shopify.io/basic/data/python/simple/gitpython/
          0.039040s   3ms DEBUG uv_client::cached_client Sending revalidation request for: https://pkgs.shopify.io/basic/data/python/simple/gitpython/
         uv_client::cached_client::revalidation_request url="https://pkgs.shopify.io/basic/data/python/simple/gitpython/"
          0.039073s   3ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.python.org/simple/gitpython/
 uv_resolver::resolver::process_request request=Prefetch typer >=0.15.3
 uv_resolver::resolver::process_request request=Prefetch gitpython *
 uv_resolver::resolver::process_request request=Prefetch cloud-pipelines >=0.23.2.4
          0.039154s   3ms DEBUG uv_client::cached_client Found stale response for: https://pkgs.shopify.io/basic/data/python/simple/typer/
          0.039156s   3ms DEBUG uv_client::cached_client Sending revalidation request for: https://pkgs.shopify.io/basic/data/python/simple/typer/
         uv_client::cached_client::revalidation_request url="https://pkgs.shopify.io/basic/data/python/simple/typer/"
          0.039170s   3ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.python.org/simple/typer/
    0.208392s DEBUG uv::commands::project::add Reverting changes to `pyproject.toml`
    0.208709s DEBUG uv::commands::project::add Reverting changes to `uv.lock`
error: Failed to fetch: `https://pkgs.shopify.io/basic/data/python/simple/typer/`
  Caused by: HTTP status client error (400 Bad Request) for url (https://pkgs.shopify.io/basic/data/python/simple/typer/)
```

0.7.2:

```
% uv add GitPython -vv
DEBUG uv 0.7.2 (Homebrew 2025-04-30)
DEBUG Found project root: `/Users/ark/src/github.com/Cloud-Pipelines/oasis-cli`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/Users/ark/src/github.com/Cloud-Pipelines/oasis-cli` at `/var/folders/_9/dl_tfx9169q_wbnqf639bn6r0000gn/T/uv-8e9243df8e848a2d.lock`
DEBUG Acquired lock for `/Users/ark/src/github.com/Cloud-Pipelines/oasis-cli`
DEBUG Reading Python requests from version file at `/Users/ark/src/github.com/Cloud-Pipelines/oasis-cli/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
TRACE Cached interpreter info for Python 3.11.11, skipping probing: .venv/bin/python3
DEBUG The virtual environment's Python version satisfies the request: `Python 3.11`
TRACE The virtual environment's Python version meets the Python requirement: `>=3.11`
DEBUG Released lock at `/var/folders/_9/dl_tfx9169q_wbnqf639bn6r0000gn/T/uv-8e9243df8e848a2d.lock`
TRACE Caching credentials for https://token:***@pkgs.shopify.io/basic/data/python/simple/
TRACE Caching credentials for https://token:***@pkgs.shopify.io/basic/data/python
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: shopify-oasis @ file:///Users/ark/src/github.com/Cloud-Pipelines/oasis-cli
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched requirements for: `shopify-oasis==0.1.0`
  Requested: {Requirement { name: PackageName("cloud-pipelines"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "0.23.2.4" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("gitpython"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("typer"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "0.15.3" }]), index: None, conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("cloud-pipelines"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "0.23.2.4" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("typer"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "0.15.3" }]), index: None, conflict: None }, origin: None }}
TRACE Performing lookahead for shopify-oasis @ file:///Users/ark/src/github.com/Cloud-Pipelines/oasis-cli
DEBUG Solving with installed Python version: 3.11.11
DEBUG Solving with target Python version: >=3.11
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: shopify-oasis*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: shopify-oasis. remaining choices: 
DEBUG Searching for a compatible version of shopify-oasis @ file:///Users/ark/src/github.com/Cloud-Pipelines/oasis-cli (*)
DEBUG Adding direct dependency: cloud-pipelines>=0.23.2.4
DEBUG Adding direct dependency: gitpython*
DEBUG Adding direct dependency: typer>=0.15.3
TRACE Assigned packages: root==0a0.dev0, shopify-oasis==0.1.0
TRACE Chose package for decision: cloud-pipelines. remaining choices: typer, gitpython
TRACE Fetching metadata for cloud-pipelines from https://token:***@pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/
TRACE Fetching metadata for cloud-pipelines from https://pypi.python.org/simple/cloud-pipelines/
TRACE Fetching metadata for gitpython from https://token:***@pkgs.shopify.io/basic/data/python/simple/gitpython/
TRACE Fetching metadata for gitpython from https://pypi.python.org/simple/gitpython/
TRACE Fetching metadata for typer from https://token:***@pkgs.shopify.io/basic/data/python/simple/typer/
TRACE Fetching metadata for typer from https://pypi.python.org/simple/typer/
TRACE Cached request https://pypi.python.org/simple/cloud-pipelines/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.python.org/simple/cloud-pipelines/
TRACE Cached request https://pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
TRACE Cached request https://pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/ has a cached response that does not allow staleness
TRACE Request https://pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/ does not have a fresh cache because its age is 686887 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/
DEBUG Sending revalidation request for: https://pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/
TRACE Handling request for https://token:****@pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/ with authentication policy auto
TRACE Request for https://token:****@pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/ already contains username and password
TRACE Cached request https://pypi.python.org/simple/typer/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.python.org/simple/typer/
TRACE Cached request https://pkgs.shopify.io/basic/data/python/simple/typer/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
TRACE Cached request https://pkgs.shopify.io/basic/data/python/simple/typer/ has a cached response that does not allow staleness
TRACE Request https://pkgs.shopify.io/basic/data/python/simple/typer/ does not have a fresh cache because its age is 763056 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pkgs.shopify.io/basic/data/python/simple/typer/
DEBUG Sending revalidation request for: https://pkgs.shopify.io/basic/data/python/simple/typer/
TRACE Handling request for https://token:****@pkgs.shopify.io/basic/data/python/simple/typer/ with authentication policy auto
TRACE Request for https://token:****@pkgs.shopify.io/basic/data/python/simple/typer/ already contains username and password
TRACE Cached request https://pkgs.shopify.io/basic/data/python/simple/gitpython/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 30s
TRACE Cached request https://pkgs.shopify.io/basic/data/python/simple/gitpython/ has a cached response that does not permit staleness because the response has a 'must-revalidate' cache-control directive set
TRACE Request https://pkgs.shopify.io/basic/data/python/simple/gitpython/ does not have a fresh cache because its age is 3479873 seconds, it is greater than the freshness lifetime of 30 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pkgs.shopify.io/basic/data/python/simple/gitpython/
DEBUG Sending revalidation request for: https://pkgs.shopify.io/basic/data/python/simple/gitpython/
TRACE Handling request for https://token:****@pkgs.shopify.io/basic/data/python/simple/gitpython/ with authentication policy auto
TRACE Request for https://token:****@pkgs.shopify.io/basic/data/python/simple/gitpython/ already contains username and password
TRACE Cached request https://pypi.python.org/simple/gitpython/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.python.org/simple/gitpython/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.shopify.io")), port: None, path: "/basic/data/python/simple/gitpython/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(400), url: "https://pkgs.shopify.io/basic/data/python/simple/gitpython/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Reverting changes to `pyproject.toml`
DEBUG Reverting changes to `uv.lock`
error: Failed to fetch: `https://pkgs.shopify.io/basic/data/python/simple/gitpython/`
  Caused by: HTTP status client error (400 Bad Request) for url (https://pkgs.shopify.io/basic/data/python/simple/gitpython/)
```

The problem no longer reproduces (I guess, the internal repository stopped returning errors). Anyways, I think the problematic behavior is worth looking at.

### Platform

Mac OS 15 arm64

### Version

0.6.0 and 0.7.2

### Python version

Python 3.11

---

_Label `bug` added by @Ark-kun on 2025-05-09 00:06_

---

_Comment by @konstin on 2025-05-09 07:04_

How does the index configuration in your `pyproject.toml`, `uv.toml` or in env vars look like?

---

_Label `bug` removed by @konstin on 2025-05-09 07:04_

---

_Label `needs-mre` added by @konstin on 2025-05-09 07:04_

---

_Assigned to @jtfmumm by @konstin on 2025-05-09 07:04_

---

_Comment by @Ark-kun on 2025-05-28 21:10_

>How does the index configuration in your pyproject.toml, uv.toml or in env vars look like?

No `uv.toml`.
Nothing special in `pyproject.toml`.
```
[project]
name = "shopify-oasis"
version = "0.1.0"
description = "CLI for Cloud Pipelines"
readme = "README.md"
authors = [
    { name = "Alexey Volkov", email = "alexey.volkov@ark-kun.com" }
]
requires-python = ">=3.10"
dependencies = [
    "click<8.2.0",
    "cloud-pipelines>=0.23.2.4",
    "gitpython>=3.1.44",
    "podman>=5.4.0.1",
    "pyyaml>=6.0.2",
    "tomli>=2.2.1",
    "typer>=0.15.3",
]

[project.scripts]
oasis = "cloud_pipelines_oasis_cli.main:app"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/cloud_pipelines_oasis_cli"]

[dependency-groups]
dev = [
    "pytest>=8.3.5",
]
```

We'll try to check env when this reproduces again. Which env variables can affect the package repository lookup?

---

_Comment by @Ark-kun on 2025-05-28 21:12_

Anyways, what I find problematic here is that after trying both URLs (one succeeds, another fails), `uv` decides to fail. My expectation would be either:
* Try one URL. If it fails, then fail.
or 
* Try multiple URLs. If one succeeds, then succeed.
but not
* Try multiple URLs. If one fails, then fail.


---

_Comment by @charliermarsh on 2025-05-28 21:17_

We should definitely fail if we fail to query an index. It would be insecure for us to silently ignore an authentication failure and instead serve you data from another index. You can configure to ignore such failures and proceed if you want to opt-in to a less secure strategy: https://docs.astral.sh/uv/configuration/indexes/#ignoring-error-codes-when-searching-across-indexes

---

_Comment by @WilliamStam on 2025-07-31 08:56_

i dont know if this is related., and started happening more with the 7 branch of uv it feels like altho i cant be more specific than that

we have a gitea package repo setup for python packages.

```
[project]
name = "leproject"
version = "0.2.16"
description = "myawesomeproject"
authors = [
    
]
requires-python = ">=3.12"
readme = "README.md"
license = {text = "MIT"}
dependencies = [
    # Custom indexer package
    "Custodian-Database==1.0.*",
    "App-Utilities==1.0.*",
   
    # pypi packages
    "aiofiles==24.1.*",
    "asyncmy==0.2.*",
    "coloredlogs==15.0.*",
    "Wand==0.6.*",
]

[tool.uv]
package = false

[[tool.uv.index]]
name = "database"
url = "https://pypi@example.com:password@git.example.com/api/packages/Database/pypi/simple"
[[tool.uv.index]]
name = "utilities"
url = "https://pypi@example.com:password@git.example.com/api/packages/Utilities/pypi/simple"
[[tool.uv.index]]
name = "reports"
url = "https://pypi@example.com:password@git.example.com/api/packages/Reports/pypi/simple"


```

it now fails a lot of the time

```
(venv) W:\Projects\API>uv sync --upgrade
⠼ app-utilities==1.0.39                                                                                                                                                                                                                                             error: Request failed after 1 retries
  Caused by: Failed to fetch: `https://git.example.com/api/packages/Reports/pypi/simple/wand/`
  Caused by: HTTP status client error (404 Not Found) for url (https://git.example.com/api/packages/Reports/pypi/simple/wand/)

(venv) W:\Projects\API>
```

windows 11, python 3.13, uv 0.8.4

is my understanding that if it finds a 404 then it will try the next indexer all the way to pypi correct?

---

_Comment by @konstin on 2025-07-31 19:51_

@WilliamStam We're aware of this problem, we're fixing it in https://github.com/astral-sh/uv/pull/14996

---
