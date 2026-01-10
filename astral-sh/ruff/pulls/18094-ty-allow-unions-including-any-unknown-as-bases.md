```yaml
number: 18094
title: "[ty] Allow unions including `Any`/`Unknown` as bases"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/unions-as-bases
created_at: 2025-05-14T14:39:14Z
updated_at: 2025-05-16T04:57:28Z
url: https://github.com/astral-sh/ruff/pull/18094
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Allow unions including `Any`/`Unknown` as bases

---

_Pull request opened by @sharkdp on 2025-05-14 14:39_

## Summary

Alternative fix for https://github.com/astral-sh/ty/issues/312

## Test Plan

New Markdown test

---

_Label `ty` added by @sharkdp on 2025-05-14 14:39_

---

_Comment by @github-actions[bot] on 2025-05-14 14:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
+ warning[unused-ignore-comment] pyinstrument/middleware.py:37:45: Unused blanket `type: ignore` directive
- error[invalid-base] pyinstrument/vendor/decorator.py:282:22: Invalid class base with type `<class '_GeneratorContextManager'> | Unknown` (all bases must be a class, `Any`, `Unknown` or `Todo`)

kornia (https://github.com/kornia/kornia)
- error[invalid-base] kornia/feature/dedode/transformer/layers/swiglu_ffn.py:62:22: Invalid class base with type `Unknown | <class 'SwiGLUFFN'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 971 diagnostics
+ Found 970 diagnostics

kopf (https://github.com/nolar/kopf)
- error[invalid-base] kopf/_core/actions/loggers.py:109:20: Invalid class base with type `Unknown | <class 'LoggerAdapter[Any]'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] kopf/_core/engines/indexing.py:237:23: Invalid class base with type `Unknown | <class 'Mapping[str, Index[Any, Any]]'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 183 diagnostics
+ Found 181 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- error[invalid-base] pybind11/setup_helpers.py:89:25: Invalid class base with type `Unknown | <class 'Extension'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 261 diagnostics
+ Found 260 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[invalid-base] tests/ignite/engine/__init__.py:68:25: Invalid class base with type `Unknown | <class 'IterableDataset'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 2263 diagnostics
+ Found 2262 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[invalid-base] tornado/curl_httpclient.py:582:17: Invalid class base with type `Unknown | <class 'HTTPClientError'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tornado/simple_httpclient.py:44:24: Invalid class base with type `Unknown | <class 'HTTPClientError'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tornado/simple_httpclient.py:60:29: Invalid class base with type `Unknown | <class 'HTTPClientError'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 511 diagnostics
+ Found 508 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-base] pwndbg/aglib/heap/structs.py:247:27: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:310:27: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:371:27: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:454:19: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:492:22: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:531:38: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:550:38: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:581:27: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:596:27: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:625:25: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:678:25: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:736:25: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:786:25: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/aglib/heap/structs.py:850:25: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/lib/elftypes.py:278:18: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/lib/elftypes.py:297:18: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/lib/elftypes.py:316:18: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] pwndbg/lib/elftypes.py:329:18: Invalid class base with type `Unknown | <class 'LittleEndianStructure'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 2239 diagnostics
+ Found 2221 diagnostics

mypy (https://github.com/python/mypy)
- error[invalid-base] mypy/test/meta/test_diff_helper.py:6:23: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/meta/test_parse_data.py:14:26: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/meta/test_update_data.py:19:23: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testapi.py:10:16: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testargs.py:18:16: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testconstraints.py:9:24: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testgraph.py:20:18: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testinfer.py:14:32: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testinfer.py:151:32: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testinfer.py:209:38: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testmodulefinder.py:13:25: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testmodulefinder.py:140:37: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testreports.py:18:28: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testsolve.py:12:18: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testsubtypes.py:10:22: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testtypes.py:59:18: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testtypes.py:244:20: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testtypes.py:695:17: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testtypes.py:1106:17: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testtypes.py:1367:21: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testtypes.py:1410:33: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] mypy/test/testtypes.py:1452:36: Invalid class base with type `Unknown | <class 'TestCase'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 3360 diagnostics
+ Found 3338 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ warning[unused-ignore-comment] pymongo/asynchronous/encryption.py:134:48: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] pymongo/synchronous/encryption.py:133:43: Unused blanket `type: ignore` directive
- Found 555 diagnostics
+ Found 557 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[invalid-base] tools/mock-meta.py:191:20: Invalid class base with type `Unknown | <class 'HTTPServer'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tools/mock-meta.py:339:18: Invalid class base with type `Unknown | <class 'BaseHTTPRequestHandler'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 746 diagnostics
+ Found 744 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-base] ddtrace/_trace/product.py:17:15: Invalid class base with type `Unknown | <class 'Env'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] ddtrace/appsec/_iast/_taint_tracking/_vendor/pybind11/pybind11/setup_helpers.py:89:25: Invalid class base with type `Unknown | <class 'Extension'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] ddtrace/contrib/internal/flask_cache/patch.py:66:23: Invalid class base with type `(Unknown & ~None) | Unknown` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] ddtrace/vendor/psutil/_compat.py:60:34: Invalid class base with type `Unknown | <class 'Exception'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/contrib/django/middleware.py:12:32: Invalid class base with type `Unknown | <class 'object'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/contrib/django/middleware.py:17:36: Invalid class base with type `Unknown | <class 'object'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/contrib/django/middleware.py:22:40: Invalid class base with type `Unknown | <class 'object'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 8710 diagnostics
+ Found 8703 diagnostics

sympy (https://github.com/sympy/sympy)
- error[invalid-base] sympy/combinatorics/coset_table.py:12:18: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/combinatorics/fp_groups.py:50:15: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/combinatorics/fp_groups.py:551:18: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/combinatorics/free_groups.py:114:17: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/combinatorics/free_groups.py:350:37: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/combinatorics/pc_groups.py:7:23: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/combinatorics/pc_groups.py:42:17: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/parsing/latex/lark/transformer.py:26:28: Invalid class base with type `Unknown | <class 'Transformer'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/polys/agca/extensions.py:11:39: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/polys/fields.py:103:17: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/polys/fields.py:292:34: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/polys/rings.py:196:16: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/polys/rings.py:586:34: Invalid class base with type `Unknown | <class 'Printable'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] sympy/utilities/matchpy_connector.py:124:21: Invalid class base with type `Unknown | <class 'Wildcard'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 17176 diagnostics
+ Found 17162 diagnostics

scipy (https://github.com/scipy/scipy)
- error[invalid-base] scipy/_lib/decorator.py:254:22: Invalid class base with type `<class '_GeneratorContextManager'> | Unknown` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 7843 diagnostics
+ Found 7842 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- error[invalid-base] misc/python/materialize/mz_version.py:26:24: Invalid class base with type `<class 'Version'> | Unknown` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 3279 diagnostics
+ Found 3278 diagnostics

```
</details>


---

_Comment by @carljm on 2025-05-15 14:50_

The primer results (and the failing mdtests) look pretty good here?

---

_Comment by @AlexWaygood on 2025-05-15 14:52_

My first attempt at implementing MRO resolution tried hard to allow unions as base classes. It's quite difficult to do correctly. You have to fork the MRO resolution logic at several points. Any given class could now have several possible MROs, which would all need to be considered when doing attribute lookups, etc.

It's possible it's something we could revisit, but at the time we decided to park it for now and go with a simpler solution until it was clear we needed it. See https://github.com/astral-sh/ruff/pull/13722

---

_Comment by @AlexWaygood on 2025-05-15 14:55_

> The primer results (and the failing mdtests) look pretty good here?

_Do_ the failing mdtests look good? Here's our behaviour on `main` for one of them:

```py
def returns_bool() -> bool:
    return True

class A: ...
class B: ...

if returns_bool():
    x = A
else:
    x = B

reveal_type(x)  # revealed: <class 'A'> | <class 'B'>

# error: 11 [invalid-base] "Invalid class base with type `<class 'A'> | <class 'B'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)"
class Foo(x): ...
reveal_type(Foo.__mro__)  # revealed: tuple[<class 'Foo'>, Unknown, <class 'object'>]
```

And here's our behaviour with this PR:

```py
def returns_bool() -> bool:
    return True

class A: ...
class B: ...

if returns_bool():
    x = A
else:
    x = B

reveal_type(x)  # revealed: <class 'A'> | <class 'B'>

class Foo(x): ...
reveal_type(Foo.__mro__)  # revealed: tuple[<class 'Foo'>, <class 'A'>, <class 'object'>]
```

It's great that the `[invalid-base]` diagnostic is going away, but the MRO inference on the final line is definitely incorrect. It should be `tuple[<class 'Foo'>, <class 'A'>, <class 'object'>] | tuple[<class 'Foo'>, <class 'B'>, <class 'object'>]`

---

_Comment by @carljm on 2025-05-15 15:02_

Hmm, I guess I didn't look closely enough at all the failing mdtests :) The intent of this PR as I understood it was only to handle unions with Unknown, which shouldn't affect the test you showed.

---

_Comment by @sharkdp on 2025-05-15 15:29_

> The intent of this PR as I understood it was only to handle unions with Unknown, which shouldn't affect the test you showed.

Yes, exactly. I was trying to find a solution for [this use case](https://play.ty.dev/79aa2c89-ec65-438f-91e6-81d9a6341275):
```py
try:
    from some_optional_module import BaseClass
except ImportError:
    from module import BaseClass

class A(BaseClass):  # Invalid class base with type `Unknown | <class 'BaseClass'>`
    pass
```

I'm happy to look further into this, if we think that it's reasonable to create a special case for unions with exactly two elements, one of them `Unknown`?

Even if we are, it feels like this could potentially create false positives (because we would basically ignore that `| Unknown` part)?

---

_Comment by @AlexWaygood on 2025-05-15 15:35_

I guess the way to preserve the gradual guarantee might be to turn `Foo | Unknown` into `Unknown` rather than turning `Foo | Unknown` into `Foo`? A class with `Unknown` in its MRO will produce fewer cascading errors later on than a class with `Foo` in its MRO (at least theoretically), since all attributes are available on a class that has `Unknown` in its MRO

---

_Renamed from "[ty] Experiment: allow `<class 'C'> | Unknown` as a base" to "[ty] Allow unions including `Any`/`Unknown` as bases" by @sharkdp on 2025-05-15 19:19_

---

_@sharkdp reviewed on 2025-05-15 19:21_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/mro.md_-_Method_Resolution_Or…_-_`__bases__`_lists_wi…_(ea7ebc83ec359b54).snap`:144 on 2025-05-15 19:21_

Not sure why this snapshot here changed.

---

_@sharkdp reviewed on 2025-05-15 19:29_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/mro.md_-_Method_Resolution_Or…_-_`__bases__`_lists_wi…_(ea7ebc83ec359b54).snap`:144 on 2025-05-15 19:29_

Probably because the iteration order of that hashmap here is not deterministic?
```rs
let mut base_to_indices: FxHashMap<ClassBase<'db>, Vec<usize>>
```

---

_Marked ready for review by @sharkdp on 2025-05-15 19:30_

---

_Review requested from @carljm by @sharkdp on 2025-05-15 19:30_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-15 19:30_

---

_Review requested from @dcreager by @sharkdp on 2025-05-15 19:30_

---

_@AlexWaygood reviewed on 2025-05-15 20:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/mro.md_-_Method_Resolution_Or…_-_`__bases__`_lists_wi…_(ea7ebc83ec359b54).snap`:144 on 2025-05-15 20:57_

Oh, that's interesting. Maybe we should be using an `IndexMap` there. The order changed in https://github.com/astral-sh/ruff/pull/18060 as well.

---

_Comment by @carljm on 2025-05-15 21:02_

> I guess the way to preserve the gradual guarantee might be to turn `Foo | Unknown` into `Unknown` rather than turning `Foo | Unknown` into `Foo`? A class with `Unknown` in its MRO will produce fewer cascading errors later on than a class with `Foo` in its MRO (at least theoretically), since all attributes are available on a class that has `Unknown` in its MRO

Isn't that basically what we already do? We consider inheriting from a union an error and construct an error MRO with `Unknown` in it?

---

_Comment by @AlexWaygood on 2025-05-15 21:04_

> Isn't that basically what we already do? We consider inheriting from a union an error and construct an error MRO with `Unknown` in it?

Correct! But the difference is that we want to avoid emitting an error here, I think, in order to preserve the gradual guarantee, since for a union `A | Unknown`, the `Unknown` could materialize to `A`, which would mean the union would simplify to `A`, which would not be an error.

---

_@AlexWaygood reviewed on 2025-05-15 21:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:151 on 2025-05-15 21:08_

I think we should still emit an error if it's a union with three or more elements. For a union `A | B | Unknown`, the type will still be a union type whatever the `Unknown` element materializes to. That means that whatever the `Unknown` element materialized to, we wouldn't be able to infer a correct MRO with our current capabilities, and I think that's something that most developers would want their type checker to warn them about, since it severely limits our ability to make accurate inferences about their code.

Test case:

```py
from typing import Any

class Foo: ...
class Bar: ...

def f(a: bool, b: bool, c: Any):
    if a:
        x = Foo
    elif b:
        x = Bar
    else:
        x = c

    class Baz(x): ...
```

---

_Comment by @AlexWaygood on 2025-05-15 21:10_

Note that this PR treats inhabitants of `Any` differently from the `Any` symbol itself. I.e. we still emit an error on this with this PR branch:

```py
from typing import Any

class Foo: ...

def f(x: bool):
    if x:
        y = Any
    else:
        y = Foo

    class Bar(y): ...
```

I think that's defensible, just wanted to note it!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class_base.rs`:151 on 2025-05-16 02:49_

> For a union `A | B | Unknown`, the type will still be a union type whatever the `Unknown` element materializes to.

Not if it materializes to `object`?

---

_@carljm reviewed on 2025-05-16 02:49_

---

_@carljm approved on 2025-05-16 02:50_

This looks good to me, thank you!

---

_@AlexWaygood reviewed on 2025-05-16 03:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:151 on 2025-05-16 03:02_

If the `Unknown` element materialises to "instance of `object`" then the union could simplify to the type "instance of `object`", yes, but that would still be an invalid class base. In order for it to be a valid class base, the union would have to simplify to "the class object `object`". I don't think that's possible?

---

_@carljm reviewed on 2025-05-16 03:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class_base.rs`:151 on 2025-05-16 03:08_

Ah, that's a good point; we have to inherit from a class literal type, which is a singleton type, so there's no way it could collapse a union to a valid base. So I think you're right that we could error on a union containing Any/Unknown if it has two other elements.

---

_Merged by @sharkdp on 2025-05-16 04:57_

---

_Closed by @sharkdp on 2025-05-16 04:57_

---

_Branch deleted on 2025-05-16 04:57_

---
