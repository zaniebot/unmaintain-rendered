```yaml
number: 10542
title: "0.3.4: pytest fails because `No data_provider tests were created for test_type_availability` ans some warnings"
type: issue
state: closed
author: kloczek
labels:
  - question
assignees: []
created_at: 2024-03-24T09:03:17Z
updated_at: 2024-03-25T13:06:26Z
url: https://github.com/astral-sh/ruff/issues/10542
synced_at: 2026-01-12T15:54:50Z
```

# 0.3.4: pytest fails because `No data_provider tests were created for test_type_availability` ans some warnings

---

_@kloczek_

I'm packaging your module as an rpm package so I'm using the typical PEP517 based build, install and test cycle used on building packages from non-root account.
- `python3 -sBm build -w --no-isolation`
- because I'm calling `build` with `--no-isolation` I'm using during all processes only locally installed modules
- install .whl file in </install/prefix> using `installer` module
- run pytest with $PYTHONPATH pointing to sitearch and sitelib inside </install/prefix>
- build is performed in env which is *`cut off from access to the public network`* (pytest is executed with `-m "not network"`)

<details>
<summary>Here is pytest output:</summary>

```console
+ PYTHONPATH=/home/tkloczko/rpmbuild/BUILDROOT/python-ruff-0.3.4-2.fc36.x86_64/usr/lib64/python3.9/site-packages:/home/tkloczko/rpmbuild/BUILDROOT/python-ruff-0.3.4-2.fc36.x86_64/usr/lib/python3.9/site-packages
+ /usr/bin/pytest -ra -m 'not network' --import-mode=importlib
============================= test session starts ==============================
platform linux -- Python 3.9.18, pytest-8.1.1, pluggy-1.4.0
rootdir: /home/tkloczko/rpmbuild/BUILD/ruff-0.3.4
configfile: pyproject.toml
plugins: hypothesis-6.99.11
collected 2003 items / 2 errors

==================================== ERRORS ====================================
_ ERROR collecting crates/ruff/libcst/libcst/metadata/tests/test_full_repo_manager.py _
crates/ruff/libcst/libcst/metadata/tests/test_full_repo_manager.py:11: in <module>
    from libcst.metadata.tests.test_type_inference_provider import _test_simple_class_helper
<frozen importlib._bootstrap>:1007: in _find_and_load
    ???
<frozen importlib._bootstrap>:986: in _find_and_load_unlocked
    ???
<frozen importlib._bootstrap>:680: in _load_unlocked
    ???
/usr/lib/python3.9/site-packages/_pytest/assertion/rewrite.py:178: in exec_module
    exec(co, module.__dict__)
/usr/lib64/python3.9/site-packages/libcst/metadata/tests/test_type_inference_provider.py:19: in <module>
    from libcst.tests.test_pyre_integration import TEST_SUITE_PATH
<frozen importlib._bootstrap>:1007: in _find_and_load
    ???
<frozen importlib._bootstrap>:986: in _find_and_load_unlocked
    ???
<frozen importlib._bootstrap>:680: in _load_unlocked
    ???
/usr/lib/python3.9/site-packages/_pytest/assertion/rewrite.py:178: in exec_module
    exec(co, module.__dict__)
/usr/lib64/python3.9/site-packages/libcst/tests/test_pyre_integration.py:87: in <module>
    class PyreIntegrationTest(UnitTest):
/usr/lib64/python3.9/site-packages/libcst/testing/utils.py:168: in __new__
    populate_data_provider_tests(dct)
/usr/lib64/python3.9/site-packages/libcst/testing/utils.py:94: in populate_data_provider_tests
    raise ValueError(
E   ValueError: No data_provider tests were created for test_type_availability! Please double check your data.
_ ERROR collecting crates/ruff/libcst/libcst/metadata/tests/test_type_inference_provider.py _
crates/ruff/libcst/libcst/metadata/tests/test_type_inference_provider.py:19: in <module>
    from libcst.tests.test_pyre_integration import TEST_SUITE_PATH
<frozen importlib._bootstrap>:1007: in _find_and_load
    ???
<frozen importlib._bootstrap>:986: in _find_and_load_unlocked
    ???
<frozen importlib._bootstrap>:680: in _load_unlocked
    ???
/usr/lib/python3.9/site-packages/_pytest/assertion/rewrite.py:178: in exec_module
    exec(co, module.__dict__)
/usr/lib64/python3.9/site-packages/libcst/tests/test_pyre_integration.py:87: in <module>
    class PyreIntegrationTest(UnitTest):
/usr/lib64/python3.9/site-packages/libcst/testing/utils.py:168: in __new__
    populate_data_provider_tests(dct)
/usr/lib64/python3.9/site-packages/libcst/testing/utils.py:94: in populate_data_provider_tests
    raise ValueError(
E   ValueError: No data_provider tests were created for test_type_availability! Please double check your data.
=============================== warnings summary ===============================
crates/ruff/libcst/libcst/codemod/tests/test_metadata.py:15
  /home/tkloczko/rpmbuild/BUILD/ruff-0.3.4/crates/ruff/libcst/libcst/codemod/tests/test_metadata.py:15: PytestCollectionWarning: cannot collect test class 'TestingCollector' because it has a __init__ constructor (from: crates/ruff/libcst/libcst/codemod/tests/test_metadata.py)
    class TestingCollector(ContextAwareVisitor):

crates/ruff/libcst/libcst/codemod/tests/test_metadata.py:23
  /home/tkloczko/rpmbuild/BUILD/ruff-0.3.4/crates/ruff/libcst/libcst/codemod/tests/test_metadata.py:23: PytestCollectionWarning: cannot collect test class 'TestingTransform' because it has a __init__ constructor (from: crates/ruff/libcst/libcst/codemod/tests/test_metadata.py)
    class TestingTransform(ContextAwareTransformer):

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
=========================== short test summary info ============================
ERROR crates/ruff/libcst/libcst/metadata/tests/test_full_repo_manager.py - Va...
ERROR crates/ruff/libcst/libcst/metadata/tests/test_type_inference_provider.py
!!!!!!!!!!!!!!!!!!! Interrupted: 2 errors during collection !!!!!!!!!!!!!!!!!!!!
======================== 2 warnings, 2 errors in 10.94s ========================
```
</details>

Please let me know if you need more details or want me to perform some diagnostics.


---

_Comment by @kloczek on 2024-03-24 12:50_

Those two failing units is result of adding to ruff tree libcst source which was necessary before.
Looks like currently pytest is not able to find any units.
```console
+ PYTHONPATH=/home/tkloczko/rpmbuild/BUILDROOT/python-ruff-0.3.4-2.fc36.x86_64/usr/lib64/python3.9/site-packages:/home/tkloczko/rpmbuild/BUILDROOT/python-ruff-0.3.4-2.fc36.x86_64/usr/lib/py
thon3.9/site-packages
+ /usr/bin/pytest -ra -m 'not network' --import-mode=importlib --deselect crates/ruff/libcst/libcst/metadata/tests/test_full_repo_manager.py --deselect crates/ruff/libcst/libcst/metadata/te
sts/test_type_inference_provider.py
============================= test session starts ==============================
platform linux -- Python 3.9.18, pytest-8.1.1, pluggy-1.4.0
rootdir: /home/tkloczko/rpmbuild/BUILD/ruff-0.3.4
configfile: pyproject.toml
plugins: hypothesis-6.99.11
collected 0 items

============================ no tests ran in 1.57s =============================
```


---

_Comment by @charliermarsh on 2024-03-24 19:59_

Can you say more about what you're trying to do? Ruff doesn't include any Python unit tests.

---

_Label `question` added by @charliermarsh on 2024-03-24 19:59_

---

_Comment by @kloczek on 2024-03-24 23:42_

> Can you say more about what you're trying to do? Ruff doesn't include any Python unit tests.

That is what I found after remove add libcst tree.
So there is no test suite at all? 🤔 

---

_Comment by @charliermarsh on 2024-03-24 23:45_

There's an extensive test suite in the Rust crates. We run it with `cargo test`, not `pytest`.

---

_Comment by @kloczek on 2024-03-25 00:04_

Hmm .. just found that there is no to much python code installed by `ruff` (only one routine which actually returns path to ruff executable).
Simple in this case it is a bit strange that this tool is registered pyton module on pypi 🤔 
OK closing.

---

_Closed by @kloczek on 2024-03-25 00:04_

---

_Comment by @zanieb on 2024-03-25 00:33_

This is a tool for using in Python projects so it's very convenient for us to ship a PyPI distribution for users to install via their normal Python project tooling.

---

_Comment by @kloczek on 2024-03-25 13:06_

Yes of course however actually included python code seems has nothing to do with `ruff` any python interface.

---
