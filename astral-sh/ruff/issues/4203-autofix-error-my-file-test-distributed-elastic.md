---
number: 4203
title: "[Autofix error] My File: test/distributed/elastic/rendezvous/etcd_rendezvous_backend_test.py"
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
assignees: []
created_at: 2023-05-03T13:06:29Z
updated_at: 2023-05-04T14:17:43Z
url: https://github.com/astral-sh/ruff/issues/4203
synced_at: 2026-01-10T01:22:43Z
---

# [Autofix error] My File: test/distributed/elastic/rendezvous/etcd_rendezvous_backend_test.py

---

_Issue opened by @FishAlchemist on 2023-05-03 13:06_

**ruff version 0.0.264 (ruff --version)**
## command(Powershell)
```powershell
ruff check --select=ALL --fix --isolated .\etcd_rendezvous_backend_test.py
```
----------
The source is a Python file from PyTorch, but it is not the original file (with a different hash value).
The original file can be found at https://github.com/pytorch/pytorch/blob/8b64dee5d2e0e61c3c37a222c43b636d20cc58b7/test/distributed/elastic/rendezvous/etcd_rendezvous_backend_test.py.
OS: Windows 11 22H2
## Terminal Content:
```powershell
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `etcd_rendezvous_backend_test.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

etcd_rendezvous_backend_test.py:1:1: D100 Missing docstring in public module
etcd_rendezvous_backend_test.py:7:1: I001 Import block is un-sorted or un-formatted
etcd_rendezvous_backend_test.py:15:89: E501 Line too long (96 > 88 characters)
etcd_rendezvous_backend_test.py:24:7: D101 Missing docstring in public class
etcd_rendezvous_backend_test.py:28:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:28:20: ANN102 Missing type annotation for `cls` in classmethod
etcd_rendezvous_backend_test.py:33:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:33:23: ANN102 Missing type annotation for `cls` in classmethod
etcd_rendezvous_backend_test.py:36:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:36:15: ANN101 Missing type annotation for `self` in method
etcd_rendezvous_backend_test.py:40:9: SIM105 Use `contextlib.suppress(EtcdKeyNotFound)` instead of `try`-`except`-`pass`
etcd_rendezvous_backend_test.py:45:89: E501 Line too long (92 > 88 characters)
etcd_rendezvous_backend_test.py:47:24: ANN101 Missing type annotation for `self` in method
etcd_rendezvous_backend_test.py:51:7: D101 Missing docstring in public class
etcd_rendezvous_backend_test.py:55:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:55:20: ANN102 Missing type annotation for `cls` in classmethod
etcd_rendezvous_backend_test.py:60:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:60:23: ANN102 Missing type annotation for `cls` in classmethod
etcd_rendezvous_backend_test.py:63:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:63:15: ANN101 Missing type annotation for `self` in method
etcd_rendezvous_backend_test.py:76:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:76:45: ANN101 Missing type annotation for `self` in method
etcd_rendezvous_backend_test.py:79:9: S101 Use of `assert` detected
etcd_rendezvous_backend_test.py:81:9: S101 Use of `assert` detected
etcd_rendezvous_backend_test.py:85:9: S101 Use of `assert` detected
etcd_rendezvous_backend_test.py:85:89: E501 Line too long (106 > 88 characters)
etcd_rendezvous_backend_test.py:93:9: S101 Use of `assert` detected
etcd_rendezvous_backend_test.py:94:9: S101 Use of `assert` detected
etcd_rendezvous_backend_test.py:94:30: PLR2004 Magic value used in comparison, consider replacing 7200 with a constant variable
etcd_rendezvous_backend_test.py:100:9: S101 Use of `assert` detected
etcd_rendezvous_backend_test.py:102:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:102:74: ANN101 Missing type annotation for `self` in method
etcd_rendezvous_backend_test.py:107:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:107:78: ANN101 Missing type annotation for `self` in method
etcd_rendezvous_backend_test.py:107:89: E501 Line too long (91 > 88 characters)
etcd_rendezvous_backend_test.py:114:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:114:65: ANN101 Missing type annotation for `self` in method
etcd_rendezvous_backend_test.py:123:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:123:65: ANN101 Missing type annotation for `self` in method
etcd_rendezvous_backend_test.py:126:89: E501 Line too long (90 > 88 characters)
etcd_rendezvous_backend_test.py:129:9: D102 Missing docstring in public method
etcd_rendezvous_backend_test.py:129:69: ANN101 Missing type annotation for `self` in method
Found 42 errors.
```
## File Content:
```py

# Copyright (c) Facebook, Inc. and its affiliates.
# All rights reserved.
#
# This source code is licensed under the BSD-style license found in the
# LICENSE file in the root directory of this source tree.

import subprocess
from base64 import b64encode
from typing import ClassVar, cast
from unittest import TestCase

from etcd import EtcdKeyNotFound  # type: ignore[import]
from rendezvous_backend_test import RendezvousBackendTestMixin

from torch.distributed.elastic.rendezvous import RendezvousConnectionError, RendezvousParameters
from torch.distributed.elastic.rendezvous.etcd_rendezvous_backend import (
    EtcdRendezvousBackend,
    create_backend,
)
from torch.distributed.elastic.rendezvous.etcd_server import EtcdServer
from torch.distributed.elastic.rendezvous.etcd_store import EtcdStore


class EtcdRendezvousBackendTest(TestCase, RendezvousBackendTestMixin):
    _server: ClassVar[EtcdServer]

    @classmethod
    def setUpClass(cls) -> None:
        cls._server = EtcdServer()
        cls._server.start(stderr=subprocess.DEVNULL)

    @classmethod
    def tearDownClass(cls) -> None:
        cls._server.stop()

    def setUp(self) -> None:
        self._client = self._server.get_client()

        # Make sure we have a clean slate.
        try:
            self._client.delete("/dummy_prefix", recursive=True, dir=True)
        except EtcdKeyNotFound:
            pass

        self._backend = EtcdRendezvousBackend(self._client, "dummy_run_id", "/dummy_prefix")

    def _corrupt_state(self) -> None:
        self._client.write("/dummy_prefix/dummy_run_id", "non_base64")


class CreateBackendTest(TestCase):
    _server: ClassVar[EtcdServer]

    @classmethod
    def setUpClass(cls) -> None:
        cls._server = EtcdServer()
        cls._server.start(stderr=subprocess.DEVNULL)

    @classmethod
    def tearDownClass(cls) -> None:
        cls._server.stop()

    def setUp(self) -> None:
        self._params = RendezvousParameters(
            backend="dummy_backend",
            endpoint=self._server.get_endpoint(),
            run_id="dummy_run_id",
            min_nodes=1,
            max_nodes=1,
            protocol="hTTp",
            read_timeout="10",
        )

        self._expected_read_timeout = 10

    def test_create_backend_returns_backend(self) -> None:
        backend, store = create_backend(self._params)

        assert backend.name == "etcd-v2"

        assert isinstance(store, EtcdStore)

        etcd_store = cast(EtcdStore, store)

        assert etcd_store.client.read_timeout == self._expected_read_timeout  # type: ignore[attr-defined]

        client = self._server.get_client()

        backend.set_state(b"dummy_state")

        result = client.get("/torch/elastic/rendezvous/" + self._params.run_id)

        assert result.value == b64encode(b"dummy_state").decode()
        assert result.ttl <= 7200

        store.set("dummy_key", "dummy_value")

        result = client.get("/torch/elastic/store/" + b64encode(b"dummy_key").decode())

        assert result.value == b64encode(b"dummy_value").decode()

    def test_create_backend_returns_backend_if_protocol_is_not_specified(self) -> None:
        del self._params.config["protocol"]

        self.test_create_backend_returns_backend()

    def test_create_backend_returns_backend_if_read_timeout_is_not_specified(self) -> None:
        del self._params.config["read_timeout"]

        self._expected_read_timeout = 60

        self.test_create_backend_returns_backend()

    def test_create_backend_raises_error_if_etcd_is_unreachable(self) -> None:
        self._params.endpoint = "dummy:1234"

        with self.assertRaisesRegex(
            RendezvousConnectionError,
            r"^The connection to etcd has failed. See inner exception for details.$",
        ):
            create_backend(self._params)

    def test_create_backend_raises_error_if_protocol_is_invalid(self) -> None:
        self._params.config["protocol"] = "dummy"

        with self.assertRaisesRegex(ValueError, r"^The protocol must be HTTP or HTTPS.$"):
            create_backend(self._params)

    def test_create_backend_raises_error_if_read_timeout_is_invalid(self) -> None:
        for read_timeout in ["0", "-10"]:
            with self.subTest(read_timeout=read_timeout):
                self._params.config["read_timeout"] = read_timeout

                with self.assertRaisesRegex(
                    ValueError, r"^The read timeout must be a positive integer.$",
                ):
                    create_backend(self._params)

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

_Renamed from "[Autofix error]" to "[Autofix error] My File: test/distributed/elastic/rendezvous/etcd_rendezvous_backend_test.py" by @FishAlchemist on 2023-05-03 13:26_

---

_Comment by @dhruvmanila on 2023-05-03 18:10_

It's coming from `SIM105`. A minimal repro:

If you remove the import, then it'll work. Somehow the import is causing this issue. It works on `0.0.263`.

```python
import math

try:
    int('ab')
except ValueError:
    pass
```

Command to run: `ruff check --select=SIM105 --fix --diff --isolated src/SIM105.py`

---

_Referenced in [astral-sh/ruff#4215](../../astral-sh/ruff/pulls/4215.md) on 2023-05-04 01:10_

---

_Label `bug` added by @charliermarsh on 2023-05-04 02:47_

---

_Referenced in [astral-sh/ruff#4221](../../astral-sh/ruff/issues/4221.md) on 2023-05-04 11:51_

---

_Closed by @MichaReiser on 2023-05-04 14:17_

---
