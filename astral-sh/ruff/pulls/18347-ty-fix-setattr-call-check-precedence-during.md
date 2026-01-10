```yaml
number: 18347
title: "[ty] Fix __setattr__ call check precedence during attribute assignment"
type: pull_request
state: merged
author: thejchap
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: feature/setattr-return-type
created_at: 2025-05-28T06:02:47Z
updated_at: 2025-07-09T02:20:27Z
url: https://github.com/astral-sh/ruff/pull/18347
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Fix __setattr__ call check precedence during attribute assignment

---

_Pull request opened by @thejchap on 2025-05-28 06:02_

## Summary
Related:
- https://github.com/astral-sh/ty/issues/111
- https://github.com/astral-sh/ruff/pull/17974#discussion_r2108527106

Previously, when validating an attribute assignment, a `__setattr__` call check was only done if the attribute wasn't found as either a class member or instance member

This PR changes the `__setattr__` call check to be attempted first, prior to the "[normal mechanism](https://docs.python.org/3/reference/datamodel.html#object.__setattr__)", as a defined `__setattr__` should take precedence over setting an attribute on the instance dictionary directly.

if the return type of `__setattr__` is `Never`, an `invalid-assignment` diagnostic is emitted

Once this is merged, a subsequent PR will synthesize a `__setattr__` method with a `Never` return type for frozen dataclasses.

### Ecosystem changes
it looks like support needs to be added for [writing special attributes](https://docs.python.org/3/reference/datamodel.html#special-writable-attributes) on `Callable`s (at least I wasn't able to find tests that asserted these are supported). These diagnostics are getting emitted due to a `CallDunderError::PossiblyUnbound`.

This seems like an expected side-effect given the change in this PR - would it make sense to add a separate issue to track this?

> Function objects also support getting and setting arbitrary attributes,

```
+ error[unresolved-attribute] yarl/_url.py:140:5: Can not assign object of `Literal["yarl"]` to attribute `__module__` on type `_T` with custom `__setattr__` method.
+ error[unresolved-attribute] optuna/_deprecated.py:185:17: Can not assign object of `Literal[""]` to attribute `__doc__` on type `CT` with custom `__setattr__` method.
+ error[unresolved-attribute] optuna/_deprecated.py:193:13: Can not assign object of `str` to attribute `__doc__` on type `CT` with custom `__setattr__` method.
+ error[unresolved-attribute] optuna/_experimental.py:129:17: Can not assign object of `Literal[""]` to attribute `__doc__` on type `CT` with custom `__setattr__` method.
+ error[unresolved-attribute] optuna/_experimental.py:133:13: Can not assign object of `str` to attribute `__doc__` on type `CT` with custom `__setattr__` method.
```

it's not obvious to me that the other ecosystem changes are related to this PR - would need to understand better how the comparison is happening

## Test Plan
Existing tests + mypy_primer

---

_Comment by @github-actions[bot] on 2025-05-28 06:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- warning[possibly-unbound-attribute] beartype/_util/api/external/utilclick.py:108:5: Attribute `callback` on type `BeartypeableT` is possibly unbound
- Found 557 diagnostics
+ Found 556 diagnostics

comtypes (https://github.com/enthought/comtypes)
+ warning[unused-ignore-comment] comtypes/_post_coinit/misc.py:377:46: Unused blanket `type: ignore` directive
- Found 472 diagnostics
+ Found 473 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ error[invalid-assignment] src/werkzeug/sansio/response.py:597:9: Object of type `def on_update(value: WWWAuthenticate) -> None` is not assignable to attribute `_on_update` on type `(Unknown & ~None) | WWWAuthenticate`
- Found 385 diagnostics
+ Found 386 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[invalid-assignment] tests/test_extension_telnet.py:16:9: Implicit shadowing of function `_get_telnet_vars`
- Found 1100 diagnostics
+ Found 1099 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ error[unresolved-attribute] tests/test_config.py:70:5: Can not assign object of type `float` to attribute `chop_threshold` on type `Display` with custom `__setattr__` method.
- Found 2769 diagnostics
+ Found 2770 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[invalid-assignment] sphinx/builders/__init__.py:698:9: Object of type `None` is not assignable to attribute `record_dependencies` of type `DependencyList`
- Found 607 diagnostics
+ Found 606 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[invalid-assignment] test/mitmproxy/addons/test_next_layer.py:374:17: Object of type `tuple[Literal["2001:db8::1"], Literal[443], Literal[0], Literal[0]] | tuple[Literal["192.0.2.1"], Literal[443]]` is not assignable to attribute `peername` of type `tuple[str, int] | None`
- error[invalid-assignment] test/mitmproxy/addons/test_tlsconfig.py:439:17: Object of type `None` is not assignable to attribute `alpn_offers` on type `Unknown | Server`
- Found 1804 diagnostics
+ Found 1802 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- error[invalid-assignment] lib/tests/streamlit/runtime/scriptrunner/script_runner_test.py:233:9: Implicit shadowing of function `_run_script`
- Found 3301 diagnostics
+ Found 3300 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-assignment] tests/tracer/test_pin.py:76:13: Invalid assignment to data descriptor attribute `service` on type `Pin` with custom `__set__` method
- Found 6845 diagnostics
+ Found 6844 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@thejchap reviewed on 2025-05-28 06:07_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/infer.rs`:3362 on 2025-05-28 06:07_

one issue here is `Never` got [introduced in 3.11](https://docs.python.org/3/library/typing.html#typing.Never)

maybe match `NoReturn` as well? (3.6)

---

_@thejchap reviewed on 2025-05-28 06:11_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/infer.rs`:3301 on 2025-05-28 06:11_

TODO figure out precedence of the setattr diagnostics vs the rest

---

_Label `ty` added by @MichaReiser on 2025-05-28 06:18_

---

_@sharkdp reviewed on 2025-05-28 08:14_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:3362 on 2025-05-28 08:14_

You don't need to worry about that here. `Type::Never` is a version-independent representation of the bottom type (in Rust), and it includes both `typing.Never` and `typing.NoReturn`. 

---

_Renamed from "[ty] Unconditional __setattr__ call check during attribute assignment" to "[ty] Fix __setattr__ call check precedence during attribute assignment" by @thejchap on 2025-05-31 01:51_

---

_Comment by @thejchap on 2025-05-31 01:59_

@sharkdp this is ready for a look, left a few notes in the description

---

_Marked ready for review by @thejchap on 2025-05-31 01:59_

---

_Review requested from @carljm by @thejchap on 2025-05-31 01:59_

---

_Review requested from @AlexWaygood by @thejchap on 2025-05-31 01:59_

---

_Review requested from @dcreager by @thejchap on 2025-05-31 01:59_

---

_Comment by @thejchap on 2025-06-01 05:52_

re: this comment/example from #17974:

> So it seems reasonable to me to check the __setattr__ call for both of these assignments. And because we see Never as a return type, emit a diagnostic for both of these assignments (which seems like the user intent).

```py
from typing import Never


class Frozen:
    existing: int = 1

    def __setattr__(self, name, value) -> Never:
        raise AttributeError("Attributes can not be modified")


instance = Frozen()
instance.non_existing = 2  # fails at runtime
instance.existing = 2  # also fails at runtime
```

i think this would be different if it were actually a frozen dataclass - in that case it should be:

```py
from typing import Never
from dataclasses import dataclass


@dataclass(frozen=True)
class Frozen:
    existing: int = 1

instance = Frozen()
instance.non_existing = 2  # unresolved attribute
instance.existing = 2  # invalid assignment
```

or at least that's how mypy handles it

if that's the case, then this first implementation won't work - we'd get back an invalid-assignment for `instance.non_existing = 2` instead of `unresolved-attribute`

seems like the logic would need to be something like:
- user-defined/overridden `__setattr__`?
  - frozen dataclass?
    - emit some diagnostic as this isn't allowed
  - else
    - attempt `__setattr__` call
- else
  - found as instance member?
    - attempt `__setattr__` call
  - else
    - `unresolved-attribute`

---

_Comment by @sharkdp on 2025-06-03 14:21_

Thank you for working on this!

I think that we need to understand the ecosystem changes more closely and write some tests here. While taking a look at the `yarl` changes in particular, I found a bug which I fixed in https://github.com/astral-sh/ruff/pull/18439. I rebased your branch on top of that (so that we can see the updated ecosystem results), and also added an initial set of tests. I also took the liberty to rewrite the metadata in your first commit, as it seemed completely off, potentially from a rebase/squash (previous state: https://github.com/astral-sh/ruff/commit/22620d5c7ba79efcd5d2a8ac6faff040469f7c61).

---

_@sharkdp reviewed on 2025-06-03 16:10_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:3251 on 2025-06-03 16:10_

I think this might be causing the changes in `beartype`, which look surprising to me. Do we need to change this branch, now that we call `__setattr__` unconditionally? We should probably add some tests with unions of classes that do/don't have a `__setattr__` method.

---

_Comment by @thejchap on 2025-06-04 22:42_

@sharkdp 

> I think that we need to understand the ecosystem changes more closely and write some tests here ... I rebased your branch on top of that

sounds good! thanks - i will investigate the remaining changes after your rebase

> it seemed completely off, potentially from a rebase/squash

🤦‍♂️ thanks - was a little hasty there - appreciate that...

---

_@thejchap reviewed on 2025-06-04 22:49_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/infer.rs`:3251 on 2025-06-04 22:49_

i will add some tests with unions and look into this a little more - but also, did this [comment/concern](https://github.com/astral-sh/ruff/pull/18347#issuecomment-2926600351) make sense at all? i just want to confirm that this is the right approach generally (doing the unconditional call)

doing the `__setattr__` call on every attribute assignment would result in (i think) the wrong diagnostics in this example:

```py
from typing import Never
from dataclasses import dataclass


@dataclass(frozen=True)
class Frozen:
    existing: int = 1

instance = Frozen()
instance.non_existing = 2  # would expect unresolved attribute, but unconditional call would result in invalid assignment
instance.existing = 2  # invalid assignment as expected
```

---

_@sharkdp reviewed on 2025-06-05 07:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:3251 on 2025-06-05 07:43_

Yes, I think it's good to bring this up, but I think we can handle this.

For an arbitrary class with a `__setattr__` method, we don't know what the body of that custom `__setattr__` method does. So I think it's fine to unconditionally call it, see if it returns `Never` (there could be multiple overloads and maybe it only returns `Never` conditionally — it would be nice to add a test for this), and show the invalid-assignment diagnostic if that's the case.

On the other hand, for synthesized `__setattr__` methods of frozen dataclasses, we understand how the body looks like. It returns an `AttributeError` if the attribute doesn't exist, and it returns a `FrozenInstanceError` if it's an existing attribute. It seems reasonable to emit "unresolved attribute" for the first case, and invalid-assignment (potentially with a customized error message for frozen instances) in the second case.

---

_Comment by @sharkdp on 2025-06-06 12:19_

> sounds good! thanks - i will investigate the remaining changes after your rebase

Please let me know if you need help with this.

---

_Converted to draft by @thejchap on 2025-06-11 23:30_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-16 22:36_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-08 11:34_

---

_Marked ready for review by @sharkdp on 2025-07-08 13:32_

---

_@sharkdp approved on 2025-07-08 13:33_

Sorry that this took so long. I brought this up to date, made a few modifications and added some new tests. I also reviewed the ecosystem changes and except for the `beartype` thing which I don't fully understand, I think the changes are good.

Thank you very much!

---

_Merged by @sharkdp on 2025-07-08 13:34_

---

_Closed by @sharkdp on 2025-07-08 13:34_

---

_Comment by @thejchap on 2025-07-08 16:07_

@sharkdp thank you!

---

_Comment by @sharkdp on 2025-07-08 18:40_

> @sharkdp thank you!

Of course, thank you for your contribution. Are you interested in following this up with a modified version of https://github.com/thejchap/ruff/pull/2/files? If not, that's also completely fine.

---

_Comment by @thejchap on 2025-07-09 02:20_

@sharkdp yep, I'll revisit that

---
