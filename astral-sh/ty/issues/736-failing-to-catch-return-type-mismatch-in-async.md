```yaml
number: 736
title: Failing to catch return type mismatch in async functions only
type: issue
state: closed
author: rian-dolphin
labels: []
assignees: []
created_at: 2025-07-01T08:37:39Z
updated_at: 2025-07-30T09:51:22Z
url: https://github.com/astral-sh/ty/issues/736
synced_at: 2026-01-12T15:54:23Z
```

# Failing to catch return type mismatch in async functions only

---

_@rian-dolphin_

### Summary

ty correctly identifies return type mismatches in synchronous functions but doesn't catch the same error in async functions.

## Minimal Reproducible Example

### Sync version (works correctly)
```python
# sync_test.py
class ExpectedOutput:
    def __init__(self, data: str):
        self.data = data

class ResultWrapper:
    def __init__(self, output: ExpectedOutput):
        self.output = output

def get_result() -> ResultWrapper:
    return ResultWrapper(ExpectedOutput("test data"))

def should_return_expected_output() -> ExpectedOutput:
    """Should return ExpectedOutput but returns ResultWrapper"""
    result = get_result()
    return result  # Type error: should be result.output
```

**Result**: `uvx ty check sync_test.py` correctly reports:
```
error[invalid-return-type]: Return type does not match returned value
  --> sync_test.py:15:40
   |
15 | def should_return_expected_output() -> ExpectedOutput:
   |                                        -------------- Expected `ExpectedOutput` because of return type
16 |     """Should return ExpectedOutput but returns ResultWrapper"""
17 |     result = get_result()
18 |     return result  # Type error: should be result.output
   |            ^^^^^^ expected `ExpectedOutput`, found `ResultWrapper`
   |
info: rule `invalid-return-type` is enabled by default
```

### Async version (fails to catch error)
```python
# async_test.py
class ExpectedOutput:
    def __init__(self, data: str):
        self.data = data

class ResultWrapper:
    def __init__(self, output: ExpectedOutput):
        self.output = output

async def get_result() -> ResultWrapper:
    return ResultWrapper(ExpectedOutput("test data"))

async def should_return_expected_output() -> ExpectedOutput:
    """Should return ExpectedOutput but returns ResultWrapper"""
    result = await get_result()
    return result  # Type error: should be result.output
```

**Result**: `uvx ty check async_test.py` incorrectly reports:
```
All checks passed!
```

## Environment

- ty version: Latest from `uvx ty` (ty 0.0.1-alpha.12 (f7446a6ee 2025-06-25))
- Platform: macOS

## Impact

I noticed this when using Pydantic AI `agent.run()`

### Version

ty 0.0.1-alpha.12 (f7446a6ee 2025-06-25)

---

_Comment by @sharkdp on 2025-07-02 06:47_

Thank you for reporting this. The problem is not that we don't catch return type mismatches in async functions, but rather that the inferred type for `result = await …` here is a dynamic type (a `@Todo` type that represents the fact that we don't support something yet, namely `await` in this case) which is assignable to everything.

This will be solved once we have support for async/await.

---

_Closed by @sharkdp on 2025-07-02 06:47_

---

_Closed by @sharkdp on 2025-07-30 09:51_

---
