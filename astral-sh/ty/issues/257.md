```yaml
number: 257
title: "`lint:unresolved-import` for some dependencies"
type: issue
state: closed
author: Secrus
labels:
  - bug
  - imports
assignees: []
created_at: 2025-05-08T00:15:27Z
updated_at: 2025-08-19T13:47:19Z
url: https://github.com/astral-sh/ty/issues/257
synced_at: 2026-01-10T02:06:24Z
```

# `lint:unresolved-import` for some dependencies

---

_Issue opened by @Secrus on 2025-05-08 00:15_

### Summary

When running `ty` on `poetry` [repo](https://github.com/python-poetry/poetry) main branch, `lint:unresolved-import` errors are raised for [`dulwich`](https://pypi.org/project/dulwich) and [`xattr`](https://pypi.org/project/xattr) packages. 
Example of such error:
```
error: lint:unresolved-import: Cannot resolve imported module `dulwich.repo`
  --> tests/vcs/git/test_backend.py:8:6
   |
 6 | import pytest
 7 |
 8 | from dulwich.repo import Repo
   |      ^^^^^^^^^^^^
 9 |
10 | from poetry.vcs.git.backend import Git
   |
info: `lint:unresolved-import` is enabled by default
```
Code that fails with the above error is [here](https://github.com/python-poetry/poetry/blob/84eeadc21f92a04d46ea769e3e39d7c902e44136/tests/vcs/git/test_backend.py#L8)




### Version

ty 0.0.0-alpha.7

---

_Label `bug` added by @carljm on 2025-05-08 04:39_

---

_Comment by @carljm on 2025-05-08 05:07_

Thanks!

---

_Comment by @sharkdp on 2025-05-08 07:03_

I can not reproduce this with properly installed dependencies:

```bash
# Without anything installed
▶ ty check --output-format concise tests/vcs/git/test_backend.py
error[lint:unresolved-import] tests/vcs/git/test_backend.py:6:8: Cannot resolve import `pytest`
error[lint:unresolved-import] tests/vcs/git/test_backend.py:8:6: Cannot resolve import `dulwich.repo`
Found 2 diagnostics

# With normal dependencies installed (this includes dulwich, but not pytest)
▶ uv run ty check --output-format concise tests/vcs/git/test_backend.py
error[lint:unresolved-import] tests/vcs/git/test_backend.py:6:8: Cannot resolve import `pytest`
Found 1 diagnostic
```

What's strange is the following behavior: if I try to layer "pytest" on top of the project dependencies, the `dulwich.repo` import error comes back:
```
▶ uv run --with pytest ty check --output-format concise tests/vcs/git/test_backend.py
error[lint:unresolved-import] tests/vcs/git/test_backend.py:8:6: Cannot resolve import `dulwich.repo`
```

But if I explicitly add `dulwich` again (which should not be needed?), no errors are emitted:
```
▶ uv run --with pytest --with dulwich ty check --output-format concise tests/vcs/git/test_backend.py
All checks passed!
```

---

_Comment by @pavelzw on 2025-05-08 08:56_

might be related to #265

---

_Comment by @Secrus on 2025-05-08 10:12_

@sharkdp I reproduced it with
```sh
uvx poetry add --dev ty
uvx poetry run ty check
```
on the poetry code from main branch. 

---

_Comment by @sharkdp on 2025-05-08 10:24_

I can not see any `dulwich` import errors if I try your two commands on commit 84eeadc21f92a04d46ea769e3e39d7c902e44136 of poetry. I do see import errors for `tomli`, `importlib_metadata`, `setuptools` and `xattr`:
```
▶ uvx poetry run ty check --output-format concise | rg unresolved-import
error[lint:unresolved-import] src/poetry/utils/_compat.py:11:12: Cannot resolve imported module `tomli`
error[lint:unresolved-import] src/poetry/utils/_compat.py:18:12: Cannot resolve imported module `importlib_metadata`
error[lint:unresolved-import] tests/fixtures/extended_with_no_setup/build.py:6:6: Cannot resolve imported module `setuptools`
error[lint:unresolved-import] tests/fixtures/extended_with_no_setup/build.py:7:6: Cannot resolve imported module `setuptools`
error[lint:unresolved-import] tests/fixtures/extended_with_no_setup/build.py:8:6: Cannot resolve imported module `setuptools.command.build_ext`
error[lint:unresolved-import] tests/fixtures/git/github.com/demo/namespace-package-one/setup.py:3:6: Cannot resolve imported module `setuptools`
error[lint:unresolved-import] tests/fixtures/git/github.com/demo/namespace-package-one/setup.py:4:6: Cannot resolve imported module `setuptools`
error[lint:unresolved-import] tests/fixtures/git/github.com/demo/no-dependencies/setup.py:3:6: Cannot resolve imported module `setuptools`
error[lint:unresolved-import] tests/fixtures/git/github.com/demo/no-version/setup.py:6:6: Cannot resolve imported module `setuptools`
error[lint:unresolved-import] tests/fixtures/git/github.com/demo/non-canonical-name/setup.py:3:6: Cannot resolve imported module `setuptools`
error[lint:unresolved-import] tests/fixtures/project_with_setup/setup.py:3:6: Cannot resolve imported module `setuptools`
error[lint:unresolved-import] tests/fixtures/project_with_setup_calls_script/setup.py:5:6: Cannot resolve imported module `setuptools`
error[lint:unresolved-import] tests/utils/env/test_env_manager.py:1267:12: Cannot resolve imported module `xattr`
```

---

_Label `module-resolution` added by @carljm on 2025-05-08 14:30_

---

_Comment by @eekcoopuw on 2025-05-08 22:50_

I get this error with dependencies installed in a `conda` environment. They will have been installed using some flavor of `pip install .` and a setup.cfg file.

I have no reason to think the dependencies are truly missing from the conda environment; the test runs in any case:

```
(fhs-pipeline-mortality) mylaptop:fhs-pipeline-mortality eekcoop$ pytest tests/test_squeeze.py 
=================================== test session starts ===================================
platform darwin -- Python 3.10.16, pytest-7.2.2, pluggy-1.5.0
rootdir: /Users/eekcoop/devel/fhs-pipeline-mortality, configfile: pytest.ini
plugins: xdoctest-1.1.6, anyio-4.9.0, typeguard-3.0.2, cov-4.0.0, xdist-3.5.0
collected 10 items                                                                        

tests/test_squeeze.py ..........                                                    [100%]

==================================== warnings summary =====================================
tests/test_squeeze.py::TestSqueeze::test_squeeze[base_data0]
  <frozen importlib._bootstrap>:241: RuntimeWarning: numpy.ndarray size changed, may indicate binary incompatibility. Expected 16 from C header, got 96 from PyObject

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
============================== 10 passed, 1 warning in 1.81s ==============================
(fhs-pipeline-mortality) mylaptop:fhs-pipeline-mortality eekcoop$ ty check tests/test_squeeze.py 
error: lint:unresolved-import: Cannot resolve imported module `pandas`
 --> tests/test_squeeze.py:6:8
  |
4 | from unittest.mock import Mock
5 |
6 | import pandas as pd
  |        ^^^^^^^^^^^^
7 | import pytest
8 | import xarray as xr
  |
info: `lint:unresolved-import` is enabled by default
```
Note the tests pass about halfway through. The `ty check` fails however on the import of `pandas`.

---

_Comment by @AlexWaygood on 2025-05-08 22:53_

@eekcoopuw see https://github.com/astral-sh/ty/issues/265 -- we currently only support virtual environments, not conda environments. We'd like to resolve that limitation, however!

---

_Comment by @sharkdp on 2025-05-12 10:17_

@Secrus It would be great if you could also show the output that you get, and highlight the part which you think shows the bug. As it stands, I can not reproduce the problem outlined in the first post here.

---

_Comment by @Dev-iL on 2025-07-13 08:32_

I'd like to add that packages that only exists as a "nested namespace", such as `ruamel.yaml` (i.e. `ruamel` itself is not importable) are treated as unresolved.

---

_Comment by @sharkdp on 2025-07-14 06:48_

> I'd like to add that packages that only exists as a "nested namespace", such as `ruamel.yaml` (i.e. `ruamel` itself is not importable) are treated as unresolved.

Please see #133 

---

_Closed by @AlexWaygood on 2025-08-19 11:53_

---

_Comment by @AlexWaygood on 2025-08-19 13:47_

@Secrus, I believe this _should_ now be fixed in v0.0.1a19 -- please let me know if you're still experiencing the error after upgrading!

---
