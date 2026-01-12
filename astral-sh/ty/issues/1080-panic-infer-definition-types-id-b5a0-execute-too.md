```yaml
number: 1080
title: "[panic] infer_definition_types(Id(b5a0)): execute: too many cycle iterations"
type: issue
state: closed
author: janbernloehr
labels:
  - fatal
assignees: []
created_at: 2025-08-21T11:13:33Z
updated_at: 2025-08-21T11:17:32Z
url: https://github.com/astral-sh/ty/issues/1080
synced_at: 2026-01-12T15:54:24Z
```

# [panic] infer_definition_types(Id(b5a0)): execute: too many cycle iterations

---

_@janbernloehr_

This panic does only happen every ~10th time of running it against our codebase

Code in question is
```
from __future__ import annotations

from collections.abc import Sequence
from enum import Enum
from typing import Any, Protocol, TypeAlias, TypeVar, runtime_checkable

import torch
from typing_extensions import Self


@runtime_checkable
class TransferableDataContainer(Protocol):

    def to(self, device: torch.device | str) -> Self: ...

    @classmethod
    def stack(cls, instance_list: Sequence[Self], dim: int = 0) -> Self: ...


ContainerTypeT = TypeVar("ContainerTypeT", bound=TransferableDataContainer)


def collate_transferable_container(batch: Sequence[ContainerTypeT], *, collate_fn_map: Any) -> ContainerTypeT:
    return batch[0].stack(batch)


NestedTensorTuple: TypeAlias = tuple["torch.Tensor | None | NestedTensorTuple", ...]  # enables (nested) NamedTuples
TensorDict: TypeAlias = (
    dict[str, torch.Tensor]
    | dict[Enum, torch.Tensor]
    | dict[str | Enum, torch.Tensor]
    | dict[str, dict[str, torch.Tensor] | torch.Tensor]
)
BaseTypes: TypeAlias = torch.Tensor | TensorDict | TransferableDataContainer | NestedTensorTuple
SupportedData: TypeAlias = BaseTypes | tuple[BaseTypes, ...] | dict[str, BaseTypes]

DatasetRetTypeT = TypeVar("DatasetRetTypeT", bound=SupportedData)
ModelRetTypeT = TypeVar("ModelRetTypeT", bound=SupportedData)

@runtime_checkable
class Detachable(Protocol):
    def detach(self) -> Self: ...


SupportedStateTypes = torch.Tensor | Detachable
TemporalStateTypeT = TypeVar("TemporalStateTypeT", bound=SupportedStateTypes | dict[str, SupportedStateTypes])

```

Stacktrace is
```
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/bje2lr/workspace/xflow3/xtorch/src/xtorch/training/data_container.py`: `infer_definition_types(Id(b5a0)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.19
info: Args: ["ty", "check", "--project", "xtorch"]
info: Backtrace:
   0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: <unknown>
  19: <unknown>
  20: <unknown>
  21: <unknown>
  22: <unknown>
  23: <unknown>
  24: <unknown>
  25: <unknown>
  26: <unknown>
  27: <unknown>
  28: <unknown>
  29: <unknown>
  30: start_thread
             at ./nptl/pthread_create.c:442:8
  31: __GI___clone3
             at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:81:0

info: query stacktrace:
   0: infer_scope_types(Id(6864))
             at crates/ty_python_semantic/src/types/infer.rs:138
   1: check_file_impl(Id(3009))
             at crates/ty_project/src/lib.rs:527
```


---

_Label `fatal` added by @AlexWaygood on 2025-08-21 11:16_

---

_Comment by @AlexWaygood on 2025-08-21 11:17_

Thanks for the report! It looks like you have a recursive type alias there, so I think this is a duplicate of #256 

---

_Closed by @AlexWaygood on 2025-08-21 11:17_

---
