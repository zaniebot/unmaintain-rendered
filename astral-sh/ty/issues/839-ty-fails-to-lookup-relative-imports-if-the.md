```yaml
number: 839
title: ty fails to lookup relative imports if the relative path from the nearest import root includes an invalid module name
type: issue
state: open
author: MichaReiser
labels:
  - imports
assignees: []
created_at: 2025-07-17T12:29:13Z
updated_at: 2025-11-14T17:38:46Z
url: https://github.com/astral-sh/ty/issues/839
synced_at: 2026-01-12T15:54:24Z
```

# ty fails to lookup relative imports if the relative path from the nearest import root includes an invalid module name

---

_@MichaReiser_

Langchain has an [`__init__.py`](https://github.com/langchain-ai/langchain/blob/491f63ca82806a98b18f785d0d315e7f5d8de29f/libs/standard-tests/langchain_tests/unit_tests/__init__.py#L21) with the following content

```py
# ruff: noqa: E402
import pytest

# Rewrite assert statements for test suite so that implementations can
# see the full error message from failed asserts.
# https://docs.pytest.org/en/7.1.x/how-to/writing_plugins.html#assertion-rewriting
modules = [
    "chat_models",
    "embeddings",
    "tools",
]

for module in modules:
    pytest.register_assert_rewrite(f"langchain_tests.unit_tests.{module}")

from .chat_models import ChatModelUnitTests
from .embeddings import EmbeddingsUnitTests
from .tools import ToolsUnitTests

__all__ = ["ChatModelUnitTests", "EmbeddingsUnitTests", "ToolsUnitTests"]
```


ty fails to resolve the relative imports `chat_models`, `embeddings` and `.tools` even though those files clearly exist. Pyright seems fine with the import. 

The issue seems to be that `file_to_module` fails because the relative path to the search root (which is the project root) `libs/standard-tests/langchain_tests/unit_tests/teswt.py` can't be turned into a valid module name because of the `standard-tests`. 

A solution here is to add `standard-tests` to the search path but having to do this is at least confusing in an LSP setup (where ty should just work out of the box)

[Playground](https://play.ty.dev/d35224bf-eb57-47c3-8877-e4a4175702dc)

---

_Label `imports` added by @MichaReiser on 2025-07-17 12:29_

---

_Renamed from "ty fails to lookup relative imports even if they clearly exist in the parent directory" to "ty fails to lookup relative imports if the relative path from the nearest import root includes an invalid module name" by @carljm on 2025-07-18 19:22_

---

_Comment by @MichaReiser on 2025-07-22 12:43_

An alterntive here is to make `file_to_module` less strict. Instead, it tries to walk up the directory chain until it finds no `__init__.py` and uses that as the module name. The behavior would remain unchagned if there's no `__init__.py` (namespace package). 

This would also help in the case where a user opens a source definition for a stub file. `resolve_module` will always return the stub file, making `file_to_module` return `None` for the source module. CC: @Gankra 

---

_Comment by @MichaReiser on 2025-11-14 09:12_

Relative imports also fail if the module uses a name that isn't a valid Python identifier, see https://github.com/astral-sh/ty/issues/1529

---

_Added to milestone `Stable` by @carljm on 2025-11-14 17:38_

---
