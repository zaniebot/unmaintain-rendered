```yaml
number: 400
title: "Bug: Type Checker Error: \"no-matching-overload\" for dataclasses.asdict with keyword argument unpacking"
type: issue
state: closed
author: jonathanreichhealthscope
labels:
  - bug
  - dataclasses
assignees: []
created_at: 2025-05-15T06:52:31Z
updated_at: 2025-05-15T12:27:24Z
url: https://github.com/astral-sh/ty/issues/400
synced_at: 2026-01-12T15:54:23Z
```

# Bug: Type Checker Error: "no-matching-overload" for dataclasses.asdict with keyword argument unpacking

---

_@jonathanreichhealthscope_

## Summary

The type checker incorrectly reports a "no-matching-overload" error for `dataclasses.asdict` when its result is used for keyword argument unpacking (`**asdict(params)`) during the instantiation of a class, even when the `params` object is a valid dataclass instance and `asdict` is imported correctly.

## Steps to Reproduce

An MVE:

```python
import logging
from dataclasses import asdict, dataclass

class DummyModel:
    def __init__(self, model_path: str, verbose: bool = False, **kwargs):
        self.model_path = model_path
        self.verbose = verbose
        self.other_params = kwargs

@dataclass
class ModelParams:
    max_tokens: int = 10
    n_gpu_layers: int = -1
    temperature: float = 0.0

def init_dummy_model(model_path: str, **kwargs):
    params = ModelParams(**kwargs)

    try:
        # The following line triggers the "no-matching-overload" error
        # when checked with ty/Ruff
        model_instance = DummyModel(model_path=model_path, verbose=True, **asdict(params))
        print("Model initialized successfully.")
    except Exception as e:
        print(f"Error during model initialization: {e}")
        raise

if __name__ == "__main__":
    dummy_path = "dummy_model.gguf"
    with open(dummy_path, "w") as f:
        f.write("This is a dummy model file.")

    init_dummy_model(model_path=dummy_path, max_tokens=100, temperature=0.7)
```

## Expected Behavior

The type checker should not report any errors, as `asdict(params)` returns a `dict[str, Any]`, which is valid for keyword argument unpacking (`**`) when instantiating `DummyModel` (or `LlamaCpp` in the original context). The `DummyModel` (and `LlamaCpp`) is designed to accept arbitrary keyword arguments.

## Actual Behavior

The type checker reports a `no-matching-overload` error for the `asdict(params)` call on the line where `DummyModel` is instantiated:


### Version

ty 0.0.1-alpha.2

---

_Renamed from "Type Checker Error: "no-matching-overload" for dataclasses.asdict with keyword argument unpacking" to "Bug: Type Checker Error: "no-matching-overload" for dataclasses.asdict with keyword argument unpacking" by @jonathanreichhealthscope on 2025-05-15 07:06_

---

_Label `bug` added by @sharkdp on 2025-05-15 08:26_

---

_Label `dataclasses` added by @sharkdp on 2025-05-15 08:26_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-15 08:28_

---

_Comment by @sharkdp on 2025-05-15 09:03_

Thank you for reporting this!

> which is valid for keyword argument unpacking (`**`)

The issue here is not with argument unpacking (although we also don't support this yet, see #247). The problem is that we didn't recognize that dataclass instances were instances of the `DataclassInstance` protocol (which is used for the parameter type of `dataclasses.asdict`). This should be fixed by https://github.com/astral-sh/ruff/pull/18115.

---

_Closed by @sharkdp on 2025-05-15 12:27_

---
