```yaml
number: 1264
title: "[panic] execute: too many cycle iterations"
type: issue
state: closed
author: wrongways
labels: []
assignees: []
created_at: 2025-09-26T18:00:10Z
updated_at: 2025-09-26T19:11:24Z
url: https://github.com/astral-sh/ty/issues/1264
synced_at: 2026-01-12T15:54:24Z
```

# [panic] execute: too many cycle iterations

---

_@wrongways_

Three files in my simpy simulation application produce the panic below. The simplest of them is this one:

```python
# workflow.py

"""
Workflow specification for clinic simulation.

Defines reusable stage specifications (`StageSpec`) that allow clinic workflows
to be expressed declaratively as a sequence of stages. Each stage describes
the resources required and the distribution of service times. The StageSpec
is then executed by the 'Stage' class.
"""

from collections.abc import Callable
from dataclasses import dataclass

from simpy import Resource


@dataclass(frozen=True, slots=True)
class StageSpec:
    """
    Declarative specification of a single clinic stage.

    A stage comprises:
      - A human-readable name
      - One or more SimPy resources that must be acquired before the service can start
      - A function that generates the service time for this stage

    Attributes:
        name: Name of the stage (e.g. "Reception", "Nurse Consult").
        resources: Resources needed to complete the stage.
        service_time_fn: Callable that returns the duration (float) of the service.
    """

    name: str
    resources: list[Resource]
    service_time_fn: Callable[[], float]

```


```shell
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/Users/abc/Developer/Mac/Python/ClinicSimulation/workflow.py`: `infer_definition_types(Id(bd8a)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.21 (ef52a1940 2025-09-19)
info: Args: ["ty", "check", "workflow.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(383c))
             at crates/ty_python_semantic/src/place.rs:706
   1: Type < 'db >::member_lookup_with_policy_(Id(302b))
             at crates/ty_python_semantic/src/types.rs:768
   2: infer_definition_types(Id(bd1c))
             at crates/ty_python_semantic/src/types/infer.rs:97
   3: place_by_id(Id(383b))
             at crates/ty_python_semantic/src/place.rs:706
   4: Type < 'db >::member_lookup_with_policy_(Id(3027))
             at crates/ty_python_semantic/src/types.rs:768
   5: infer_definition_types(Id(7598))
             at crates/ty_python_semantic/src/types/infer.rs:97
   6: infer_expression_types_impl(Id(1acb))
             at crates/ty_python_semantic/src/types/infer.rs:192
   7: infer_definition_types(Id(75ab))
             at crates/ty_python_semantic/src/types/infer.rs:97
   8: infer_expression_types_impl(Id(1ace))
             at crates/ty_python_semantic/src/types/infer.rs:192
   9: infer_expression_types_impl(Id(1ad0))
             at crates/ty_python_semantic/src/types/infer.rs:192
  10: static_expression_truthiness(Id(1ad0))
             at crates/ty_python_semantic/src/types/infer.rs:391
  11: place_by_id(Id(380e))
             at crates/ty_python_semantic/src/place.rs:706
  12: Type < 'db >::member_lookup_with_policy_(Id(300f))
             at crates/ty_python_semantic/src/types.rs:768
  13: infer_definition_types(Id(1402))
             at crates/ty_python_semantic/src/types/infer.rs:97
  14: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:68
  15: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:522


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

---

_Comment by @MeGaGiGaGon on 2025-09-26 19:10_

This is another manifestation of #256, using the same strategy as in #1098 I was able to minimize the erroring code to this file in simpy https://gitlab.com/team-simpy/simpy/-/blob/master/src/simpy/resources/base.py
And then minimize the file to this code
```py
from __future__ import annotations

from typing import (
    TYPE_CHECKING,
    ClassVar,
    ContextManager,
    Generic,
    MutableSequence,
    Optional,
    Type,
    TypeVar,
    Union,
)


ResourceType = TypeVar('ResourceType', bound='BaseResource')


class Put(Event, ContextManager['Put'], Generic[ResourceType]):...


class Get(Event, ContextManager['Get'], Generic[ResourceType]):...


PutType = TypeVar('PutType', bound=Put)
GetType = TypeVar('GetType', bound=Get)


class BaseResource(Generic[PutType, GetType]):...
```

Where the cycle is `BaseResource -> (PutType, GetType) -> (Put, Get) -> ResourceType -> BaseResource`

---

_Closed by @AlexWaygood on 2025-09-26 19:11_

---
