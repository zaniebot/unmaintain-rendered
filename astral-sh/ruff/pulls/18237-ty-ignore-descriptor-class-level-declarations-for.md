```yaml
number: 18237
title: "[ty] Ignore descriptor class-level declarations for purposes of finding instance attributes"
type: pull_request
state: open
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/fix-350
created_at: 2025-05-21T08:33:11Z
updated_at: 2025-05-23T13:27:40Z
url: https://github.com/astral-sh/ruff/pull/18237
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Ignore descriptor class-level declarations for purposes of finding instance attributes

---

_@sharkdp_

## Summary

The code example that we're fixing here looks similar to this:
```py
class C:
    def f(self) -> None:
        print("original f")

    def replacement(self) -> None:
        print("a replacement")

    def switch(self):
        self.f = self.replacement

c = C()
# call c.switch() or not
c.f()
````

When we invoke the descriptor protocol in the attribute access `c.f` (last line), the following happens:

* We first look up `f` on the meta type `type(c) = C`. We find `C.f` and notice that it is a non-data descriptor (every function is).
* Since non-data descriptors can be overwritten on instances, we now look for instance attributes `f` on instances of `C`.
  * There are implicit attribute assignments (`self.f = self.replacement`)
  * There is **also a binding and a declaration of `f` on the class body** (the function definition)
  * We usually use declared types on the class body as a trusted source for the type of implicit instance attributes (if we see any assignments in methods)
  * However, if we do that here, we would return a function literal type `Literal[C.f]` (and `__get__` is not called on instance attributes!)

So in this change-set, we adapt the logic to *not trust* the declared type on the class body if it is a (non-data) descriptor. This seems reasonable, because if there are any attribute assignments, they would overwrite that class-level non-data descriptor. 

closes https://github.com/astral-sh/ty/issues/350

## Test Plan

* New Markdown tests
* Regression test

---

_Label `ty` added by @sharkdp on 2025-05-21 08:33_

---

_Comment by @github-actions[bot] on 2025-05-21 08:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:584:5: Attribute `update` on type `BeartypeForwardScope | None` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:584:5: Attribute `update` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:585:5: Attribute `update` on type `BeartypeForwardScope | None` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:585:5: Attribute `update` on type `Unknown | None` is possibly unbound

graphql-core (https://github.com/graphql-python/graphql-core)
- error[invalid-argument-type] src/graphql/utilities/value_from_ast.py:138:58: Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[str, Any] & ~AlwaysFalsy`
- error[missing-argument] src/graphql/utilities/value_from_ast.py:140:26: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] src/graphql/validation/rules/values_of_correct_type.py:162:28: No argument provided for required parameter `node` of function `parse_literal`
+ warning[unused-ignore-comment] tests/type/test_definition.py:138:44: Unused blanket `type: ignore` directive
- error[missing-argument] tests/type/test_definition.py:152:16: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] tests/type/test_definition.py:154:13: No argument provided for required parameter `node` of function `parse_literal`
- error[invalid-argument-type] tests/type/test_definition.py:158:72: Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[Unknown, Unknown]`
- error[missing-argument] tests/type/test_scalars.py:60:24: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] tests/type/test_scalars.py:222:24: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] tests/type/test_scalars.py:348:24: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] tests/type/test_scalars.py:471:24: No argument provided for required parameter `node` of function `parse_literal`
- error[missing-argument] tests/type/test_scalars.py:616:24: No argument provided for required parameter `node` of function `parse_literal`
- Found 407 diagnostics
+ Found 397 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[missing-argument] mitmproxy/proxy/layers/modes.py:29:20: No argument provided for required parameter `event` of function `handle_event`
- error[missing-argument] mitmproxy/proxy/layers/modes.py:37:20: No argument provided for required parameter `event` of function `handle_event`
- error[missing-argument] test/mitmproxy/proxy/test_layer.py:170:17: No argument provided for required parameter `event` of function `handle_event`
- error[missing-argument] test/mitmproxy/proxy/test_layer.py:170:17: No argument provided for required parameter `event` of function `handle_event`
- Found 2111 diagnostics
+ Found 2107 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[missing-argument] tests/profiling/test_recorder.py:43:5: No argument provided for required parameter `events` of function `push_events`
- error[missing-argument] tests/profiling/test_scheduler.py:23:5: No argument provided for required parameter `events` of function `push_events`
- error[missing-argument] tests/profiling/test_scheduler.py:44:5: No argument provided for required parameter `events` of function `push_events`
- error[missing-argument] tests/profiling/test_scheduler.py:55:5: No argument provided for required parameter `events` of function `push_events`
- Found 6867 diagnostics
+ Found 6863 diagnostics

manticore (https://github.com/trailofbits/manticore)
- error[missing-argument] examples/script/aarch64/hello42.py:43:5: No argument provided for required parameter `callback` of function `subscribe`
+ error[call-non-callable] examples/script/aarch64/hello42.py:43:5: Object of type `None` is not callable
- error[missing-argument] examples/script/aarch64/hello42.py:47:5: No argument provided for required parameter `callback` of function `subscribe`
+ error[call-non-callable] examples/script/aarch64/hello42.py:47:5: Object of type `None` is not callable
- error[missing-argument] examples/script/basic_statemerging.py:27:5: No argument provided for required parameter `callback` of function `subscribe`
- error[missing-argument] examples/script/basic_statemerging.py:28:5: No argument provided for required parameter `callback` of function `subscribe`
+ error[call-non-callable] examples/script/basic_statemerging.py:27:5: Object of type `None` is not callable
+ error[call-non-callable] examples/script/basic_statemerging.py:28:5: Object of type `None` is not callable

```
</details>


---

_Marked ready for review by @sharkdp on 2025-05-21 10:47_

---

_Review requested from @carljm by @sharkdp on 2025-05-21 10:47_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-21 10:47_

---

_Review requested from @dcreager by @sharkdp on 2025-05-21 10:47_

---

_Comment by @sharkdp on 2025-05-21 11:46_

> ```diff
> + error[call-non-callable] examples/script/aarch64/hello42.py:43:5: Object of type `None` is not callable
> ```

These new ecosystem diagnostics are arguably true positives. There's a call to `m.subscribe` (on an instance `m` of `Manticore`). `subscribe` is a proper function on the class body of (a base of) `Manticore`. But there is also a `self.subscribe = None` assignment in another method. The calling code does not check if `m.subscribe is not None`, so the full type of `m.subscribe` is `(bound method Manticore.subscribe(…) -> …) | Unknown | None`.

---

_@AlexWaygood reviewed on 2025-05-21 14:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md`:227 on 2025-05-21 14:08_

Interesting... in some other cases, we will need to upcast function-literal types to `Callable` types before considering assignability. For example, when considering the Liskov Substitution Principle for subclasses, we'll want to emit an error for this, because there are calls to `Foo.method` that would succeed if called on instances of `Foo` but would not succeed when called on instances of `Bar`:

```py
class Foo:
    def method(self): ...

class Bar(Foo):
    def method(self, extra_arg): ...
```

but we'll want to allow this, even though `Foo.method` and `Bar.method` will be inferred as inhabiting distinct (and disjoint) function-literal types -- we'll need to upcast the two types to `Callable` types before considering assignability for Liskov purposes:

```py
class Foo:
    def method(self): ...

class Bar(Foo):
    def method(self): ...
```

But there is maybe an argument to say that we should limit that kind of function-literal upcasting to Liskov checks (and similar) where the type of the method definition is viewed from the perspective of code outside the original defining class (in this case, a subclass). I.e., in this situation here, we're overriding the method from inside a method in the same class, so maybe you're correct that we should disallow it here and not do the same function-literal upcasting. Not sure!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1771 on 2025-05-21 14:30_

```suggestion
                        // attribute is a non-data descriptor, it cannot possibly be the
```

---

_@AlexWaygood approved on 2025-05-21 14:34_

LGTM other than the long question I had about your TODO comment 😆

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md`:208 on 2025-05-21 15:05_

It feels to me here like, in conjunction with emitting a diagnostic on `self.attr = "normal"`, we should also be ignoring it, and just have the type `Literal["non-data"]` here.

I guess in the ecosystem manticore case, that would also suggest that we would error on the `self.subscribe = None` line, and not error on attempts to call `m.subscribe`. To me that seems correct? You could add a `ClassVar[Callable[...]] | None` annotation on the class to explicitly say it should be allowed to be `None`.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md`:227 on 2025-05-21 15:11_

I think if we want to support the kind of code shown here, we should ideally support it by abstracting the "declared" type for a function definition "declaration" to the callable type of the function literal rather than the function literal type itself. (The inferred type can still be the function literal type, of course.) It doesn't seem right to support this code by respecting an internal assignment on `self` that should be a type error, because it is violating a "declared" type on the class.

---

_@carljm reviewed on 2025-05-21 15:17_

I might be thinking about this wrongly, but I am not sure that this PR is adjusting the semantics in the way that we should adjust them?

---

_Renamed from "[ty] Invoke descriptor protocol when accessing class-body-declared symbols via `instance_member`" to "[ty] Ignore descriptor class-level declarations for purposes of finding instance attributes" by @sharkdp on 2025-05-21 17:30_

---

_Converted to draft by @sharkdp on 2025-05-21 18:51_

---
