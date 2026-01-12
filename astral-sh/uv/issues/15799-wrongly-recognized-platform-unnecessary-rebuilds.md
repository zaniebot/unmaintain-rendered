```yaml
number: 15799
title: Wrongly recognized platform, unnecessary rebuilds
type: issue
state: closed
author: fleetingbytes
labels:
  - bug
assignees: []
created_at: 2025-09-12T01:41:38Z
updated_at: 2025-10-24T22:23:38Z
url: https://github.com/astral-sh/uv/issues/15799
synced_at: 2026-01-12T16:02:17Z
```

# Wrongly recognized platform, unnecessary rebuilds

---

_@fleetingbytes_

### Summary

In a Python project which depends on [cryptography][cryptography] and [cffi][cffi] I am facing unnecessary rebuilds with every `uv run` or `uv sync` because there appears to be a platform mismatch. My platform is `FreeBSD 14.2-RELEASE amd64` (that's the output of `uname -srm`), yet in the log of `uv sync --verbose` I read:

```
The distribution is compatible with \x1b[36mFreeBSD\x1b[39m (`\x1b[36mfreebsd_14_2_release_amd64\x1b[39m`), but you're on \x1b[36mFreeBSD\x1b[39m (`\x1b[36mfreebsd_14_2_RELEASE_x86_64\x1b[39m`)
```

Here is a minimal reproducible example. cffi suffices as a dependency. We omit cryptography as it produces the same issue but a longer build time and longer log output:
```sh
uv init rebuild
cd rebuild
uv add cffi
RUST_LOG=trace uv sync --verbose
```
and here is the beginning of that log:
```
DEBUG uv 0.8.17 (21a92c163 2025-09-11)
DEBUG Found project root: `/home/fleetingbytes/src/rebuild`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/home/fleetingbytes/src/rebuild` at `/tmp/uv-6ba0532bfdeb457a.lock`
DEBUG Acquired lock for `/home/fleetingbytes/src/rebuild`
DEBUG Reading Python requests from version file at `/home/fleetingbytes/src/rebuild/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
TRACE Found cached interpreter info for Python 3.11.13, skipping query of: .venv/bin/python3
DEBUG The project environment's Python version satisfies the request: `Python 3.11`
TRACE The project environment's Python version meets the Python requirement: `>=3.11`
TRACE The virtual environment's Python interpreter meets the Python preference: `prefer managed`
DEBUG Released lock at `/tmp/uv-6ba0532bfdeb457a.lock`
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: rebuild @ file:///home/fleetingbytes/src/rebuild
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 3 packages in 0.98ms
DEBUG Using request timeout of 30s
TRACE Comparing installed with source: InstalledDist { kind: Registry(InstalledRegistryDist { name: PackageName("cffi"), version: "2.0.0", path: "/home/fleetingbytes/src/rebuild/.venv/lib/python3.11/site-packages/cffi-2.0.0.dist-info", cache_info: None, build_info: Some(BuildInfo { config_settings: ConfigSettings({}), extra_build_requires: [], extra_build_variables: {} }) }), metadata_cache: OnceLock(<uninit>), tags_cache: OnceLock(<uninit>) } Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.0.0" }]), index: Some(IndexMetadata { url: Pypi(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }
DEBUG Platform tags mismatch for cffi==2.0.0: The distribution is compatible with \x1b[36mFreeBSD\x1b[39m (`\x1b[36mfreebsd_14_2_release_amd64\x1b[39m`), but you're on \x1b[36mFreeBSD\x1b[39m (`\x1b[36mfreebsd_14_2_RELEASE_x86_64\x1b[39m`)
DEBUG Requirement installed, but mismatched:
  Installed: InstalledDist { kind: Registry(InstalledRegistryDist { name: PackageName("cffi"), version: "2.0.0", path: "/home/fleetingbytes/src/rebuild/.venv/lib/python3.11/site-packages/cffi-2.0.0.dist-info", cache_info: None, build_info: Some(BuildInfo { config_settings: ConfigSettings({}), extra_build_requires: [], extra_build_variables: {} }) }), metadata_cache: OnceLock(<uninit>), tags_cache: OnceLock(Some(ExpandedTags([Small { small: WheelTagSmall { python_tag: CPython { python_version: (3, 11) }, abi_tag: CPython { gil_disabled: false, python_version: (3, 11) }, platform_tag: FreeBsd { release_arch: "14_2_release_amd64" } } }]))) }
  Requested: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.0.0" }]), index: Some(IndexMetadata { url: Pypi(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }
DEBUG Identified uncached distribution: cffi==2.0.0
TRACE Comparing installed with source: InstalledDist { kind: Registry(InstalledRegistryDist { name: PackageName("pycparser"), version: "2.23", path: "/home/fleetingbytes/src/rebuild/.venv/lib/python3.11/site-packages/pycparser-2.23.dist-info", cache_info: None, build_info: None }), metadata_cache: OnceLock(<uninit>), tags_cache: OnceLock(<uninit>) } Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.23" }]), index: Some(IndexMetadata { url: Pypi(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }
DEBUG Requirement already installed: pycparser==2.23
TRACE Checking lock for `/home/fleetingbytes/.cache/uv/sdists-v9/pypi/cffi/2.0.0` at `/home/tejul/.cache/uv/sdists-v9/pypi/cffi/2.0.0/.lock`
DEBUG Acquired lock for `/home/fleetingbytes/.cache/uv/sdists-v9/pypi/cffi/2.0.0`
TRACE Response from https://files.pythonhosted.org/packages/eb/56/b1ba7935a17738ae8453301356628e8147c79dbb825bcbc73dc7401f9846/cffi-2.0.0.tar.gz is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/eb/56/b1ba7935a17738ae8453301356628e8147c79dbb825bcbc73dc7401f9846/cffi-2.0.0.tar.gz
   Building cffi==2.0.0
```

The full log is attached

[full_log.txt](https://github.com/user-attachments/files/22287886/full_log.txt)

The same rebuild errors happen with [ruff] and [ty] dependencies 😟.

[cffi]: https://pypi.org/project/cffi/
[cryptography]: https://pypi.org/project/cryptography/
[ruff]: https://astral.sh/ruff/
[ty]: https://astral.sh/ty/

### Platform

FreeBSD 14.2-RELEASE amd64

### Version

uv 0.8.17 (21a92c163 2025-09-11)

### Python version

Python 3.11.13

---

_Label `bug` added by @fleetingbytes on 2025-09-12 01:41_

---

_Comment by @charliermarsh on 2025-09-12 14:58_

Thanks. Out of curiosity, what's the exact name of the wheel that gets built here?

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-12 15:05_

---

_Comment by @fleetingbytes on 2025-09-12 17:04_

@charliermarsh do you mean the contents of the cffi WHEEL file?:
```
Wheel-Version: 1.0
Generator: setuptools (80.9.0)
Root-Is-Purelib: false
Tag: cp311-cp311-freebsd_14_2_release_amd64
```

---

_Comment by @charliermarsh on 2025-09-12 17:06_

Yeah that's good enough, thanks. I was curious if it was `freebsd_14_2_release_amd64` or something else.

---

_Comment by @konstin on 2025-09-14 11:21_

@fleetingbytes Could you share the output of `python -m sysconfig`?

---

_Comment by @eabase on 2025-09-14 13:30_

Because someone is adding variables with ANSI graphic codes into a case sensitive RegEx check. 

---

_Comment by @charliermarsh on 2025-09-14 13:39_

(The graphic codes aren't relevant here, that's just from copy-pasting the error message.)

---

_Closed by @charliermarsh on 2025-09-14 14:48_

---

_Comment by @fleetingbytes on 2025-09-14 15:28_

> (The graphic codes aren't relevant here, that's just from copy-pasting the error message.)

@charliermarsh actually, they are not an artifact of copy-pasting. It is really how the log output looked like. It had correctly interpreted color codes around the word `DEBUG` or `TRACE` at the beginning of the line (they appeared in their respective colors) but there were still some uninterpreted color codes in the following message.

[system-python-sysconfig.txt](https://github.com/user-attachments/files/22321197/system-python-sysconfig.txt) I thing I will post it as a separate issue: #15840

---

_Comment by @michael-o on 2025-10-24 22:23_

@fleetingbytes Thank you so much for raising and @charliermarsh for fixing it. I had the exact same issue this summer on FreeBSD 13.5 and wanted to report this, but didn't find time. I reported the same issue with maturin and provided the fix in August: https://github.com/PyO3/maturin/commit/43527f2aef4e84ab1c7fab104681e577aff4b94d

As far as I can see in the commit you referenced maturin so, all setuptools, maturin and uv are in line with the platform tag generation on FreeBSD.

Thank again, guys!

For the other FreeBSD guys: I am currently working on improving the ports system to autogenerate wheels and collect by poudriere which can be feed into pypi-server and consumed by uv. Some hefty Fortran-/rust-based deps (usual suspects) now take with uv only seconds to resolve:
```
$ UNAME_r=13.5-RELEASE NETRC=.netrc uv  --cache-dir=uv-cache sync
Using CPython 3.10.18 interpreter at: /usr/local/bin/python3
Creating virtual environment at: .venv
Resolved 48 packages in 3.27s
Prepared 46 packages in 1.87s
Installed 46 packages in 53ms
 + amqp==5.2.0
 + annotated-types==0.7.0
 + attrs==25.4.0
 + billiard==4.2.1
 + cafe-client-python==0.1.1
 + celery==5.3.6
 + certifi==2025.10.5
 + charset-normalizer==3.4.4
 + click==8.1.7
 + click-didyoumean==0.3.0
 + click-plugins==1.1.1.2
 + click-repl==0.3.0
 + decorator==5.2.1
 + greenlet==3.2.4
 + gssapi==1.8.3
 + idna==3.11
 + joblib==1.5.1
 + jsonschema==4.25.1
 + jsonschema-specifications==2025.9.1
 + kombu==5.3.7
 + numpy==1.26.4
 + pandas==2.2.3
 + pip-system-certs==5.0
 + prompt-toolkit==3.0.52
 + pydantic==2.11.10
 + pydantic-core==2.33.2
 + pyodbc==5.0.1
 + python-dateutil==2.9.0
 + pytz==2025.2
 + pyyaml==6.0.3
 + referencing==0.37.0
 + requests==2.32.5
 + requests-gssapi==1.4.0
 + rpds-py==0.27.1
 + scikit-learn==1.4.0
 + scipy==1.11.1
 + six==1.17.0
 + sqlalchemy==2.0.44
 + threadpoolctl==3.6.0
 + truststore==0.10.4
 + typing-extensions==4.15.0
 + typing-inspection==0.4.2
 + tzdata==2025.2
 + urllib3==2.5.0
 + vine==5.1.0
 + wcwidth==0.2.8
```

---
