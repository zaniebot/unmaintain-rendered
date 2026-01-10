```yaml
number: 17717
title: "[`red-knot`] Allow subclasses of Any to be assignable to Callable types"
type: pull_request
state: merged
author: Kalmaegi
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-any-subclass-callable
created_at: 2025-04-29T17:03:20Z
updated_at: 2025-05-01T08:27:04Z
url: https://github.com/astral-sh/ruff/pull/17717
synced_at: 2026-01-10T18:57:03Z
```

# [`red-knot`] Allow subclasses of Any to be assignable to Callable types

---

_Pull request opened by @Kalmaegi on 2025-04-29 17:03_

## Summary

Fixes #17701. I'm uncertain whether this Python code needs to be added to the test cases. I'd be happy to supplement it if needed.


---

_Review requested from @carljm by @Kalmaegi on 2025-04-29 17:03_

---

_Review requested from @AlexWaygood by @Kalmaegi on 2025-04-29 17:03_

---

_Review requested from @sharkdp by @Kalmaegi on 2025-04-29 17:03_

---

_Review requested from @dcreager by @Kalmaegi on 2025-04-29 17:03_

---

_Comment by @github-actions[bot] on 2025-04-29 17:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
starlette (https://github.com/encode/starlette)
- error[lint:invalid-argument-type] tests/test_templates.py:191:39: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Unknown | MagicMock`
- error[lint:invalid-argument-type] tests/test_templates.py:267:39: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Unknown | MagicMock`
- error[lint:invalid-argument-type] tests/test_templates.py:388:32: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Unknown | MagicMock`
- error[lint:invalid-argument-type] tests/test_templates.py:420:39: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Unknown | MagicMock`
- Found 181 diagnostics
+ Found 177 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[lint:invalid-assignment] tests/test_messagequeue.py:238:9: Object of type `Mock` is not assignable to attribute `_connect_no_keepalive` of type `(...) -> Unknown`
- error[lint:invalid-assignment] tests/test_messagequeue.py:239:9: Object of type `Mock` is not assignable to attribute `_connect_and_keepalive` of type `(...) -> Unknown`
- Found 174 diagnostics
+ Found 172 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[lint:invalid-argument-type] tests/ignite/contrib/engines/test_tbptt.py:61:68: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/contrib/engines/test_tbptt.py:63:70: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_create_supervised.py:374:77: Argument to this function is incorrect: Expected `(Any, Any, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_create_supervised.py:410:73: Argument to this function is incorrect: Expected `(Any, Any, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_custom_events.py:89:55: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_custom_events.py:126:72: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:79:25: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:93:25: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:107:25: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:117:25: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:139:25: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:197:25: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:306:25: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:405:25: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:409:60: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Mock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:412:62: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Mock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:431:25: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_engine.py:848:52: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:32:21: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:180:43: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:181:47: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:182:45: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:183:45: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:184:41: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:185:45: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:186:41: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:187:41: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:193:47: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:197:45: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:198:45: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:199:41: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:341:48: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:349:41: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:350:37: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:351:37: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:352:41: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:375:54: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:378:33: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:383:33: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:443:21: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:451:21: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:468:21: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/engine/test_event_handlers.py:478:21: Argument to this function is incorrect: Expected `(Engine, Any, /) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/handlers/test_base_logger.py:238:28: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/handlers/test_base_logger.py:262:32: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/handlers/test_base_logger.py:268:32: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/handlers/test_base_logger.py:289:32: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- error[lint:invalid-argument-type] tests/ignite/handlers/test_base_logger.py:338:32: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MagicMock`
- Found 2461 diagnostics
+ Found 2413 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[lint:invalid-argument-type] comtypes/test/test_inout_args.py:38:33: Argument to this function is incorrect: Expected `(...) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] comtypes/test/test_inout_args.py:83:33: Argument to this function is incorrect: Expected `(...) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] comtypes/test/test_inout_args.py:111:33: Argument to this function is incorrect: Expected `(...) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] comtypes/test/test_inout_args.py:138:33: Argument to this function is incorrect: Expected `(...) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] comtypes/test/test_inout_args.py:283:41: Argument to this function is incorrect: Expected `(...) -> Any`, found `MagicMock`
- error[lint:invalid-argument-type] comtypes/test/test_inout_args.py:491:33: Argument to this function is incorrect: Expected `(...) -> Any`, found `MagicMock`
- Found 705 diagnostics
+ Found 699 diagnostics

optuna (https://github.com/optuna/optuna)
- error[lint:invalid-argument-type] tests/samplers_tests/test_nsgaii.py:663:54: Argument to this function is incorrect: Expected `((...) -> @Todo(specialized non-generic class)) | None`, found `MagicMock`
- Found 2136 diagnostics
+ Found 2135 diagnostics

vision (https://github.com/pytorch/vision)
- error[lint:invalid-argument-type] gallery/transforms/plot_cutmix_mixup.py:44:56: Argument to this function is incorrect: Expected `((...) -> Unknown) | None`, found `Compose`
- Found 2251 diagnostics
+ Found 2250 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:invalid-argument-type] tests/appsec/iast/secure_marks/test_sanitizers.py:33:39: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Mock`
- error[lint:invalid-argument-type] tests/appsec/iast/secure_marks/test_sanitizers.py:58:29: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Mock`
- error[lint:invalid-argument-type] tests/appsec/iast/secure_marks/test_sanitizers.py:82:29: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Mock`
- error[lint:invalid-argument-type] tests/appsec/iast/secure_marks/test_validators.py:29:30: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Mock`
- error[lint:invalid-argument-type] tests/appsec/iast/secure_marks/test_validators.py:53:20: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Mock`
- error[lint:invalid-argument-type] tests/appsec/iast/secure_marks/test_validators.py:77:20: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Mock`
- error[lint:invalid-argument-type] tests/appsec/iast/taint_sinks/test_command_injection.py:233:29: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Mock`
- Found 6776 diagnostics
+ Found 6769 diagnostics

```
</details>


---

_Label `red-knot` added by @AlexWaygood on 2025-04-29 17:10_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-29 17:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1291 on 2025-04-29 20:43_

The typing spec does not specify the behavior of subclassing `Any`, it just says it should be possible. And PEP 484 is a historical document, not an authoritative source (and it also doesn't mention subclassing `Any`). So there is no simple statement from authority that we are following here. Rather, we are following the "core concepts" section of the typing spec, and reasoning from that basis. The `Any` that is inherited can be "materialized" to any type, therefore `C` (where `class C(Any): ...`) is assignable to non-final types, because that `Any` could "materialize" to any non-final type. But it is not assignable to final types, because that `Any` cannot materialize to a final type (it would lead to an invalid inheritance from a final type.) Similar logic applies to literal types; there is no possible type that the `Any` in `class C(Any): ...` could materialize to, which would make an instance of `C` assignable, to, e.g., a `Literal` type.

---

_@carljm requested changes on 2025-04-29 20:45_

Thanks for the PR!

As @sharkdp mentioned on the issue, there should be some exceptions here. One of those exceptions (subclass of `Any` can't be a subclass of a final class) we already have a test for, which is causing the tests to fail on this PR. I think all literal types should also be excluded (an instance of an `Any` subclass can't possibly inhabit `Literal[1]` or any other literal type).

I am not 100% sure that a separate catchall check at the top of the function will still be the clearest implementation here, once we account for all the exceptions. It depends on the balance of how many types we have to carve out of this catchall, vs how many we'd have to add a dedicated check for inheritance-from-Any to.

And yes, Python code samples should be added to the mdtests in this PR demonstrating the cases we want to handle that were not previously handled.

---

_Converted to draft by @Kalmaegi on 2025-04-30 08:18_

---

_@Kalmaegi reviewed on 2025-04-30 08:21_

---

_Review comment by @Kalmaegi on `crates/red_knot_python_semantic/src/types.rs`:1291 on 2025-04-30 08:21_

Thank you very much! I thought we needed to link a rule here XD.

---

_Marked ready for review by @Kalmaegi on 2025-04-30 10:51_

---

_Comment by @Kalmaegi on 2025-04-30 10:58_

> Thanks for the PR!
> 
> As @sharkdp mentioned on the issue, there should be some exceptions here. One of those exceptions (subclass of `Any` can't be a subclass of a final class) we already have a test for, which is causing the tests to fail on this PR. I think all literal types should also be excluded (an instance of an `Any` subclass can't possibly inhabit `Literal[1]` or any other literal type).
> 
> I am not 100% sure that a separate catchall check at the top of the function will still be the clearest implementation here, once we account for all the exceptions. It depends on the balance of how many types we have to carve out of this catchall, vs how many we'd have to add a dedicated check for inheritance-from-Any to.
> 
> And yes, Python code samples should be added to the mdtests in this PR demonstrating the cases we want to handle that were not previously handled.

I checked for existing tests and found that assignments from Any to both Final and Literal are already covered. I just added a test for the Callable case.
I haven’t found other cases that need special handling for now—just these two. Since we’re checking `self ` at the start of the method, I think this placement works reasonably well XD

---

_Comment by @sharkdp on 2025-04-30 13:30_

> I checked for existing tests and found that assignments from Any to both Final and Literal are already covered.

Where do we cover assignments to literals? It looks like it is possibly to assign to a literal on your branch. You will need a test that makes sure that instances of `Subclass(Any)` are not assignable to something like `Literal[1]`:
```py
from typing import Any, Literal

class C(Any):
    pass

x: Literal[1] = C()  # this should fail, but does not on your branch.
```


As @carljm mentioned, it might be better to follow a "opt in" strategy, i.e. to handle special cases to the existing match statement. This way, we make sure that we exhaustively handle all type variants; and that we properly treat union and intersection types. For example:
```py
from typing import final, Any

@final
class F1: ...

@final
class F2: ...

class Subclass(Any): ...

f: F1 | F2 = Subclass()  # this should fail, but does not on your branch.
```

---

_Comment by @Kalmaegi on 2025-04-30 14:23_

> > I checked for existing tests and found that assignments from Any to both Final and Literal are already covered.
> 
> Where do we cover assignments to literals? It looks like it is possibly to assign to a literal on your branch. You will need a test that makes sure that instances of `Subclass(Any)` are not assignable to something like `Literal[1]`:
> 
> ```python
> from typing import Any, Literal
> 
> class C(Any):
>     pass
> 
> x: Literal[1] = C()  # this should fail, but does not on your branch.
> ```
> 
> As @carljm mentioned, it might be better to follow a "opt in" strategy, i.e. to handle special cases to the existing match statement. This way, we make sure that we exhaustively handle all type variants; and that we properly treat union and intersection types. For example:
> 
> ```python
> from typing import final, Any
> 
> @final
> class F1: ...
> 
> @final
> class F2: ...
> 
> class Subclass(Any): ...
> 
> f: F1 | F2 = Subclass()  # this should fail, but does not on your branch.
> ```

Ah, sorry about that—I misread the test. I’ll update the code accordingly.

---

_Converted to draft by @Kalmaegi on 2025-04-30 15:00_

---

_Marked ready for review by @sharkdp on 2025-05-01 08:09_

---

_@sharkdp approved on 2025-05-01 08:11_

@Kalmaegi I didn't really see any evidence in the newly added tests here that we need the catch-all case. In fact, there were some new ecosystem false-positives that looked incorrect (see below). It might be that we're still missing some cases where we should allow assignability, but until then, I'd rather be conservative and only allow assignability to `Callable` specifically. I changed the code to reflect this, hope that's fine for you. These false positives are now gone after the update:
```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ error[lint:invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:374:26: Argument to this function is incorrect: Expected `PytestRuntestProtocolHooks`, found `PytestRuntestProtocolHooks`
- Found 322 diagnostics
+ Found 323 diagnostics

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
+ error[lint:invalid-argument-type] mypy_protobuf/main.py:134:64: Argument to this function is incorrect: Expected `FileDescriptorProto`, found `FileDescriptorProto`
+ error[lint:invalid-argument-type] mypy_protobuf/main.py:135:59: Argument to this function is incorrect: Expected `FileDescriptorProto`, found `FileDescriptorProto`
+ error[lint:invalid-argument-type] mypy_protobuf/main.py:941:22: Argument to this function is incorrect: Expected `FieldDescriptorProto`, found `FieldDescriptorProto`
```

Thank you for working on this.

---

_Renamed from "[`red-knot`] Allow subclasses of Any to be assignable to all types" to "[`red-knot`] Allow subclasses of Any to be assignable to Callable types" by @sharkdp on 2025-05-01 08:18_

---

_Merged by @sharkdp on 2025-05-01 08:18_

---

_Closed by @sharkdp on 2025-05-01 08:18_

---

_Comment by @Kalmaegi on 2025-05-01 08:27_

> @Kalmaegi I didn't really see any evidence in the newly added tests here that we need the catch-all case. In fact, there were some new ecosystem false-positives that looked incorrect (see below). It might be that we're still missing some cases where we should allow assignability, but until then, I'd rather be conservative and only allow assignability to `Callable` specifically. I changed the code to reflect this, hope that's fine for you. These false positives are now gone after the update:
> 
> ```diff
> pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
> + error[lint:invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:374:26: Argument to this function is incorrect: Expected `PytestRuntestProtocolHooks`, found `PytestRuntestProtocolHooks`
> - Found 322 diagnostics
> + Found 323 diagnostics
> 
> mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
> + error[lint:invalid-argument-type] mypy_protobuf/main.py:134:64: Argument to this function is incorrect: Expected `FileDescriptorProto`, found `FileDescriptorProto`
> + error[lint:invalid-argument-type] mypy_protobuf/main.py:135:59: Argument to this function is incorrect: Expected `FileDescriptorProto`, found `FileDescriptorProto`
> + error[lint:invalid-argument-type] mypy_protobuf/main.py:941:22: Argument to this function is incorrect: Expected `FieldDescriptorProto`, found `FieldDescriptorProto`
> ```
> 
> Thank you for working on this.

@sharkdp Thank you so much for your help. I'm still familiarizing myself with the project structure, so some of my ideas might be a bit rudimentary.

---
