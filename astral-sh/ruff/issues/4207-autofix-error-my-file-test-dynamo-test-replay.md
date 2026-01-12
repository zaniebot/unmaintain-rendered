```yaml
number: 4207
title: "[Autofix error] My File: test\\dynamo\\test_replay_record.py"
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
assignees: []
created_at: 2023-05-03T13:43:39Z
updated_at: 2023-05-04T14:44:04Z
url: https://github.com/astral-sh/ruff/issues/4207
synced_at: 2026-01-12T15:54:44Z
```

# [Autofix error] My File: test\dynamo\test_replay_record.py

---

_@FishAlchemist_


ruff version 0.0.264 (ruff --version)

## command(Powershell)
```powershell
ruff check --select=ALL --fix --isolated .\test_replay_record.py
```
The source is a Python file from PyTorch, but it is not the original file (with a different hash value).
The original file can be found at https://github.com/pytorch/pytorch/blob/8b64dee5d2e0e61c3c37a222c43b636d20cc58b7/test/dynamo/test_replay_record.py
OS: Windows 11 22H2
## Terminal Content:
```powershell
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `test_replay_record.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

test_replay_record.py:1:1: D100 Missing docstring in public module
test_replay_record.py:18:7: D101 Missing docstring in public class
test_replay_record.py:18:25: SLF001 Private member accessed: `_dynamo`
test_replay_record.py:20:9: ANN206 Missing return type annotation for classmethod `setUpClass`
test_replay_record.py:20:9: D102 Missing docstring in public method
test_replay_record.py:20:20: ANN102 Missing type annotation for `cls` in classmethod
test_replay_record.py:24:17: SLF001 Private member accessed: `_dynamo`
test_replay_record.py:24:64: FBT003 Boolean positional value in function call
test_replay_record.py:28:40: SLF001 Private member accessed: `_dynamo`
test_replay_record.py:28:84: FBT003 Boolean positional value in function call
test_replay_record.py:28:89: E501 Line too long (89 > 88 characters)
test_replay_record.py:33:40: SLF001 Private member accessed: `_dynamo`
test_replay_record.py:33:81: FBT003 Boolean positional value in function call
test_replay_record.py:37:17: SLF001 Private member accessed: `_dynamo`
test_replay_record.py:39:17: S108 Probable insecure usage of temporary file or directory: "/tmp/_torchdynamo_debug_/"
test_replay_record.py:44:9: ANN206 Missing return type annotation for classmethod `tearDownClass`
test_replay_record.py:44:9: D102 Missing docstring in public method
test_replay_record.py:44:23: ANN102 Missing type annotation for `cls` in classmethod
test_replay_record.py:45:23: SLF001 Private member accessed: `_dynamo`
test_replay_record.py:48:9: ANN201 Missing return type annotation for public function `check_replay`
test_replay_record.py:48:9: D102 Missing docstring in public method
test_replay_record.py:48:22: ANN101 Missing type annotation for `self` in method
test_replay_record.py:48:28: ANN001 Missing type annotation for function argument `fn`
test_replay_record.py:48:33: ANN002 Missing type annotation for `*args`
test_replay_record.py:48:39: ANN001 Missing type annotation for function argument `exp_exc_name`
test_replay_record.py:49:18: SLF001 Private member accessed: `_dynamo`
test_replay_record.py:51:13: SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
test_replay_record.py:53:13: S110 `try`-`except`-`pass` detected, consider logging the exception
test_replay_record.py:53:20: BLE001 Do not catch blind exception: `Exception`
test_replay_record.py:62:13: S101 Use of `assert` detected
test_replay_record.py:62:89: E501 Line too long (94 > 88 characters)
test_replay_record.py:64:13: SLF001 Private member accessed: `_dynamo`
test_replay_record.py:66:13: ANN202 Missing return type annotation for private function `get_error_name`
test_replay_record.py:66:28: ANN001 Missing type annotation for function argument `log`
test_replay_record.py:68:13: S101 Use of `assert` detected
test_replay_record.py:74:13: S101 Use of `assert` detected
test_replay_record.py:76:9: S101 Use of `assert` detected
test_replay_record.py:76:89: E501 Line too long (117 > 88 characters)
test_replay_record.py:79:9: ANN201 Missing return type annotation for public function `test_unsuccessful_inline`
test_replay_record.py:79:9: D102 Missing docstring in public method
test_replay_record.py:79:34: ANN101 Missing type annotation for `self` in method
test_replay_record.py:80:13: ANN202 Missing return type annotation for private function `level2`
test_replay_record.py:85:13: ANN202 Missing return type annotation for private function `level1`
test_replay_record.py:89:13: ANN202 Missing return type annotation for private function `level0`
test_replay_record.py:96:9: ANN201 Missing return type annotation for public function `test_successful_inline`
test_replay_record.py:96:9: D102 Missing docstring in public method
test_replay_record.py:96:32: ANN101 Missing type annotation for `self` in method
test_replay_record.py:97:13: ANN202 Missing return type annotation for private function `test_fn`
test_replay_record.py:100:17: ANN202 Missing return type annotation for private function `level1`
test_replay_record.py:100:24: ANN001 Missing type annotation for function argument `a`
test_replay_record.py:110:9: ANN201 Missing return type annotation for public function `test_nonlocal_fn_call`
test_replay_record.py:110:9: D102 Missing docstring in public method
test_replay_record.py:110:31: ANN101 Missing type annotation for `self` in method
test_replay_record.py:111:13: ANN202 Missing return type annotation for private function `nonlocal_fn`
test_replay_record.py:111:25: ANN001 Missing type annotation for function argument `x`
test_replay_record.py:114:13: ANN202 Missing return type annotation for private function `test_fn`
test_replay_record.py:122:9: ANN201 Missing return type annotation for public function `test_nonlocal_module_fn_call`
test_replay_record.py:122:9: D102 Missing docstring in public method
test_replay_record.py:122:38: ANN101 Missing type annotation for `self` in method
test_replay_record.py:130:13: ANN202 Missing return type annotation for private function `test_fn`
test_replay_record.py:138:9: ANN201 Missing return type annotation for public function `test_nonlocal_module_class`
test_replay_record.py:138:9: D102 Missing docstring in public method
test_replay_record.py:138:36: ANN101 Missing type annotation for `self` in method
test_replay_record.py:144:13: ANN202 Missing return type annotation for private function `test_fn`
test_replay_record.py:152:9: ANN201 Missing return type annotation for public function `test_local_module`
test_replay_record.py:152:9: D102 Missing docstring in public method
test_replay_record.py:152:27: ANN101 Missing type annotation for `self` in method
test_replay_record.py:156:17: ANN202 Missing return type annotation for private function `test_fn`
test_replay_record.py:156:25: ANN001 Missing type annotation for function argument `x`
test_replay_record.py:164:17: ANN202 Missing return type annotation for private function `test_fn`
test_replay_record.py:164:25: ANN001 Missing type annotation for function argument `x`
test_replay_record.py:174:9: ANN201 Missing return type annotation for public function `test_fn_call_args`
test_replay_record.py:174:9: D102 Missing docstring in public method
test_replay_record.py:174:27: ANN101 Missing type annotation for `self` in method
test_replay_record.py:175:13: ANN202 Missing return type annotation for private function `test_fn`
test_replay_record.py:175:21: ANN001 Missing type annotation for function argument `x`
test_replay_record.py:175:24: ANN001 Missing type annotation for function argument `y`
Found 77 errors.
```
## File Content:
```python
import logging
import re
import shutil
import unittest

import torch
import torch._dynamo.test_case
import torch._dynamo.testing

try:
    import dill
except ImportError:
    dill = None

requires_dill = unittest.skipIf(dill is None, "requires dill")


class ReplayRecordTests(torch._dynamo.test_case.TestCase):
    @classmethod
    def setUpClass(cls):
        super().setUpClass()
        cls._exit_stack.enter_context(
            unittest.mock.patch.object(
                torch._dynamo.config, "replay_record_enabled", True,
            ),
        )
        cls._exit_stack.enter_context(
            unittest.mock.patch.object(torch._dynamo.config, "print_graph_breaks", True),
        )
        # Most of the tests are checking to see if errors got logged, so we
        # ask for errors to be suppressed
        cls._exit_stack.enter_context(
            unittest.mock.patch.object(torch._dynamo.config, "suppress_errors", True),
        )
        cls._exit_stack.enter_context(
            unittest.mock.patch.object(
                torch._dynamo.config,
                "debug_dir_root",
                "/tmp/_torchdynamo_debug_/",
            ),
        )

    @classmethod
    def tearDownClass(cls):
        shutil.rmtree(torch._dynamo.config.debug_dir_root, ignore_errors=True)
        cls._exit_stack.close()

    def check_replay(self, fn, *args, exp_exc_name=None):
        fn_opt = torch._dynamo.optimize("eager")(fn)
        with self.assertLogs(logger="torch._dynamo", level=logging.ERROR) as log_orig:
            try:
                fn_opt(*args)
            except Exception:
                pass  # we'll check the logs for the raised exception

        with self.assertLogs(
            logger="torch._dynamo", level=logging.ERROR,
        ) as log_replayed:
            file_name_match = re.search(
                r"torch._dynamo\.replay\('(.*)'\)", log_orig.output[-1],
            )
            assert file_name_match is not None, "No record file name found in generated logs."

            torch._dynamo.replay(file_name_match.groups()[0])

        def get_error_name(log):
            error_name = re.search(r"\w+Error", log.output[-1])
            assert error_name is not None, "No error name found in logs."
            return error_name[0]

        orig_error = get_error_name(log_orig)
        replayed_error = get_error_name(log_replayed)
        if exp_exc_name is not None:
            assert orig_error == exp_exc_name

        assert orig_error == replayed_error, "Error logs for recorded execution and replayed execution should match."

    @requires_dill
    def test_unsuccessful_inline(self):
        def level2():
            z = torch.ones(2, 2)
            a = {z: 10}  # Error here, tensor as key to dict
            return a[z] * torch.ones(1)

        def level1():
            y = torch.ones(1, 1)
            return level2() + y

        def level0():
            x = torch.ones(1, 1)
            return level1() + x

        self.check_replay(level0, exp_exc_name="AssertionError")

    @requires_dill
    def test_successful_inline(self):
        def test_fn():
            x = torch.ones(2, 2)

            def level1(a):
                return a + torch.ones(2, 2)

            y = level1(x)

            return y + torch.ones(3, 3)  # dimension mismatch

        self.check_replay(test_fn, exp_exc_name="RuntimeError")

    @requires_dill
    def test_nonlocal_fn_call(self):
        def nonlocal_fn(x):
            return x + torch.ones(2, 2)

        def test_fn():
            z = torch.ones(2, 2)
            x = nonlocal_fn(z)
            return x + torch.ones(3, 3)

        self.check_replay(test_fn, exp_exc_name="RuntimeError")

    @requires_dill
    def test_nonlocal_module_fn_call(self):
        # replay when we use a module
        # not defined in the replay env
        try:
            from . import mock_modules
        except ImportError:
            import mock_modules

        def test_fn():
            z = mock_modules.mock_module2.method1([], 2)
            z = torch.ones(2, 2) + z[0]
            return z + torch.zeros(3, 3)

        self.check_replay(test_fn, exp_exc_name="RuntimeError")

    @requires_dill
    def test_nonlocal_module_class(self):
        try:
            from .mock_modules import mock_module2
        except ImportError:
            from mock_modules import mock_module2

        def test_fn():
            z = mock_module2.Class1(1, 2)
            y = z.method2(torch.ones(3, 3))
            return y + torch.zeros(3, 5)

        self.check_replay(test_fn, exp_exc_name="TypeError")

    @requires_dill
    def test_local_module(self):
        try:
            from .mock_modules import mock_module3 as _  # noqa: F401

            def test_fn(x):
                from .mock_modules import mock_module3

                z = mock_module3.method1([], torch.ones(5, 1))
                return torch.ones(2, 2) + x + z[0]

        except ImportError:

            def test_fn(x):
                from mock_modules import mock_module3

                z = mock_module3.method1([], torch.ones(5, 1))
                return torch.ones(2, 2) + x + z[0]

        self.check_replay(test_fn, torch.ones(1, 1), exp_exc_name="RuntimeError")

    # Verfiy that we replay when we have tensor arguments to the frame being replayed
    @requires_dill
    def test_fn_call_args(self):
        def test_fn(x, y):
            return x + y + torch.zeros(2, 2)

        self.check_replay(
            test_fn, torch.ones(3, 3), torch.ones(2, 2), exp_exc_name="RuntimeError",
        )


if __name__ == "__main__":
    from torch._dynamo.test_case import run_tests

    run_tests()
```
## pyproject.toml
```toml
[build-system]
requires = [
    "setuptools",
    "wheel",
    "astunparse",
    "numpy",
    "ninja",
    "pyyaml",
    "setuptools",
    "cmake",
    "typing-extensions",
    "requests",
]
# Use legacy backend to import local packages in setup.py
build-backend = "setuptools.build_meta:__legacy__"


[tool.black]
# Uncomment if pyproject.toml worked fine to ensure consistency with flake8
# line-length = 120
target-version = ["py38", "py39", "py310", "py311"]


[tool.ruff]
target-version = "py38"

# NOTE: Synchoronize the ignores with .flake8
ignore = [
    # these ignores are from flake8-bugbear; please fix!
    "B007", "B008", "B017",
    "B018", # Useless expression
    "B019", "B020",
    "B023", "B024", "B026",
    "B028", # No explicit `stacklevel` keyword argument found
    "B027", "B904", "B905",
    "E402",
    "C408", # C408 ignored because we like the dict keyword argument syntax
    "E501", # E501 is not flexible enough, we're using B950 instead
    "E721",
    "E731", # Assign lambda expression
    "E741",
    "EXE001",
    "F405",
    "F821",
    "F841",
    # these ignores are from flake8-logging-format; please fix!
    "G101", "G201", "G202",
    "SIM102", "SIM103", "SIM112", # flake8-simplify code styles
    "SIM105", # these ignores are from flake8-simplify. please fix or ignore with commented reason
    "SIM108",
    "SIM110",
    "SIM114", # Combine `if` branches using logical `or` operator
    "SIM115",
    "SIM116", # Disable Use a dictionary instead of consecutive `if` statements
    "SIM117",
    "SIM118",
]
line-length = 120
select = [
    "B",
    "C4",
    "G",
    "E",
    "F",
    "SIM1",
    "W",
]

[tool.ruff.per-file-ignores]
"__init__.py" = ["F401"]
"torchgen/api/types/__init__.py" = [
    "F401",
    "F403",
]
"torchgen/executorch/api/types/__init__.py" = [
    "F401",
    "F403",
]
```

---

_Label `bug` added by @charliermarsh on 2023-05-04 02:47_

---

_Comment by @dhruvmanila on 2023-05-04 14:43_

This is fixed by #4215 

> debug error: Autofix introduced a syntax error in `/Users/dhruv/playground/python/ruff-play/repro/test.py` with rule codes SIM105: invalid syntax.
 Got unexpected token 'contextlib' at byte offset 135


---

_Closed by @charliermarsh on 2023-05-04 14:43_

---

_Comment by @charliermarsh on 2023-05-04 14:44_

👍 Thank you @dhruvmanila!

---
