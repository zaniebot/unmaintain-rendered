```yaml
number: 22407
title: "[ty] Fix classmethod + contextmanager + Self"
type: pull_request
state: merged
author: eclbg
labels:
  - ty
assignees: []
merged: true
base: main
head: classmethod-contextmanager-self-fix
created_at: 2026-01-05T19:01:18Z
updated_at: 2026-01-13T14:47:54Z
url: https://github.com/astral-sh/ruff/pull/22407
synced_at: 2026-01-13T15:29:24Z
```

# [ty] Fix classmethod + contextmanager + Self

---

_@eclbg_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

The test I've added illustrates the fix. Copying it here too:

```python
from contextlib import contextmanager
from typing import Iterator
from typing_extensions import Self

class Base:
    @classmethod
    @contextmanager
    def create(cls) -> Iterator[Self]:
        yield cls()

class Child(Base): ...

with Base.create() as base:
    reveal_type(base)  # revealed: Base (after the fix, None before)

with Child.create() as child:
    reveal_type(child)  # revealed: Child (after the fix, None before)
```

Full disclosure: I've used LLMs for this PR, but the result is thoroughly reviewed by me before submitting. I'm excited about my first Rust contribution to Astral tools and will address feedback quickly.

Related to https://github.com/astral-sh/ty/issues/2030, I am working on a fix for the TypeVar case also reported in that issue (by me)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

Updated mdtests


---

_Comment by @astral-sh-bot[bot] on 2026-01-05 19:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-05 19:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `None | Coroutine[Any, Any, None] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `AwsCredentials | Coroutine[Any, Any, AwsCredentials]`
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `None | Coroutine[Any, Any, None] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `AwsCredentials | Coroutine[Any, Any, AwsCredentials]`
- src/integrations/prefect-aws/tests/test_s3.py:1141:24: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `credentials`
+ src/integrations/prefect-aws/tests/test_s3.py:1141:24: warning[possibly-missing-attribute] Attribute `credentials` may be missing on object of type `S3Bucket | Coroutine[Any, Any, S3Bucket]`
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:28:19: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
+ src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:28:19: error[invalid-await] `AzureBlobStorageCredentials | Coroutine[Any, Any, AzureBlobStorageCredentials]` is not awaitable
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:38:19: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
+ src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:38:19: error[invalid-await] `AzureBlobStorageCredentials | Coroutine[Any, Any, AzureBlobStorageCredentials]` is not awaitable
- src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:36: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
+ src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:36: error[invalid-await] `DbtCliProfile | Coroutine[Any, Any, DbtCliProfile]` is not awaitable
+ src/integrations/prefect-email/tests/test_credentials.py:91:12: warning[possibly-missing-attribute] Attribute `smtp_type` may be missing on object of type `EmailServerCredentials | Coroutine[Any, Any, EmailServerCredentials]`
+ src/integrations/prefect-email/tests/test_credentials.py:92:14: warning[possibly-missing-attribute] Attribute `get_server` may be missing on object of type `EmailServerCredentials | Coroutine[Any, Any, EmailServerCredentials]`
- src/integrations/prefect-email/tests/test_credentials.py:91:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `smtp_type`
+ src/integrations/prefect-email/tests/test_credentials.py:93:12: error[unresolved-attribute] Object of type `SMTP` has no attribute `port`
- src/integrations/prefect-email/tests/test_credentials.py:92:14: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `get_server`
- src/integrations/prefect-email/tests/test_credentials.py:108:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `smtp_type`
+ src/integrations/prefect-email/tests/test_credentials.py:108:12: warning[possibly-missing-attribute] Attribute `smtp_type` may be missing on object of type `EmailServerCredentials | Coroutine[Any, Any, EmailServerCredentials]`
+ src/integrations/prefect-email/tests/test_credentials.py:109:12: warning[possibly-missing-attribute] Attribute `verify` may be missing on object of type `EmailServerCredentials | Coroutine[Any, Any, EmailServerCredentials]`
+ src/integrations/prefect-email/tests/test_credentials.py:110:14: warning[possibly-missing-attribute] Attribute `get_server` may be missing on object of type `EmailServerCredentials | Coroutine[Any, Any, EmailServerCredentials]`
- src/integrations/prefect-email/tests/test_credentials.py:109:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `verify`
+ src/integrations/prefect-email/tests/test_credentials.py:111:12: error[unresolved-attribute] Object of type `SMTP` has no attribute `port`
- src/integrations/prefect-email/tests/test_credentials.py:110:14: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `get_server`
- src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[invalid-context-manager] Object of type `None | Coroutine[Any, Any, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[invalid-context-manager] Object of type `SqlAlchemyConnector | Coroutine[Any, Any, SqlAlchemyConnector]` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly missing
- src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[invalid-context-manager] Object of type `None | Coroutine[Any, Any, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[invalid-context-manager] Object of type `SqlAlchemyConnector | Coroutine[Any, Any, SqlAlchemyConnector]` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly missing
- src/prefect/cli/deployment.py:292:49: error[unresolved-attribute] Object of type `None` has no attribute `model_dump`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/pull.py:288:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReadableDeploymentStorage | WritableDeploymentStorage`, found `None`
+ src/prefect/deployments/steps/pull.py:288:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReadableDeploymentStorage | WritableDeploymentStorage`, found `Block`
- src/prefect/flow_engine.py:696:32: error[unresolved-attribute] Object of type `None` has no attribute `client`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1278:32: error[unresolved-attribute] Object of type `None` has no attribute `client`
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/task_engine.py:821:32: error[unresolved-attribute] Object of type `None` has no attribute `client`
- src/prefect/testing/utilities.py:280:17: error[invalid-argument-type] Argument to function `assert_blocks_equal` is incorrect: Expected `Block`, found `ReadableFileSystem | None`
- src/prefect/testing/utilities.py:291:17: error[invalid-argument-type] Argument to function `assert_blocks_equal` is incorrect: Expected `Block`, found `ReadableFileSystem | None`
- Found 5367 diagnostics
+ Found 5368 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`


```

</details>


No memory usage changes detected ✅



---

_@eclbg reviewed on 2026-01-05 19:27_

---

_Review comment by @eclbg on `crates/ty_python_semantic/src/types.rs`:2603 on 2026-01-05 19:27_

I feel this is somewhat redundant with the change in `bind_self` in `signatures.rs`. In both places I'm detecting whether the method is a classmethod in different ways. Open to alternatives, of course.

---

_Marked ready for review by @eclbg on 2026-01-05 19:31_

---

_Review requested from @carljm by @eclbg on 2026-01-05 19:31_

---

_Review requested from @AlexWaygood by @eclbg on 2026-01-05 19:31_

---

_Review requested from @sharkdp by @eclbg on 2026-01-05 19:31_

---

_Review requested from @dcreager by @eclbg on 2026-01-05 19:31_

---

_Renamed from "Fix classmethod + contextmanager + Self" to "[ty] Fix classmethod + contextmanager + Self" by @eclbg on 2026-01-05 19:35_

---

_Label `ty` added by @MichaReiser on 2026-01-06 09:04_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2607 on 2026-01-08 19:24_

If you change this to `instance`, I think you can remove all of the changes in `signatures.rs`. In other words, `bind_self` should be given the type that `Self` should specialize to, not the type of the first parameter.

Doing that, both `else` branches will then be the same, and so you should be able to simplify this overall to

```rust
let self_type = if callable.is_classmethod_like(db) && instance.is_none(db) {
    owner
} else {
    instance
};
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2603 on 2026-01-08 19:24_

I agree, see below

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/call/methods.md`:485 on 2026-01-08 19:26_

Can you also add a couple of additional `reveal_type`s? Doing that in the playground is really what helped me figure out what's happening here:

```py
reveal_type(Base.create)
reveal_type(Base().create)

reveal_type(Child.create)
reveal_type(Child().create)
```

(In particular, doing the member access on both the class and on an instance of the class showed how we were only getting the wrong behavior before this PR for the class access case. The helped zero in on the `None` descriptor protocol argument being the root cause.)

---

_@dcreager reviewed on 2026-01-08 19:27_

---

_@eclbg reviewed on 2026-01-08 21:22_

---

_Review comment by @eclbg on `crates/ty_python_semantic/resources/mdtest/call/methods.md`:485 on 2026-01-08 21:22_

Thanks a lot for such a detailed review. I'll make the changes tomorrow.

---

_Converted to draft by @eclbg on 2026-01-11 09:11_

---

_@eclbg reviewed on 2026-01-12 19:02_

---

_Review comment by @eclbg on `crates/ty_python_semantic/resources/mdtest/call/methods.md`:485 on 2026-01-12 19:02_

I've added a few more `reveal_type`s

---

_@eclbg reviewed on 2026-01-12 19:18_

---

_Review comment by @eclbg on `crates/ty_python_semantic/src/types.rs`:2607 on 2026-01-12 19:18_

I've been playing around with this and couldn't get the new test to pass without the changes in `signatures.rs`, even with the change to `types.rs` suggested in your comment.  

Without those, the return type is inferred as `<class 'Base'>` or `<class 'Child'>`. This makes sense to me because `owner` is a `SubclassOf(SubclassOfType{subclass_of: Class ...`, and I can see why we would need to turn that into the instance type for classmethod-like callables.  

Did I understand your suggestion correctly?

I've pushed the change to `types.rs` and left the original code in `signatures.rs` to illustrate this. The tests are green like this, and this is how the revealed types mismatch if I remove the changes in `signatures.rs`: 

```
crates/ty_python_semantic/resources/mdtest/call/methods.md:486 unmatched assertion: revealed: _GeneratorContextManager[Base, None, None]
crates/ty_python_semantic/resources/mdtest/call/methods.md:486 unexpected error: 13 [revealed-type] "Revealed type: `_GeneratorContextManager[<class 'Base'>, None, None]`"
crates/ty_python_semantic/resources/mdtest/call/methods.md:488 unmatched assertion: revealed: Base
crates/ty_python_semantic/resources/mdtest/call/methods.md:488 unexpected error: 17 [revealed-type] "Revealed type: `<class 'Base'>`"
crates/ty_python_semantic/resources/mdtest/call/methods.md:494 unmatched assertion: revealed: _GeneratorContextManager[Child, None, None]
crates/ty_python_semantic/resources/mdtest/call/methods.md:494 unexpected error: 13 [revealed-type] "Revealed type: `_GeneratorContextManager[<class 'Child'>, None, None]`"
crates/ty_python_semantic/resources/mdtest/call/methods.md:496 unmatched assertion: revealed: Child
crates/ty_python_semantic/resources/mdtest/call/methods.md:496 unexpected error: 17 [revealed-type] "Revealed type: `<class 'Child'>`"
```


---

_Marked ready for review by @eclbg on 2026-01-12 19:18_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-13 08:34_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2026-01-13 13:44_

---

_@dcreager reviewed on 2026-01-13 14:12_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2607 on 2026-01-13 14:12_

Good catch! So the `bind_self` needs to take in the type that `Self` specializes to, but the `owner` parameter when invoking the descriptor protocol is the class type itself (so an instance of `type[Self]`). That means we do need the `to_instance` call to get from the type of `owner` to the type of `Self`. I've moved the call from `signatures.rs` to `try_call_dunder_get`.

---

_@dcreager approved on 2026-01-13 14:15_

---

_Merged by @dcreager on 2026-01-13 14:29_

---

_Closed by @dcreager on 2026-01-13 14:29_

---

_@eclbg reviewed on 2026-01-13 14:47_

---

_Review comment by @eclbg on `crates/ty_python_semantic/src/types.rs`:2607 on 2026-01-13 14:47_

Thanks! Much better :)

---

_Comment by @eclbg on 2026-01-13 14:47_

Thanks for the review and for getting this merged!

---
