```yaml
number: 16121
title: "[red-knot] Method calls and the descriptor protocol"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/descriptor-protocol
created_at: 2025-02-12T15:16:26Z
updated_at: 2025-02-21T17:28:38Z
url: https://github.com/astral-sh/ruff/pull/16121
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Method calls and the descriptor protocol

---

_Pull request opened by @sharkdp on 2025-02-12 15:16_

## Summary

This PR achieves the following:

* Add support for checking method calls, and inferring return types from method calls. For example:
  ```py
  reveal_type("abcde".find("abc"))  # revealed: int
  reveal_type("foo".encode(encoding="utf-8"))  # revealed: bytes
  
  "abcde".find(123)  # error: [invalid-argument-type]
  
  class C:
      def f(self) -> int:
          pass
  
  reveal_type(C.f)  # revealed: <function `f`>
  reveal_type(C().f)  # revealed: <bound method: `f` of `C`>
  
  C.f()  # error: [missing-argument]
  reveal_type(C().f())  # revealed: int
  ```
* Implement the descriptor protocol, i.e. properly call the `__get__` method when a descriptor object is accessed through a class object or an instance of a class. For example:
  ```py
  from typing import Literal
  
  class Ten:
      def __get__(self, instance: object, owner: type | None = None) -> Literal[10]:
          return 10
  
  class C:
      ten: Ten = Ten()
  
  reveal_type(C.ten)  # revealed: Literal[10]
  reveal_type(C().ten)  # revealed: Literal[10]
  ```
* Add support for member lookup on intersection types.
* Support type inference for `inspect.getattr_static(obj, attr)` calls. This was mostly used as a debugging tool during development, but seems more generally useful. It can be used to bypass the descriptor protocol. For the example above:
  ```py
  from inspect import getattr_static
  
  reveal_type(getattr_static(C, "ten"))  # revealed: Ten
  ```
* Add a new `Type::Callable(…)` variant with the following sub-variants:
  * `Type::Callable(CallableType::BoundMethod(…))` — represents bound method objects, e.g. `C().f` above
  * `Type::Callable(CallableType::MethodWrapperDunderGet(…))` — represents `f.__get__` where `f` is a function
  * `Type::Callable(WrapperDescriptorDunderGet)` — represents `FunctionType.__get__`
* Add new known classes:
  * `types.MethodType`
  * `types.MethodWrapperType`
  * `types.WrapperDescriptorType`
  * `builtins.range`

## Performance analysis

On this branch, we do more work. We need to do more call checking, since we now check all method calls. We also need to do ~twice as many member lookups, because we need to check if a `__get__` attribute exists on accessed members.

A brief analysis on `tomllib` shows that we now call `Type::call` 1780 times, compared to 612 calls before. ~~We also emit 18 additional diagnostics, which might also explain some of the performance regression.~~

I did some wall time benchmarks as well and only find a minor 1% regression on `black`, and a 6% regression on `tomllib`.

## `black`

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./red_knot_feature check --project ~/black` | 184.1 ± 8.1 | 166.1 | 193.2 | 1.01 ± 0.07 |
| `./red_knot_main check --project ~/black` | 182.7 ± 10.1 | 164.1 | 194.6 | 1.00 |

## `tomllib`

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./red_knot_feature check --project ~/tomllib` | 24.6 ± 0.9 | 22.7 | 26.7 | 1.06 ± 0.06 |
| `./red_knot_main check --project ~/tomllib` | 23.2 ± 1.1 | 21.3 | 29.0 | 1.00 |

## Limitations

* Data descriptors are not yet supported, i.e. we do not infer correct types for descriptor attribute accesses in `Store` context and do not check writes to descriptor attributes. I felt like this was something that could be split out as a follow-up without risking a major architectural change.
* We currently distinguish between `Type::member` (with descriptor protocol) and `Type::static_member` (without descriptor protocol). The former corresponds to `obj.attr`, the latter corresponds to `getattr_static(obj, "attr")`. However, to model some details correctly, we would also need to distinguish between a static member lookup *with* and *without* instance variables. The lookup without instance variables corresponds to `find_name_in_mro` [here](https://docs.python.org/3/howto/descriptor.html#invocation-from-an-instance). We currently approximate both using `member_static`, which leads to two open TODOs. Changing this would be a larger refactoring of `Type::own_instance_member`, so I chose to leave it out of this PR.

Please let me know if you would like to see any or both of these limitations be solved in this PR instead.

## Test Plan

* New `call/methods.md` test suite for method calls
* New tests in `descriptor_protocol.md`
* New `call/getattr_static.md` test suite for `inspect.getattr_static`
* Various updated tests 

---

_Label `red-knot` added by @sharkdp on 2025-02-12 15:16_

---

_@sharkdp reviewed on 2025-02-17 15:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:76 on 2025-02-17 15:24_

It doesn't look like it, but this is progress. We now understand attribute access on `StringLiteral` types, but `str.join` is an `@overload`ed method.

---

_Comment by @codspeed-hq[bot] on 2025-02-17 15:27_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fdescriptor-protocol)

### Merging #16121 will **degrade performances by 6.88%**

<sub>Comparing <code>david/descriptor-protocol</code> (05d689d) with <code>main</code> (f62e540)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` red_knot_check_file[incremental] `` | 5.1 ms | 5.5 ms | -6.88% |


---

_Comment by @github-actions[bot] on 2025-02-17 15:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2025-02-17 20:14_

---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/red_knot.rs`:76 on 2025-02-17 20:14_

So these simply happen because we don't understand protocols yet, and don't understand that `int` is assignable to `SupportsIndex`.

I invested 20 min trying to get rid of these by treating `Protocol` as a dynamic `@Todo` base, but ran into problems and decided to simply add these for now. Let me know if you think we should patch this somehow until we add proper support for protocols.

---

_@sharkdp reviewed on 2025-02-17 20:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:856 on 2025-02-17 20:16_

Member access on intersection types is a pre-existing TODO that I did not solve in this PR.

---

_@sharkdp reviewed on 2025-02-17 20:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1007 on 2025-02-17 20:22_

So this change is probably unexpected, but I'm not sure it's important enough to fix just for this test. A static member lookup of `getattr_static(f, "__defaults__")` still gives us `@Todo(full tuple[...] support) | None` like before, but when we now look up `__get__` on this type, we get `@Todo(…)` from the first element of the union, and `Symbol::Unbound` from the second element of the union (`None`). We then call `UnionType::map_with_boundness` to combine those into a single symbol, which ends up being a possibly-unbound `@Todo(…)` type. This is why the `None` has vanished from the union.

A new TODO comment in `types.rs` mentions how this should/could be solved eventually (e.g. with a `Symbol::Union` variant), but I didn't want to include a `Symbol` refactor in this branch.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:25 on 2025-02-17 20:39_

This change — and similar ones below — are not strictly needed, but I didn't want to see `Unknown` everywhere. We have a new test for undeclared descriptors further down below.

---

_@sharkdp reviewed on 2025-02-17 20:39_

---

_@sharkdp reviewed on 2025-02-17 20:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:12 on 2025-02-17 20:43_

When we now look up `types.ModuleType.__loader__`, we try to access the `__get__` member on `LoaderProtocol | None`, which triggers this `@Todo`. I thought this wasn't important enough to fix this, but let me know if you think otherwise.

---

_@sharkdp reviewed on 2025-02-18 08:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1009 on 2025-02-18 08:56_

I didn't introduce any special handling of `__call__` in this PR, but we might want to model this as `<method-wrapper '__call__' of 'f'>`, similar to `__get__`, if we need any special-casing in `Type::call` for it.

---

_@sharkdp reviewed on 2025-02-18 09:10_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:132 on 2025-02-18 09:10_

I didn't consider this high priority, so it's left as a TODO for now.

---

_@sharkdp reviewed on 2025-02-18 11:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4669 on 2025-02-18 11:18_

This whole logic here was simply extracted into a function. The logic itself didn't change.

---

_Marked ready for review by @sharkdp on 2025-02-18 11:31_

---

_Review requested from @carljm by @sharkdp on 2025-02-18 11:31_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-18 11:31_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-18 11:31_

---

_Converted to draft by @sharkdp on 2025-02-18 19:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:856 on 2025-02-19 01:00_

Related to the discussion about descriptor protocol for unions in Discord, I suspect that handling this correctly (not in this PR) will require following the descriptor protocol separately for each member of the intersection and then intersecting the results, whether this is done manually in the descriptor protocol implementation or via some kind of generalized support that we add for this "map some arbitrary, possibly composite, type operation over a union/intersection" pattern.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1037 on 2025-02-19 01:07_

```suggestion
bools are instances of that class:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:232 on 2025-02-19 01:21_

I haven't looked at the implementation yet, but this struck me as a TODO that might result in significant architecture changes if deferred? I could be wrong, though.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:12 on 2025-02-19 01:23_

We should of course fix this `@Todo` at some point, but I agree there's no need to do that in this PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:955 on 2025-02-19 03:44_

I'm slightly concerned about how many of these type properties are being defined based on `Type::Callable` being a single-valued / singleton type, which will not be true of a more general `Type::Callable`. But I guess it makes sense to do this for now and leave it up to future work to decide how to change this, since it's not totally clear yet if we'll want to keep these single-valued variants or switch to a general callable type for bound methods and method wrappers. (We will need to keep the specific variants if we want to keep e.g. the fallbacks to `MethodType` and `WrapperDescriptorType`, and if we keep the specific variants, I'm not even sure they should be grouped under the same `Type` variant as general callable types, which would call into question the use of the `Type::Callable` name here.)

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:70 on 2025-02-19 03:45_

This comment now seems misplaced.

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:76 on 2025-02-19 03:46_

I think it's fine to just have these as known false positives for now

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1910 on 2025-02-19 06:57_

It would only make sense to emit a diagnostic if there's no default given, right?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2602 on 2025-02-19 06:59_

So if I understand correctly, these were added only so that we could special case them as "not descriptors" because they are relied on by our test suite?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1532 on 2025-02-19 07:07_

I do think this describes what we need to do correctly, but I'm still not totally convinced that embedding this in `Symbol` is the best implementation approach, vs abstracting the concept of mapping an operation (expressed as a callback) over a type (which may be a union or intersection). This avoids having to package up a union or intersection of symbols inside `Symbol` (making it another representation of union or intersection outside Type).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1606 on 2025-02-19 07:12_

Is there a reason we don't handle this the same as `Type::ClassLiteral`? Is that incorrect for a reason I'm missing, or does it result in some problems?

---

_@carljm reviewed on 2025-02-19 07:26_

This is looking pretty good to me! Left a few comments.

---

_@sharkdp reviewed on 2025-02-19 07:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2602 on 2025-02-19 07:56_

Yes. It's not even required for our current test cases. I saw uses of `memoryview` in our tests (probably just as a way to construct "some" distinct type different from `str`, `int`, …). And I wanted to avoid any sort of surprises if someone were to create a `memoryview` attribute somewhere, before we implement generics and protocols.

I'm also fine with removing these again.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:955 on 2025-02-19 08:00_

> I'm slightly concerned about how many of these type properties are being defined based on `Type::Callable` being a single-valued / singleton type, which will not be true of a more general `Type::Callable`

Yes, this is why I explicitly matched on these three variants, instead of a generic `Type::Callable(…)` pattern.

> and if we keep the specific variants, I'm not even sure they should be grouped under the same `Type` variant as general callable types, which would call into question the use of the `Type::Callable` name here.

I'm fine with choosing another name for this `Type` variant right now, if we think that it avoids confusion once we add a generic `Callable` type.

---

_@sharkdp reviewed on 2025-02-19 08:00_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1910 on 2025-02-19 08:00_

Yes. I'll update the TODO comment.

---

_@sharkdp reviewed on 2025-02-19 08:00_

---

_@sharkdp reviewed on 2025-02-19 08:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:232 on 2025-02-19 08:06_

This TODO refers to the second "limitation" in the PR description:

> We currently distinguish between `Type::member` (with descriptor protocol) and `Type::static_member` (without descriptor protocol). The former corresponds to `obj.attr`, the latter corresponds to `getattr_static(obj, "attr")`. However, to model some details correctly, we would also need to distinguish between a static member lookup *with* and *without* instance variables. The lookup without instance variables corresponds to `find_name_in_mro` [here](https://docs.python.org/3/howto/descriptor.html#invocation-from-an-instance). We currently approximate both using `member_static`, which leads to two open TODOs. Changing this would be a larger refactoring of `Type::own_instance_member`, so I chose to leave it out of this PR.

> might result in significant architecture changes if deferred

Yes. I think it will require us to split `static_member` into two methods, a parametrized method, or something similar. I do not think that it requires any significant changes to the logic being added in this PR. Ideally, we would just replace one `.static_member(…)` lookup with a `.find_in_mro(…)` lookup, but I could be wrong.

I can try to look into this today and potentially add it to this PR.

---

_@sharkdp reviewed on 2025-02-19 13:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1007 on 2025-02-19 13:27_

This change is now gone.

---

_@sharkdp reviewed on 2025-02-19 13:29_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1532 on 2025-02-19 13:29_

This comment is now removed, and we properly support unions by matching on them explicitly. This was implemented in a fraction of the time that it took me to write this comment :upside_down_face:.

---

_@sharkdp reviewed on 2025-02-19 13:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1606 on 2025-02-19 13:32_

> Is there a reason we don't handle this the same as `Type::ClassLiteral`?

Not really, except that I thought you mentioned something regarding `SubtypeOf` being slightly more complex (https://github.com/astral-sh/ruff/issues/15948?), and that it would be okay to leave it as a TODO. But I was now "forced" to move this pattern to the `ClassLiteral` branch due to some test failures, so your reasoning seems correct :+1: 

---

_@sharkdp reviewed on 2025-02-19 13:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:856 on 2025-02-19 13:43_

Ok, I added support for intersections now.

---

_Marked ready for review by @sharkdp on 2025-02-19 15:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:232 on 2025-02-19 16:11_

I did look into this and got quite far, but I'd prefer to postpone this, and focus on what's in this PR first. There's also some interaction between this and *data* descriptors (because we would do a non-instance-member lookup, then check if `__set__` is available, and only fall back on an instance-member-lookup if it's not available), which are also not supported yet.

---

_@sharkdp reviewed on 2025-02-19 16:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:99 on 2025-02-19 17:42_

Nit: I would probably use `write!` here
```suggestion
                write!(
                    f, 
                    "<bound method `{method}` of `{instance}`>`",
                    method = bound_method.function(self.db).name(self.db),
                    instance = bound_method.self_instance(self.db).display(self.db)
                )?;
```

And the same for the other `Callable` handler

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1537 on 2025-02-19 17:48_

I don't think this needs to be done but I've a slight preference if this method would already return a `Result` and
that we instead add a `TODO` at the call sites that suppress the error handling. Doing this now also ensures that new call sites *have* to think about handling the error or it requires an explicit TODO comment if they can't (e.g. because it's a call in `.memeber`)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1608 on 2025-02-19 17:49_

Could we add a `TODO` here that it should return a `Result` instead of the `Symbol` directly.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1798 on 2025-02-19 17:52_

Should this error if the first argument is `None`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2075 on 2025-02-19 17:53_

Nit: It might be worth introducing a `CallArguments::emppy` or a static?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3793 on 2025-02-19 17:54_

Is this guaranteed to be an `InstanceType` or could it be something else? If not, should this be `InstanceTYpe` instead of `Type`?

---

_@MichaReiser approved on 2025-02-19 17:55_

---

_@AlexWaygood reviewed on 2025-02-19 19:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2602 on 2025-02-19 19:15_

I see the rationale for adding them, but I do worry that we'll forget to remove them again when they're no longer needed... `memoryview` in particular is kind of wacky, since it's pretty obscure relative to most other builtin classes 😆

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/getattr_static.md`:1 on 2025-02-19 19:24_

is it worth also adding examples of what we infer when `inspect.getattr_static` is passed a dynamic string?

```py
class Foo:
    attr: int

def f(x: str, y: list):
    reveal_type(getattr_static(Foo(), x)
    reveal_type(getattr_static(Foo(), x, "bar")
    reveal_type(getattr_static(Foo(), x, y)
```

and when it is passed invalid arguments?

```py
class Foo:
    attr: int

def f(x: str):
    reveal_type(getattr_static(Foo(), 42))
    reveal_type(getattr_static(Foo(), "attr", "bar", "baz"))
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:22 on 2025-02-19 19:26_

it could be useful to link to the Descriptor HOWTO somewhere from this first paragraph (and possibly Brent's blogpost unravelling attribute access too!)

---

_@sharkdp reviewed on 2025-02-19 19:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3793 on 2025-02-19 19:28_

This can contain all kinds of types. For example, if we call `"foo".find`, the bound method type contains a `Type::StringLiteral` instance. And I could imagine that this extra precision (over falling back to `str`) might be helpful later if a class method dispatches on the type of `self`, for example.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:26 on 2025-02-19 19:32_

```suggestion
function attribute. The way the special `__get__` method *on functions* works is as follows. In
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:27 on 2025-02-19 19:32_

```suggestion
the former case, if the `instance` argument is `None`, `__get__` simply returns the function itself. In
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:86 on 2025-02-19 19:39_

Fun Python esoteric knowledge that is probably irrelevant here, but which you may find interesting: if there are extra attributes present in a function's instance `__dict__` (e.g. attributes that have been monkey-patched onto the function), those attributes are also accessible from a bound method derived from the function. E.g.

```pycon
>>> def foo(self): pass
... 
>>> foo.bar = 42
>>> method = foo.__get__(object())
>>> method
<bound method foo of <object object at 0x1022307b0>>
>>> method.bar
42
>>> foo.bar = 56
>>> method.bar
56
```

I'm not sure exactly whether attributes are first looked up in the underlying function's instance dictionary or the class dictionary of `types.MethodType`.

Anyway, probably irrelevant for us here, as I say!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:88 on 2025-02-19 19:53_

```suggestion
Descriptors that define `__set__` or `__delete__` are called *data descriptors*. An example
of a data descriptor is a `property` with a setter and/or a deleter.
Descriptors that only define `__get__`, meanwhile, are called *non-data descriptors*. Examples include
functions, `classmethod` or `staticmethod`).
```

---

_@AlexWaygood reviewed on 2025-02-19 19:54_

Just a partial review of the tests so far -- but I will continue reviewing tomorrow :-)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1048 on 2025-02-20 00:38_

Not related to this PR, but just noticed it:

```suggestion
All attribute access on literal `bytes` types is currently delegated to `builtins.bytes`:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:112 on 2025-02-20 00:43_

```suggestion
        # This explains why data descriptors come first in the precedence chain. If
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:232 on 2025-02-20 00:45_

All makes sense, sounds good!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:955 on 2025-02-20 07:05_

Eh, I'm not sure we're clear enough on what things will look like with a generic callable type to make such decisions now, so I think we should just leave it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1798 on 2025-02-20 07:09_

No, first argument being `None` is the signal that the descriptor is being called on a class instead of an instance, it's not an error; returning the function itself is correct in that case (for a function `__get__`) and matches the runtime behavior.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2602 on 2025-02-20 07:11_

Yeah I would also be inclined to remove the `memoryview` one; `range` it seems plausible to me that we might want to special-case at some point, but `memoryview` not really. And I don't think it's likely that our tests will ever do more with it than they do today, which is use it as an opaque built-in type that's distinct from other builtin types. (Maybe those tests would be clearer and simpler, if a bit more verbose, if they just created classes `A`, `B`, `C` etc, instead of using a bunch of builtin types.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1606 on 2025-02-20 07:19_

Oh, calling `SubclassOf` is more complex because `__init__` and `__new__` overrides aren't required to obey Liskov, so calling a `SubclassOf` type is inherently unsound. I don't think this applies to member access or descriptor handling, though. (Well, potentially it could apply to member access of `__init__` or `__new__` method on a `SubclassOf` type, since this would also be unsound. But that would be handled as a special case, doesn't need to affect anything in this PR.)

---

_@carljm approved on 2025-02-20 07:24_

This looks really good to me! The cold-check regression (6% on tomllib, 1% on black) seems well within the range of expectations considering the added complexity we are modeling here and the extra work required to model it. (I do wonder whether making `try_call_dunder_get` a Salsa query might be a win here, if it means we don't have to repeatedly try to access `__get__` member on many types that don't even have it.

Or if making `own_instance_member` a query (to fix https://github.com/astral-sh/ruff/issues/16172) could help -- did we already try that, or just discuss it on Discord? It definitely seems plausible that all the extra instance-member-getting we do here (of `__get__` methods) could be causing a new direct cross-module AST dependency in our benchmark.

---

_@sharkdp reviewed on 2025-02-20 08:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1798 on 2025-02-20 08:41_

Yes, thank you! I'm now modelling this much more precisely by creating a `Signature` dynamically, which let's us catch all kinds of call errors. This also revealed two minor mistakes in one of the descriptor tests where I was passing a function `f` as the `owner` argument, instead of `type(f)`. Check out the changes in this commit: 28e37cbbea957b03cafbebda761dba2815a61699

---

_@sharkdp reviewed on 2025-02-20 08:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1798 on 2025-02-20 08:45_

> No, first argument being `None` is the signal that the descriptor is being called on a class instead of an instance, it's not an error

I think we need to distinguish Rust's `None` and Python's `None`. We handle a Python `None` correctly already. What @MichaReiser was referring to was probably a Rust `None`, i.e. that no parameter is passed at all. This was *not* handled correctly, but it is now fixed with the referenced commit.

---

_@sharkdp reviewed on 2025-02-20 08:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2602 on 2025-02-20 08:53_

Ok. Removed `memoryview`

---

_@sharkdp reviewed on 2025-02-20 08:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2602 on 2025-02-20 08:54_

> Maybe those tests would be clearer and simpler, if a bit more verbose, if they just created classes A, B, C etc, instead of using a bunch of builtin types

I agree; will note it down as a low-prio todo

---

_@sharkdp reviewed on 2025-02-20 09:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:22 on 2025-02-20 09:25_

Brent? :stuck_out_tongue: 

I linked to [this section](https://docs.python.org/3/howto/descriptor.html#functions-and-methods) in the descriptor guide now. We already refer to the full descriptor guide in `descriptor_protocol.md`, but this section is about `FunctionType.__get__` specifically.

Brett's post doesn't mention bound methods at all, but I'll probably link to that as well when I work on the follow up which distinguishes between `getattr_static`/`Type::static_member` and `find_name_in_mro` :+1: 

---

_@sharkdp reviewed on 2025-02-20 09:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:86 on 2025-02-20 09:47_

Yes. Bound methods simply keep a reference to the function in a `im_func` member …

https://github.com/python/cpython/blob/10b32054ad6bce821e3b40101d4414d025be6e36/Objects/classobject.c#L124

… which they expose via `bound_method.__func__` here …

https://github.com/python/cpython/blob/10b32054ad6bce821e3b40101d4414d025be6e36/Objects/classobject.c#L161C32-L161C38

… and they fall back to looking up attributes on the function in `getattr` here:

https://github.com/python/cpython/blob/10b32054ad6bce821e3b40101d4414d025be6e36/Objects/classobject.c#L212

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:1 on 2025-02-20 10:50_

Do we have any tests that demonstrate our behaviour when a non-data descriptor is overridden on an instance? This is another case where, while it works at runtime, I think it's questionable whether we want to allow it, as it's often indicative of a user mistake IMO. For example, you could have something like this, where a non-data descriptor is used to log all accesses of an underlying attribute:

```pycon
>>> import datetime, pprint
>>> LOG = []
>>> class LoggedAccessList:
...     def __set_name__(self, owner, name):
...         self.public_name = name
...         self._private_name = f"_{name}"
...         setattr(owner, self._private_name, [])
... 
...     def __get__(self, instance, owner):
...         if instance is None:
...             return self
...         LOG.append(f"List {self.public_name!r} on {instance.__class__.__name__!r} object was accessed and possibly modified at {datetime.datetime.now().strftime('%d/%m/%y %H:%M:%S')}")
...         return getattr(instance, self._private_name)
...         
>>> class Foo:
...     bar = LoggedAccessList()
...     
>>> f = Foo()
>>> f.bar
[]
>>> f.bar
[]
>>> pprint.pp(LOG, width=100)
["List 'bar' on 'Foo' object was accessed and possibly modified at 20/02/25 10:48:10",
 "List 'bar' on 'Foo' object was accessed and possibly modified at 20/02/25 10:48:11"]
```

Since `LoggedAccessList` is a non-data descriptor here, it's possible to override it on instances... but now accesses to the underlying list are no longer logged:

```pycon
>>> f.bar = []
>>> f.bar
[]
>>> f.bar
[]
>>> # there are no more log entries than there were before...
>>> LOG
["List 'bar' on 'Foo' object was accessed and possibly modified at 20/02/25 10:48:10", "List 'bar' on 'Foo' object was accessed and possibly modified at 20/02/25 10:48:11"]
```

Since it works at runtime, perhaps this is too strict a rule to be enabled by default, but I'd love to have a disabled-by-default rule prohibiting non-data descriptors from being overridden in this way. If you want a descriptor to be overridable, you should really give it an explicit `__set__` method IMO.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:38 on 2025-02-20 10:53_

```suggestion
# TODO: This should be an error (as the wrong type is being implicitly passed to `Ten.__set__`),
# but the error message is misleading.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:42 on 2025-02-20 10:53_

```suggestion
# TODO: same as above
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:143 on 2025-02-20 10:57_

hmm... I suppose this is consistent behaviour, though I don't really love it. We should consider having an option which, when enabled, would cause us to disallow them to be overridden on the class object. If that option was enabled, we could just use the return type of `__get__` without the `Unknown |` union for the type of the attribute when accessed on instances or classes.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:269 on 2025-02-20 10:59_

```suggestion
attributes, since that attribute could be overwritten externally. Even a data descriptor with a
`__set__` method can be overwritten when accessed through a class object.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:274 on 2025-02-20 11:02_

I know this specific example is showing what happens if a descriptor is _set_ on the class object. But the `__get__` signature here implies that if the attribute is _read_ (rather than _set_) on the class, we should emit an error, since `None` would be passed as the first argument to `__get__` in that case. Is that correct?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:285 on 2025-02-20 11:03_

should this have a TODO next to it about how this would change to `Literal["something else"]` if we did local-scope narrowing of attributes based on assignments?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:391 on 2025-02-20 11:06_

haha! Taking literate testing to the next level by integrating the assertion as part of a sentence 🤣

---

_@AlexWaygood reviewed on 2025-02-20 11:08_

The tests are fantastic! Looking at the code now...

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:86 on 2025-02-20 11:10_

Ah yes, and of course that's visible from Python if you do something like this -- the attribute in the `MethodType` class dictionary takes precedence over the attribute in the function's instance dictionary!

```pycon
>>> def foo(): ...
... 
>>> foo.__hash__ = 42
>>> method = foo.__get__(object())
>>> method.__hash__
<method-wrapper '__hash__' of method object at 0x102ed0a40>
```

I was obviously lacking imagination last night 😄

---

_@AlexWaygood reviewed on 2025-02-20 11:10_

---

_@AlexWaygood reviewed on 2025-02-20 11:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:1 on 2025-02-20 11:14_

Another idea for a test: a non-callable `__set__` method:

```pycon
>>> class Foo:
...     def __get__(self, instance, owner):
...         return 42
...     __set__ = None
...     
>>> class Bar:
...     foo = Foo()
...     
>>> b = Bar()
>>> b.foo
42
>>> b.foo = 56
Traceback (most recent call last):
  File "<python-input-60>", line 1, in <module>
    b.foo = 56
    ^^^^^
TypeError: 'NoneType' object is not callable
```

Interestingly, the traceback range from CPython looks kind-of off there -- it should probably highlight the whole attribute assignment rather than just the left-hand side

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:76 on 2025-02-20 11:22_

this makes me wonder whether we should implement `FromIterator` for `Parameters`. But I suppose we don't really need it right now; `new()` seems like it leads to more readable code for our current use cases.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:636 on 2025-02-20 11:25_

I assume you've checked that all the stable property tests pass with the changes to `Type::is_subtype_of()`, etc.? :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1500 on 2025-02-20 11:36_

Should we have a TODO here about how we need to take the `LiteralString` overloads in the `str` methods if it's a method access on `LiteralString`, once we understand overloads? Or will that just naturally take care of itself?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1514 on 2025-02-20 11:39_

While this isn't _wrong_, there might be some methods here where we could do quite a lot better than typeshed's stubs. Typeshed's stubs basically assume that `self` is a homogeneous tuple, but `Type::Tuple` in our model is a heterogeneous tuple.

I don't know if it's actually worth it to add any special casing here; I haven't really spent that much time thinking about it... it might at least be worth a TODO comment to look into the question?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1537 on 2025-02-20 12:06_

could you possibly add some doc-comments explaining what it means if this function returns `Ok(None)`, and how that differs from returning an `Err()` variant?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1417 on 2025-02-20 12:09_

It looks like we now have this special case in both `Type::member()` and `Type::static_member()`. Do we _need_ it on both?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1605 on 2025-02-20 12:19_

Hmm, okay. I think the fun esoterics I was mentioning in https://github.com/astral-sh/ruff/pull/16121#discussion_r1962267062 _are_ relevant here, actually! For example, typeshed declares a `__kwdefaults__` attribute on `types.FunctionType`, but not on `types.MethodType`. But the `__kwdefaults__` attribute can be accessed on methods too (it just delegates to the underlying function, as we discussed):

```pycon
>>> def f(self): ...
... 
>>> method = f.__get__(object())
>>> method.__kwdefaults__ is None
True
```

IDK if this is something we should model explicitly here or not, though. Typeshed's stubs appear to assume elsewhere that type checkers will _not_ model this attribute delegation: it has these properties declared in the stub for `types.MethodType` even though they do not appear at runtime: https://github.com/python/typeshed/blob/ac8f2632ec37bb4a82ade0906e6ce9bdb33883d3/stdlib/types.pyi#L463-L468, because of the fact that accessing these properties on an instance of `types.MethodType` will always just grab the attribute from the underlying function object. So perhaps we could fix this issue by just adding some more properties to the typeshed stub, rather than accurately modeling the runtime semantics here. It might be more performant.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1866 on 2025-02-20 12:22_

the construction of these parameter types looks like it could possibly be cached as a Salsa query? That _might_ improve the perf here a bit?

Simply cloning a short array of `Type`s might be more performant than constructing a new one from scratch each time, given that constructing a new one from scratch requires many Salsa db lookups, but cloning a `Type` should be cheap since most variants are just a u32

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3745 on 2025-02-20 12:26_

```suggestion
/// This type represents bound method objects that are created when a method is accessed
```

---

_@AlexWaygood reviewed on 2025-02-20 12:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2067 on 2025-02-20 12:32_

I think this gives us less-than-ideal behaviour for something like this:

```py
from inspect import getattr_static
from typing import Any, reveal_type

def _(x: Any):
    reveal_type(getattr_static(x, "foo", 42))
```

We reveal `Any` here on this branch, but a more precise type would be `Any | Literal[42]`

---

_@AlexWaygood reviewed on 2025-02-20 12:32_

---

_@AlexWaygood approved on 2025-02-20 12:32_

This is fantastic -- great work!

---

_@sharkdp reviewed on 2025-02-20 12:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:1 on 2025-02-20 12:37_

> the traceback range from CPython looks kind-of off there -- it should probably highlight the whole attribute assignment rather than just the left-hand side

I guess you could argue that a mere access of that descriptor object *in store-context* is an error, as it would necessarily invoke a call to `__set__`? For example, if you had `for b.foo in iterable: ...`, it's reasonable to only highlight the `b.foo` as the loop variable (which is what CPython does for that case).

---

_@AlexWaygood reviewed on 2025-02-20 12:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:1 on 2025-02-20 12:39_

Yeah, fair point -- and I think you're right that that's probably why we end up with that range. It still looks odd to a user in this specific case IMO, though!

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:285 on 2025-02-20 12:51_

I added a comment without a TODO.

---

_@sharkdp reviewed on 2025-02-20 12:51_

---

_@sharkdp reviewed on 2025-02-20 12:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:143 on 2025-02-20 12:58_

> we could just use the return type of __get__ without the Unknown | union for the type of the attribute when accessed on instances or classes.

Yes. This is how I initially modeled it, but then Carl rightly pointed out that they can be overwritten on the class.

> consider having an option

I can't really tell how much demand there would be for an option like this. I'm going to assume it's fine to defer this for now.

---

_@AlexWaygood reviewed on 2025-02-20 12:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:143 on 2025-02-20 12:59_

> I'm going to assume it's fine to defer this for now.

of course

---

_@sharkdp reviewed on 2025-02-20 13:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:274 on 2025-02-20 13:01_

> But the `__get__` signature here implies that if the attribute is _read_ (rather than _set_) on the class, we should emit an error, since `None` would be passed as the first argument to `__get__` in that case. Is that correct?

I you access `C.descriptor` on the class itself, `None` would be passed as the `instance` argument, yes. But that should not produce an error. `None` is compatible with `object`. This `__get__` method here was not supposed to distinguish between class-based or instance-based access.

---

_@AlexWaygood reviewed on 2025-02-20 13:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:274 on 2025-02-20 13:03_

> `None` is compatible with `object`

duh, of course. Sorry, I guess I forgot this was the case :-)

---

_@AlexWaygood reviewed on 2025-02-20 13:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2067 on 2025-02-20 13:16_

I suppose this would probably require some maybe-ugly special casing for dynamic types...

---

_@sharkdp reviewed on 2025-02-20 13:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:636 on 2025-02-20 13:34_

Ahem, yes, of course… *cough*.

Completely forgot. But they do pass. Even after I now added `FunctionLiteral` and `Type::Callable(Callable::BoundMethod(…))` as new types.

---

_@sharkdp reviewed on 2025-02-20 13:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1500 on 2025-02-20 13:37_

> Or will that just naturally take care of itself?

I assume we're going to test on all kinds of types once we add support for overloads. And nothing would need to be changed in this particular place. We would grab the static member of `str` here, and it would be some overloaded `Callable` type. We would then bind an instance of `LiteralString` to it, and later dispatch on the type of `self` when calling that bound method.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1417 on 2025-02-20 13:54_

Hm. In a previous iteration of my implementation, we needed both — but that does not seem to be the case anymore. In fact, the one in `static_member` is wrong! Because what we actually get from `getattr_static(x, "__class__")` is a `getset_descriptor` (of type `GetSetDescriptorType`), i.e. a callable, and not a string. I'm going to assume it's not a high priority to model this precisely right now. I removed the branch in `static_member`, which means that we fall back to the `object.__class__` annotation from typeshed, which we do not understand yet.

---

_@sharkdp reviewed on 2025-02-20 13:54_

---

_@sharkdp reviewed on 2025-02-20 14:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2067 on 2025-02-20 14:04_

> but a more precise type would be `Any | Literal[42]`

I don't think so. `Any` is the correct type for this example. If the attribute `foo` actually exists, it could literally be of *any* type:

```py
class C:
    foo = True

_(C())  # would reveal a type of `bool`, which is not gradual-equivalent to `Any | Literal[42]`
```

---

_@AlexWaygood reviewed on 2025-02-20 14:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2067 on 2025-02-20 14:09_

> I don't think so. `Any` is the correct type for this example. If the attribute `foo` actually exists, it could literally be of _any_ type:

Right... but if it doesn't exist, the returned value of `getattr_static(x, "foo", 42)` will be `42`. By including `Literal[42]` in the union, we force the user to make sure that any operations they attempt on the returned value are valid for values of type `Literal[42]`.

Anyway, this obviously shouldn't block merging; it's a pretty minor issue in the broader scheme of things.

---

_@sharkdp reviewed on 2025-02-20 14:23_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1605 on 2025-02-20 14:23_

Thanks for pointing it out once more. I simply modeled that fallback correctly now. See a7a6a2a0f10b63021716890b066d4d2ec4d5f4d4

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1619 on 2025-02-20 14:27_

Does this work?

```suggestion
                    KnownClass::MethodType
                        .to_instance(db)
                        .member(db, name)
                        .or_fall_back_to(db, || {
                            // If an attribute is not available on the bound method object,
                            // it will be looked up on the underlying function object:
                            Type::FunctionLiteral(bound_method.function(db)).member(db, name)
                        })
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:97 on 2025-02-20 14:30_

`__kwdefaults__` might be a better example here, since [typeshed says](https://github.com/python/typeshed/blob/ac8f2632ec37bb4a82ade0906e6ce9bdb33883d3/stdlib/builtins.pyi#L102-L105) that `object` has a `__module__` attribute (even though it doesn't -- see https://github.com/python/typeshed/pull/8787, https://github.com/python/typeshed/pull/8789, and https://github.com/python/typeshed/issues/12128 for prior discussion...)

---

_@AlexWaygood reviewed on 2025-02-20 14:30_

---

_@sharkdp reviewed on 2025-02-20 14:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2067 on 2025-02-20 14:45_

You're right of course. Not sure what I was thinking. Attempted to solve this in e2d52a826c550128182d632f891137686b197e43

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:97 on 2025-02-20 14:47_

Oops, thanks. And sorry for changing your original example. I wanted to choose something where we do not produce a `@Todo` type.

---

_@sharkdp reviewed on 2025-02-20 14:47_

---

_@sharkdp reviewed on 2025-02-20 15:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:1 on 2025-02-20 15:37_

> If you want a descriptor to be overridable, you should really give it an explicit `__set__` method IMO.

But a descriptor can not really replace itself using `__set__`, can it? So it's probably a valid thing to do. I'd like to revisit this when I work on assignments to descriptors, which is not included in this PR.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:1 on 2025-02-20 16:10_

> But a descriptor can not really replace itself using `__set__`, can it?

no, but if `__set__` is defined it means that the attribute can be set on _instances_ without the descriptor object itself being entirely shadowed by an attribute in an instance dictionary

> I'd like to revisit this when I work on assignments to descriptors, which is not included in this PR.

SGTM 👍

---

_@AlexWaygood reviewed on 2025-02-20 16:10_

---

_Merged by @sharkdp on 2025-02-20 22:22_

---

_Closed by @sharkdp on 2025-02-20 22:22_

---

_Branch deleted on 2025-02-20 22:22_

---

_@carljm reviewed on 2025-02-20 23:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2602 on 2025-02-20 23:21_

> low-prio todo

Apparently @sharkdp low-prio todos are the ones that are complete and landed the following morning 😆 You don't want to see how efficiently the high-pri ones get done!

---

_@carljm reviewed on 2025-02-20 23:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:1 on 2025-02-20 23:40_

Shadowing non-data descriptors with instance-dict attributes is a quite common and useful thing to do; it's the core of a widely-used technique for creating very efficient "cached properties", which now also exists in the stdlib as `functools.cached_property`. I think enabling this use case is the reason why non-data descriptors work the way they do; otherwise it would have been simpler to just make all descriptors non-shadowable. So I do not think we should forbid this or emit a diagnostic on it. Maybe it could be an opt-in rule in the future as part of a type-aware linter if there's really demand for it, but definitely not something we should spend time on right now IMO.

I would flip it around and say, if you don't want your descriptor to be shadowable, you should define a `__set__` method on it (even if it just raises an exception, like a `@property` without a setter does), and then it will be a data descriptor and not shadowable.

---

_@carljm reviewed on 2025-02-20 23:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:391 on 2025-02-20 23:43_

Yes, I first noticed @sharkdp using this trick in the documentation/tests around our "union with Unknown" behavior.

---

_@AlexWaygood reviewed on 2025-02-21 12:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:1 on 2025-02-21 12:01_

> I would flip it around and say, if you don't want your descriptor to be shadowable, you should define a `__set__` method on it (even if it just raises an exception, like a `@property` without a setter does), and then it will be a data descriptor and not shadowable.

Yes, fair enough -- that's probably the better way to think about it. We can leave this for now, then. Thanks!

I guess the way to signal to a type checker that a descriptor should be read-only might be to set `__set__` to `None`? Since annotating a `__set__` method as returning `Never` just leads to confusing diagnostics about dead code later on in the program, rather than an error when the descriptor attribute is actually assigned to.

---

_@carljm reviewed on 2025-02-21 17:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:1 on 2025-02-21 17:28_

Yeah, that could work. Though we can probably also special-case a return type of `Never` with a better diagnostic in some cases?

---
