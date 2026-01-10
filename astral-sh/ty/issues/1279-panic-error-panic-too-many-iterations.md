```yaml
number: 1279
title: "[panic] error[panic]: too many iterations"
type: issue
state: closed
author: jasminemoore-verses
labels: []
assignees: []
created_at: 2025-09-29T16:02:23Z
updated_at: 2025-09-29T17:37:10Z
url: https://github.com/astral-sh/ty/issues/1279
synced_at: 2026-01-10T02:06:25Z
```

# [panic] error[panic]: too many iterations

---

_Issue opened by @jasminemoore-verses on 2025-09-29 16:02_

Error:
```shell
Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/Users/jasmine/Code/genius-engines/packages/genius-pymdp-engine/src/genius_pymdp_engine/pymdp_mapper.py`: `infer_definition_types(Id(19235)): execute: too many cycle iterations`
```

Source file:
```python
# -*- coding: utf-8 -*-
"""
mapper.py: Core module for handling POMDP-related requests using PYMDP

This module provides the Mapper class, which serves as an interface between
the API requests and the PYMDP model operations. It handles the creation of
POMDP models, running simulations, and generating visualizations.

Key components:
- PymdpMapper: Main class for handling POMDP operations and requests

The module integrates with the POMDP_Model from utils.py to perform the actual
POMDP computations and simulations.
"""

from __future__ import annotations
from typing import List
from overrides import override

from pyvfg.versions.v_0_5_0.vfg_0_5_0 import VFG

from genius_engines_api.request_response import (
    InferenceRequest,
    InferenceResponse,
    InferenceResult,
    LearnHistory,
)
from genius_engines_common_components.mapper import Mapper
from genius_engines_common_components.metrics import (
    create_response_metadata,
    start_tracing_metrics,
    stop_tracing_metrics,
)
from genius_pymdp_engine.pomdp_model import PomdpModel
from genius_pymdp_engine.utils import sort_pomdp_factors


class PymdpMapper(PomdpModel, Mapper):
    """
    Mapper class for handling POMDP-related requests using PYMDP.

    This class processes requests for various POMDP operations including
    model building, environment creation, and simulation. It serves as the main
    interface between the API and the POMDP model implementation.
    """

    def __init__(
        self,
        vfg: VFG,
        policy_len: int = 1,
        use_utility: bool = True,
        use_infogain: bool = True,
        learn_A: bool = False,
        learn_B: bool = False,
        learn_D: bool = False,
        learning_rate: float = 1.0,
    ):
        """
        Initialize the PyMDP mapper.
        """
        super().__init__(
            policy_len=policy_len,
            use_utility=use_utility,
            use_states_info_gain=use_infogain,
            learn_A=learn_A,
            learn_B=learn_B,
            learn_D=learn_D,
            learning_rate=learning_rate,
        )
        self.create_model(sort_pomdp_factors(vfg))

    @override
    def process_inference_request(self, request: InferenceRequest) -> InferenceResponse:
        """Process an action selection request."""

        metrics_start = start_tracing_metrics()

        # Use the existing POMDP model instance
        result, warnings = self.perform_inference(request)

        end = stop_tracing_metrics(metrics_start["tracing_id"])

        # Update internal VFG state but keep the same POMDP model instance
        if result.updated_vfg and request.save is True:
            self.vfg = result.updated_vfg

        history: List[LearnHistory] = []
        for name, data in result.action_data.items():
            history.append(LearnHistory(metrics=data.to_metrics_dict()))

        inference_result = InferenceResult(
            history=history,
            returned_variables=None,
            result_model=self.vfg.deepcopy(),
        )

        return InferenceResponse(
            success=True,
            error=None,
            result=inference_result,
            metadata=create_response_metadata(metrics_start, end),
            warnings=warnings,
        )

```

---

_Comment by @C0rn3j on 2025-09-29 16:22_

Tauon's [src/tauon/main.py](https://github.com/taiko2k/tauon) codebase (which is a hardcore test suite if anything) is also crashing on a panic at the moment, against current `ty` master.

```
[Error - 18:27:16] Request textDocument/inlayHint failed.
  Message: request handler panicked at /home/user/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25:
exported_names(Id(186f4)): execute: too many cycle iterationsquery stacktrace:
   0: DatabaseKeyIndex(IngredientIndex(96), Id(1bf6))
   1: DatabaseKeyIndex(IngredientIndex(92), Id(1bf6))
   2: DatabaseKeyIndex(IngredientIndex(108), Id(12504))
   3: DatabaseKeyIndex(IngredientIndex(141), Id(e87b))
   4: DatabaseKeyIndex(IngredientIndex(145), Id(2000))


run with `RUST_BACKTRACE=1` environment variable to display a backtrace

  Code: -32603 
```

---

_Closed by @carljm on 2025-09-29 17:34_

---

_Reopened by @carljm on 2025-09-29 17:34_

---

_Closed by @carljm on 2025-09-29 17:34_

---

_Comment by @AlexWaygood on 2025-09-29 17:36_

(@C0rn3j, your panic looks like a different problem from the one originally reported in this issue -- it looks like a duplicate of #444 rather than #256)

---
