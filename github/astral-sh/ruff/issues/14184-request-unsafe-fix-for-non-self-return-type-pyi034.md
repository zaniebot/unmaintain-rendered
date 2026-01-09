---
number: 14184
title: "Request: Unsafe fix for `non-self-return-type`/`PYI034`"
type: issue
state: closed
author: Avasam
labels:
  - fixes
assignees: []
created_at: 2024-11-08T00:59:26Z
updated_at: 2024-11-09T02:11:39Z
url: https://github.com/astral-sh/ruff/issues/14184
synced_at: 2026-01-07T13:12:16-06:00
---

# Request: Unsafe fix for `non-self-return-type`/`PYI034`

---

_Issue opened by @Avasam on 2024-11-08 00:59_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

https://docs.astral.sh/ruff/rules/non-self-return-type/

Very similar to https://github.com/astral-sh/ruff/issues/14183
I feel like logically a fix for this rule should be feasible without too much complexity? (at least in pyi files and maybe py files above 3.10, see https://github.com/astral-sh/ruff/issues/9761). The fix should be unsafe as there might be a specific intent, or a stub shows that the runtime returns an instance not actually based on `self/cls`. Marking it unsafe may also simplify the logic to apply the fix (if `Self` already exists in scope).

Maybe it can be restricted to where the return type to replace is the same as the class and not overloaded ?
```py
class ArrayDerivative(Derivative):
    def __new__(cls, expr, *variables, **kwargs) -> ArrayDerivative: ... # unsafe autofix to replace with self
```
```py
class CaptureOutput:
    @overload
    def __new__(
        cls, backend: Literal[CaptureOutputs.PIL] = CaptureOutputs.PIL
    ) -> PILCaptureOutput: ...
    @overload
    def __new__(cls, backend: Literal[CaptureOutputs.NUMPY]) -> NumpyCaptureOutput: ...
    @overload
    def __new__(cls, backend: Literal[CaptureOutputs.NUMPY_FLOAT]) -> NumpyFloatCaptureOutput: ...
    @overload
    def __new__(cls, backend: Literal[CaptureOutputs.PYTORCH]) -> PytorchCaptureOutput: ...
    @overload
    def __new__(
        cls, backend: Literal[CaptureOutputs.PYTORCH_FLOAT]
    ) -> PytorchFloatCaptureOutput: ...
    @overload
    def __new__(cls, backend: Literal[CaptureOutputs.PYTORCH_GPU]) -> PytorchGPUCaptureOutput: ...
    @overload
    def __new__(
        cls, backend: Literal[CaptureOutputs.PYTORCH_FLOAT_GPU]
    ) -> PytorchFloatGPUCaptureOutput: ...
    @overload
    def __new__(cls, backend: CaptureOutputs) -> CaptureOutput: ...
    def __new__(cls, backend: CaptureOutputs = CaptureOutputs.PIL) -> CaptureOutput:  # no autofix
        return cls._initialize_backend(backend)
```



---

_Referenced in [microsoft/python-type-stubs#340](../../microsoft/python-type-stubs/pulls/340.md) on 2024-11-08 03:11_

---

_Label `fixes` added by @dhruvmanila on 2024-11-08 08:16_

---

_Referenced in [astral-sh/ruff#14217](../../astral-sh/ruff/pulls/14217.md) on 2024-11-09 00:03_

---

_Closed by @charliermarsh on 2024-11-09 02:11_

---
