```yaml
number: 19187
title: "[ty] Do not consider a type `T` to satisfy a method member on a protocol unless the method is available on the meta-type of `T`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/method-metatype
created_at: 2025-07-07T17:20:12Z
updated_at: 2025-07-25T10:16:08Z
url: https://github.com/astral-sh/ruff/pull/19187
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Do not consider a type `T` to satisfy a method member on a protocol unless the method is available on the meta-type of `T`

---

_Pull request opened by @AlexWaygood on 2025-07-07 17:20_

## Summary

A callable instance attribute is not sufficient for a type to satisfy a protocol with a method member: a method member specified by a protocol `P` must exist on the *meta-type* of `T` for `T` to be a subtype of `P`:

```py
from typing import Callable, Protocol
from ty_extensions import static_assert, is_assignable_to

class SupportsFooMethod(Protocol):
    def foo(self): ...

class SupportsFooAttr(Protocol):
    foo: Callable[..., object]

class Foo:
    def __init__(self):
        self.foo: Callable[..., object] = lambda *args, **kwargs: None

static_assert(not is_assignable_to(Foo, SupportsFooMethod))
static_assert(is_assignable_to(Foo, SupportsFooAttr))
```

There are several reasons why we must enforce this rule:
1. Some methods, such as dunder methods, are always looked up on the class directly. If a class with an `__iter__` instance attribute satisfied the `Iterable` protocol, for example, the `Iterable` protocol would not accurately describe the requirements Python has for a class to be iterable at runtime. We _could_ apply different rules for dunder method members as opposed to non-dunder method members, but I worry that this would appear inconsistent and would confuse users.
2. Allowing callable instance attributes to satisfy method members of protocols would make `issubclass()` narrowing of runtime-checkable protocols unsound, as the `issubclass()` mechanism at runtime for protocols only checks whether a method is accessible on the class object, not the instance. (Protocols with non-method members cannot be passed to `issubclass()` at all at runtime.)
3. If we allowed an instance-only attribute to satisfy a method member on a protocol, it would make `type[]` types unsound for protocols. For example, this currently type-checks fine on `main`, but it crashes at runtime; under this PR, it no longer type-checks:

   ```py
   from typing import Protocol
	
   class Foo(Protocol):
       def method(self) -> str: ...
	
	   def f(x: Foo):
	       type(x).method
	
   class Bar:
	   def __init__(self):
	       self.method = lambda: "foo"
	
   f(Bar())
   ```

Enforcing this rule fixes https://github.com/astral-sh/ty/issues/764, because the inconsistency between our understanding of `Iterable` assignability and types that `Type::try_iterate()` returned `Ok()` for is now fixed. Many types that we previously incorrectly considered assignable to `Iterable` are now no longer considered assignable to `Iterable`.

## Test plan

- I added mdtests to `protocol.md` explaining and enforcing this rule
- I added a corpus test for https://github.com/astral-sh/ty/issues/764 that ensures that the crash is fixed
- In a PR based on top of this one (https://github.com/astral-sh/ruff/pull/19208), I moved `zope.interface` to the `good.txt` list in mypy_primer, and confirmed that this PR means we no longer crash when analyzing that codebase. (Previously, we crashed due to https://github.com/astral-sh/ty/issues/764.)
- I analyzed the mypy_primer report in https://github.com/astral-sh/ruff/pull/19187#issuecomment-3052558437. Overall it LGTM.
- The fixes in this PR allow us to stabilise the property test added in https://github.com/astral-sh/ruff/pull/19186. I checked that it _is_ stable locally by running `QUICKCHECK_TESTS=1000000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable`
- I also added some failing tests regarding class-literal types where a class has `Any` or `Unknown` in its MRO. I think there's an argument that these _should_ be considered `Iterable` actually, but _not_ because they might have an `__iter__` instance attribute. Rather, the metaclass of such a class might define an `__iter__` method, which would make all classes with that metaclass iterable. We can't come to any firm conclusion about what the metaclass of a class with `Any` in its MRO might be, because the `Any` might materialize to a type with a custom metaclass.

  For now, I defer the question of fixing this, but I have a draft PR open to explore fixing it: https://github.com/astral-sh/ruff/pull/19157. It has some... surprising primer results. I need to do some more digging there to figure out what's going on. I suspect that there is yet another underlying bug being uncovered there. 😆

## Performance

~~There's a performance regression on this PR, but not a huge one. I think that's sort-of unavoidable; we're again just doing slightly more work than we did before. https://github.com/astral-sh/ruff/pull/19230 will more than reclaim the performance hit here, though, if that PR is accepted.~~

After rebasing on #19230, this PR now shows speedups of 3% on some benchmarks, and slowdowns of 2% on others. Overall the picture looks pretty positive to me, but it's hard to make out much signal here: https://codspeed.io/astral-sh/ruff/branches/alex%2Fmethod-metatype?runnerMode=Instrumentation


---

_Comment by @AlexWaygood on 2025-07-07 17:43_

The post-the-primer-comment CI job isn't working at the moment, but here's the primer diff:

```diff
itsdangerous (https://github.com/pallets/itsdangerous)
+ error[invalid-assignment] src/itsdangerous/serializer.py:95:5: Object of type `<module 'json'>` is not assignable to `_PDataSerializer[Any]`
+ error[invalid-assignment] src/itsdangerous/url_safe.py:21:5: Object of type `<class '_CompactJSON'>` is not assignable to `_PDataSerializer[str]`
- Found 6 diagnostics
+ Found 8 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-argument-type] tests/test_test.py:438:36: Argument to bound method `add_file` is incorrect: Expected `str | PathLike[str] | IO[bytes]`, found `SpecialInput`
+ error[invalid-argument-type] tests/test_test.py:438:36: Argument to bound method `add_file` is incorrect: Expected `str | PathLike[str] | IO[bytes] | FileStorage`, found `SpecialInput`

pwndbg (https://github.com/pwndbg/pwndbg)
+ error[unsupported-operator] pwndbg/aglib/nearpc.py:304:24: Operator `*` is unsupported between objects of type `Unknown | Parameter` and `Literal[" "]`
+ error[unsupported-operator] pwndbg/commands/context.py:1189:39: Operator `*` is unsupported between objects of type `Literal[" "]` and `Parameter`
- Found 2273 diagnostics
+ Found 2275 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ error[invalid-argument-type] tests/scripts/pipeline-demo.py:190:38: Argument to function `_pipeline_communicate` is incorrect: Expected `PGconn`, found `LoggingPGconn`
+ error[invalid-argument-type] tests/scripts/pipeline-demo.py:212:38: Argument to function `_pipeline_communicate` is incorrect: Expected `PGconn`, found `LoggingPGconn`
- Found 561 diagnostics
+ Found 563 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[invalid-argument-type] tests/test_utils_spider.py:20:17: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `AsyncGenerator[Unknown, None]`
- Found 1100 diagnostics
+ Found 1099 diagnostics
```

---

_Label `ty` added by @AlexWaygood on 2025-07-07 17:43_

---

_Comment by @github-actions[bot] on 2025-07-08 10:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
itsdangerous (https://github.com/pallets/itsdangerous)
+ error[invalid-assignment] src/itsdangerous/serializer.py:95:5: Object of type `<module 'json'>` is not assignable to `_PDataSerializer[Any]`
+ error[invalid-assignment] src/itsdangerous/url_safe.py:21:5: Object of type `<class '_CompactJSON'>` is not assignable to `_PDataSerializer[str]`
- Found 6 diagnostics
+ Found 8 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ error[invalid-argument-type] tests/scripts/pipeline-demo.py:190:38: Argument to function `_pipeline_communicate` is incorrect: Expected `PGconn`, found `LoggingPGconn`
+ error[invalid-argument-type] tests/scripts/pipeline-demo.py:212:38: Argument to function `_pipeline_communicate` is incorrect: Expected `PGconn`, found `LoggingPGconn`
- Found 669 diagnostics
+ Found 671 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[invalid-argument-type] tests/test_utils_spider.py:20:17: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `AsyncGenerator[Unknown, None]`
- Found 1099 diagnostics
+ Found 1098 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-argument-type] tests/test_test.py:438:36: Argument to bound method `add_file` is incorrect: Expected `str | PathLike[str] | IO[bytes]`, found `SpecialInput`
+ error[invalid-argument-type] tests/test_test.py:438:36: Argument to bound method `add_file` is incorrect: Expected `str | PathLike[str] | IO[bytes] | FileStorage`, found `SpecialInput`

pwndbg (https://github.com/pwndbg/pwndbg)
+ error[unsupported-operator] pwndbg/aglib/nearpc.py:304:24: Operator `*` is unsupported between objects of type `Unknown | Parameter` and `Literal[" "]`
+ error[unsupported-operator] pwndbg/commands/context.py:1189:39: Operator `*` is unsupported between objects of type `Literal[" "]` and `Parameter`
- Found 2275 diagnostics
+ Found 2277 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~25MB
+     struct metadata = ~23MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-07-08 10:19_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmethod-metatype?runnerMode=WallTime)

### Merging #19187 will **not alter performance**

<sub>Comparing <code>alex/method-metatype</code> (25b9d54) with <code>main</code> (59aa869)</sub>



### Summary

`✅ 7` untouched benchmarks  





---

_Comment by @AlexWaygood on 2025-07-09 12:50_

## Primer analysis

### `psycopg`

> ```diff
> psycopg (https://github.com/psycopg/psycopg)
> + error[invalid-argument-type] tests/scripts/pipeline-demo.py:190:38: Argument to function `_pipeline_communicate` is incorrect: Expected `PGconn`, found `LoggingPGconn`
> + error[invalid-argument-type] tests/scripts/pipeline-demo.py:212:38: Argument to function `_pipeline_communicate` is incorrect: Expected `PGconn`, found `LoggingPGconn`
> ```

The `PGConn` protocol is [here](https://github.com/psycopg/psycopg/blob/ce085bc927d82526dc19362ea1a66f460374aff0/psycopg/psycopg/pq/abc.py#L22), and `LoggingPGConn` is [here](https://github.com/psycopg/psycopg/blob/ce085bc927d82526dc19362ea1a66f460374aff0/tests/scripts/pipeline-demo.py#L34). Previously we considered `LoggingPGConn` a subtype of `PGConn` because of [`LoggingPGConn.__getattr__`](https://github.com/psycopg/psycopg/blob/ce085bc927d82526dc19362ea1a66f460374aff0/tests/scripts/pipeline-demo.py#L63-L64), which means that arbitrary attributes are available on _instances_ of `LoggingPGConn`. However, it does not mean that arbitrary attributes are available on the class `LoggingPGConn` itself:

```pycon
>>> class Foo:
...     def __getattr__(self, attr): return Foo()
...     
>>> f = Foo()
>>> f.__iter__
<__main__.Foo object at 0x10320aad0>
>>> Foo.__iter__
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    Foo.__iter__
AttributeError: type object 'Foo' has no attribute '__iter__'. Did you mean: '__str__'?
>>> for x in f:
...     pass
...     
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    for x in f:
             ^
TypeError: 'Foo' object is not iterable
```

The `PGConn` protocol does not actually have any dunder methods in its interface, and the distinction here _is_ most important for dunder methods, since dunder methods are always looked up on the class object rather than the instance. The protocol is also not decorated with `@runtime_checkable`, so concerns about the soundness of `issubclass()` narrowing also don't apply. This therefore feels a little unfortunate: it would arguably be sound in mosrt cases to consider `LoggingPGConn` a subtype of `PGConn` in this instance. Still, I don't really like the idea of applying different rules to how subtyping of protocol method members works depending on whether the protocol method member is a dunder method and whether the protocol is decorated with `@runtime_checkable`. That feels like it would be inconsistent and confusing. Considering instance attributes sufficient to satisfy method members would also make `type[P]` unsound if `P` was a protocol type: if a method is available on instances of `P`, it should be available as a function instance-attribute on the meta-type of any instance of `P`.

---

### `pwndbg`

> ```diff
> pwndbg (https://github.com/pwndbg/pwndbg)
> + error[unsupported-operator] pwndbg/aglib/nearpc.py:304:24: Operator `*` is unsupported between objects of type `Unknown | Parameter` and `Literal[" "]`
> + error[unsupported-operator] pwndbg/commands/context.py:1189:39: Operator `*` is unsupported between objects of type `Literal[" "]` and `Parameter`
> ```

This one looks like a true positive! The hits in question are lines like [this](https://github.com/pwndbg/pwndbg/blob/7cec1187712e1729c2f2ae8793253b3466a2f6a8/pwndbg/aglib/nearpc.py#L304-L306). The `opcode_separator_bytes` variable on that line is defined [here](https://github.com/pwndbg/pwndbg/blob/7cec1187712e1729c2f2ae8793253b3466a2f6a8/pwndbg/aglib/nearpc.py#L84C1-L89) -- we can see that it's the result of a call to `pwndbg.config.add_param()`. `pwndbg.config` [is a `pwndbg.config.Config` instance](https://github.com/pwndbg/pwndbg/blob/7cec1187712e1729c2f2ae8793253b3466a2f6a8/pwndbg/__init__.py#L6), and `pwndbg.config.Config.add_param()` [returns an instance of `pwndbg.config.Parameter`](https://github.com/pwndbg/pwndbg/blob/7cec1187712e1729c2f2ae8793253b3466a2f6a8/pwndbg/lib/config.py#L224-L234) -- the TL;DR here is that the `opcode_separator_bytes` variable is an instance of `pwndbg.config.Parameter`, so the question here is whether we should emit an error when we see a `Parameter` instance being multiplied by a string literal.

I think the answer to that is "yes, we should". The `*` operation could go via `Parameter.__mul__` or `str.__rmul__` -- `Parameter.__mul__` [only accepts `int`s](https://github.com/pwndbg/pwndbg/blob/7cec1187712e1729c2f2ae8793253b3466a2f6a8/pwndbg/lib/config.py#L197-L198) (a string literal type is not a subtype of `int`), and `str.__rmul__` [only accepts instances of the `SupportsIndex` protocol](https://github.com/python/typeshed/blob/9317dc62bd4fb46b8b48ce5353286cab80308d47/stdlib/builtins.pyi#L638-L641). Currently we consider that `Parameter` satisfies the `SupportsIndex` protocol because it has a `__getattr__` method, but that's incorrect: `__getattr__` is only called when looking up attributes on instances, and dunder methods such as `__index__` are looked up directly on the class object:

```pycon
>>> class Foo:
...     def __init__(self):
...         self.__index__ = lambda self: 42
...         
>>> int(Foo())
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    int(Foo())
    ~~~^^^^^^^
TypeError: int() argument must be a string, a bytes-like object or a real number, not 'Foo'
>>> class Foo:
...     __index__ = lambda self: 42
...     
>>> int(Foo())
42
```

---

### `itsdangerous`

```diff
itsdangerous (https://github.com/pallets/itsdangerous)
+ error[invalid-assignment] src/itsdangerous/serializer.py:95:5: Object of type `<module 'json'>` is not assignable to `_PDataSerializer[Any]`
+ error[invalid-assignment] src/itsdangerous/url_safe.py:21:5: Object of type `<class '_CompactJSON'>` is not assignable to `_PDataSerializer[str]`
```

These are similar cases to the `psycopg` hits: considering `<module 'json'>` or `<class '_CompactJson'>` assignable of `_PDataSerializer[str]` would probably not be unsafe in most cases, so this PR probably leads to a usability regression for this kind of scenario. In order for `<module json>` to be considered assignable to this protocol under the new rules in this PR, you would have to rewrite the protocol like this:

```diff
  class _PDataSerializer(t.Protocol[_TSerialized]):
-     def loads(self, payload: _TSerialized, /) -> t.Any: ...
+     @property
+     def loads(self) -> t.Callable[[_TSerialized], Any]: ...
      # A signature with additional arguments is not handled correctly by type
      # checkers right now, so an overload is used below for serializers that
      # don't match this strict protocol.
-     def dumps(self, obj: t.Any, /) -> _TSerialized: ...
+     @property
+     def dumps(self) -> Callable[[t.Any], _TSerialized]: ...
```

---

### `scrapy`

```diff
scrapy (https://github.com/scrapy/scrapy)
- error[invalid-argument-type] tests/test_utils_spider.py:20:17: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `AsyncGenerator[Unknown, None]`
```

This is a false positive going away. The line we emit the error on is [here](https://github.com/scrapy/scrapy/blob/6b2997af9000bbf9114aca681b62713c30cc628e/tests/test_utils_spider.py#L20); the reason why we previously emitted the error is that we thought that the `iterate_spider_output` call returned an instance of `Deferred[T]`, and we thought that an instance of `Deferred[T]` wasn't a good type to pass to the `list()` constructor. The reason why we thought the `iterate_spider_output` returned a `Deferred[T]` instance was that we were picking the [first overload here](https://github.com/scrapy/scrapy/blob/6b2997af9000bbf9114aca681b62713c30cc628e/scrapy/commands/parse.py#L138-L144) -- we picked that overload because we thought that the [`Item`](https://github.com/scrapy/scrapy/blob/6b2997af9000bbf9114aca681b62713c30cc628e/scrapy/item.py#L57) type satisifed the `AsyncGenerator` protocol. We came to that incorrect conclusion because of the [`Item.__getattr__`](https://github.com/scrapy/scrapy/blob/6b2997af9000bbf9114aca681b62713c30cc628e/scrapy/item.py#L103-L106) method, but now we correctly see that as insufficient, which leads us to correctly pick the second overload of `iterate_spider_output`, causing us to infer the result of the `iterate_spider_output` call as `Iterable[Any]`, which is obviously acceptable as an argument to pass to the `list()` constructor.

---

_Marked ready for review by @AlexWaygood on 2025-07-09 16:00_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-09 16:00_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-09 16:00_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-09 16:00_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-10 07:06_

---

_Comment by @github-actions[bot] on 2025-07-10 07:14_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 1 | 1 |
| `invalid-assignment` | 2 | 0 | 0 |
| `unsupported-operator` | 2 | 0 | 0 |
| **Total** | **4** | **1** | **1** |

**[Full report with detailed diff](https://alex-method-metatype.ecosystem-663.pages.dev/diff)**


---

_Comment by @AlexWaygood on 2025-07-10 08:58_

(I updated my description in the PR summary of the performance characteristics of this PR, which are now pretty different after rebasing it on top of #19230)

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-10 09:00_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-10 09:00_

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-15 09:29_

---

_Comment by @carljm on 2025-07-21 21:11_

Sorry that I didn't get to this sooner! I never quite got to the bottom of my post-vacation review queue last week, and unfortunately I was tackling the notifications top-down, so this PR suffered from not being noisy enough, recently enough. Thanks for the reminder!

Neither mypy nor pyright enforce this rule; they seem to instead avoid the `__getattr__` issues by never allowing a `__getattr__` to factor into protocol assignability.

After reading through the description here, and the ecosystem analysis, I admit I am not convinced that we should enforce this rule, either. The ecosystem false positives on psycopg and itsdangerous seem really problematic to me, and I don't find the arguments presented in favor of enforcing this rule convincing. Specifically, I think that both `type[Proto]` and `@runtime_checkable` enforcement are so deeply unsound to begin with, that this issue barely registers a noticeable difference in their soundness. And I am also not convinced that distinguishing dunder methods from non-dunder methods will be too confusing to users. We already model this actual difference in how dunders are looked up elsewhere in ty. It's a real runtime behavior difference, and I don't see why we should avoid correctly modeling it here as well.

So my take would be that we should not allow an instance-only attribute to satisfy a dunder-method member of a Protocol, but we should otherwise allow instance-only attributes to satisfy Protocol method members. If I'm reading the issue and the ecosystem analysis correctly, I think that approach would fix all the false negatives fixed by this PR (and would fix the Iterable-assignability problem), without triggering any of the new false positives.

---

_Comment by @AlexWaygood on 2025-07-24 17:36_

(requested ping!)

---

_Comment by @carljm on 2025-07-24 17:38_

Haha I just went and looked it up right away so you wouldn't have to do that, but thank you 😆 Will review it shortly, after I record some of the other conclusions from our discussion so I don't forget them.

---

_@carljm approved on 2025-07-24 23:07_

We discussed this in person, and I'm open to try this approach. If we execute the descriptor protocol on accessing a method from a Protocol type, then it makes sense to require that it exist on the meta-type.

---

_Merged by @AlexWaygood on 2025-07-25 10:16_

---

_Closed by @AlexWaygood on 2025-07-25 10:16_

---

_Branch deleted on 2025-07-25 10:16_

---
