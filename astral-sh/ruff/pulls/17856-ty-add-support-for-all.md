```yaml
number: 17856
title: "[ty] Add support for `__all__`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/dunder-all
created_at: 2025-05-05T13:24:57Z
updated_at: 2025-05-07T16:12:45Z
url: https://github.com/astral-sh/ruff/pull/17856
synced_at: 2026-01-12T15:56:07Z
```

# [ty] Add support for `__all__`

---

_@dhruvmanila_

## Summary

This PR adds support for the `__all__` module variable.

Reference spec: https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols

This PR adds a new `dunder_all_names` query that returns a set of `Name`s defined in the `__all__` variable of the given `File`. The query works by implementing the `StatementVisitor` and collects all the names by recognizing the supported idioms as mentioned in the spec. Any idiom that's not recognized are ignored.

The current implementation is minimum to what's required for us to remove all the false positives that this is causing. Refer to the "Follow-ups" section below to see what we can do next. I'll a open separate issue to keep track of them.

Closes: astral-sh/ty#106 
Closes: astral-sh/ty#199

### Follow-ups

* Diagnostics:
  * Add warning diagnostics for unrecognized `__all__` idioms, `__all__` containing non-string element
  * Add an error diagnostic for elements that are present in `__all__` but not defined in the module. This could lead to runtime error
* Maybe we should return `<type>` instead of `Unknown | <type>` for `module.__all__`. For example: https://playknot.ruff.rs/2a6fe5d7-4e16-45b1-8ec3-d79f2d4ca894
* Mark a symbol that's mentioned in `__all__` as used otherwise it could raise (possibly in the future) "unused-name" diagnostic

Supporting diagnostics will require that we update the return type of the query to be something other than `Option<FxHashSet<Name>>`, something that behaves like a result and provides a way to check whether a name exists in `__all__`, loop over elements in `__all__`, loop over the invalid elements, etc.

## Ecosystem analysis

The following are the maximum amount of diagnostics **removed** in the ecosystem:

* "Type <module '...'> has no attribute ..."
    * `collections.abc` - 14
    * `numpy` - 35534
    * `numpy.ma` - 296
    * `numpy.char` - 37
    * `numpy.testing` - 175
    * `hashlib` - 311
    * `scipy.fft` - 2
    * `scipy.stats` - 38
* "Module '...' has no member ..."
    * `collections.abc` - 85
    * `numpy` - 508
    * `numpy.testing` - 741
    * `hashlib` - 36
    * `scipy.stats` - 68
    * `scipy.interpolate` - 7
    * `scipy.signal` - 5

The following modules have dynamic `__all__` definition, so `ty` assumes that `__all__` doesn't exists in that module:
* `scipy.stats` (https://github.com/scipy/scipy/blob/95a5d6ea8b9f2e482f9a79aa40b719433f8e3c4d/scipy/stats/__init__.py#L665)
* `scipy.interpolate` (https://github.com/scipy/scipy/blob/95a5d6ea8b9f2e482f9a79aa40b719433f8e3c4d/scipy/interpolate/__init__.py#L221)
* `scipy.signal` (indirectly via https://github.com/scipy/scipy/blob/95a5d6ea8b9f2e482f9a79aa40b719433f8e3c4d/scipy/signal/_signal_api.py#L30)
* `numpy.testing` (https://github.com/numpy/numpy/blob/de784cd6ee53ffddf07e0b3f89bf22bda9fd92fb/numpy/testing/__init__.py#L16-L18)

~There's this one category of **false positives** that have been added:~ Fixed the false positives by also ignoring `__all__` from a module that uses unrecognized idioms.

<details><summary>Details about the false postivie:</summary>
<p>

The `scipy.stats` module has dynamic `__all__` and it imports a bunch of symbols via star imports. Some of those modules have a mix of valid and invalid `__all__` idioms. For example, in https://github.com/scipy/scipy/blob/95a5d6ea8b9f2e482f9a79aa40b719433f8e3c4d/scipy/stats/distributions.py#L18-L24, 2 out of 4 `__all__` idioms are invalid but currently `ty` recognizes two of them and says that the module has a `__all__` with 5 values. This leads to around **2055** newly added false positives of the form:
```
Type <module 'scipy.stats'> has no attribute ...
```

I think the fix here is to completely ignore `__all__`, not only if there are invalid elements in it, but also if there are unrecognized idioms used in the module.

</p>
</details> 

## Test Plan

Add a bunch of test cases using the new `ty_extensions.dunder_all_names` function to extract a module's `__all__` names.

Update various test cases to remove false positives around `*` imports and re-export convention.

Add new test cases for named import behavior as `*` imports covers all of it already (thanks Alex!).


---

_Label `ty` added by @dhruvmanila on 2025-05-05 13:24_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/dunder_all.rs`:33 on 2025-05-06 16:07_

Nit: Avoid logging the entire file here
```suggestion
        let _span = tracing::trace_span!("dunder_all_names", file=?file.path(db)).entered();
```

---

_@MichaReiser reviewed on 2025-05-06 16:07_

---

_@MichaReiser reviewed on 2025-05-06 16:08_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/dunder_all.rs`:34 on 2025-05-06 16:08_

What's the reason that we need cycle handling for this. Is it called from within the semantic index builder. If not: Where/how does it cross file boundaries.

Oh, is it because we also collect names across modules.

---

_@MichaReiser reviewed on 2025-05-06 16:12_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/dunder_all.rs`:183 on 2025-05-06 16:12_

You could consider implementing `StatementVisitor` instead

---

_@dhruvmanila reviewed on 2025-05-06 16:14_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/dunder_all.rs`:34 on 2025-05-06 16:14_

Star imports require looking up `__all__` names because if a symbol is not defined in the module's `__all__` (if present), then it is unbound. Star imports can be recursive without a runtime error:

https://github.com/astral-sh/ruff/blob/4069b90e3d8ea15a8ae9fc0e16197d86d0fc363f/crates/ty_python_semantic/resources/mdtest/import/star.md#cyclic-star-imports

Regarding the file boundaries, the `__all__` in a file can be conditionally defined and it can even extend another module's `__all__`. The "conditionally defined" part is implemented by way of checking whether it is a statically known branch and what's the truthiness of the test expression. The "extend another module's `__all__`" requires resolving the module and invoking the `dunder_all_names` query on that module. For example:

```py
import sys
import submodule

__all__ = []

if sys.version_info >= (3, 10):
	__all__.extend(submodule.__all__)
else:
	__all__.append("A")

class A: ...
```

---

_@dhruvmanila reviewed on 2025-05-06 16:21_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/dunder_all.rs`:34 on 2025-05-06 16:21_

> Oh, is it because we also collect names across modules.

Yes, that is one of the supported idioms: https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols

---

_Comment by @github-actions[bot] on 2025-05-06 23:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- error[lint:unresolved-attribute] parso/cache.py:265:17: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] parso/grammar.py:47:24: Type `<module 'hashlib'>` has no attribute `sha256`
- Found 85 diagnostics
+ Found 83 diagnostics

beartype (https://github.com/beartype/beartype)
- error[lint:unresolved-import] beartype/_cave/_cavefast.py:65:5: Module `collections.abc` has no member `Set`
- error[lint:unresolved-import] beartype/_check/code/codescope.py:89:29: Module `collections.abc` has no member `Set`
- error[lint:unresolved-import] beartype/_data/cls/datacls.py:40:5: Module `collections.abc` has no member `Set`
- error[lint:unresolved-import] beartype/_util/kind/map/utilmapset.py:26:5: Module `collections.abc` has no member `Set`
- error[lint:unresolved-import] beartype/_util/kind/map/utilmaptest.py:19:5: Module `collections.abc` has no member `Set`
- Found 561 diagnostics
+ Found 556 diagnostics

bidict (https://github.com/jab/bidict)
- error[lint:unresolved-import] bidict/_orderedbidict.py:24:29: Module `collections.abc` has no member `Set`
- Found 17 diagnostics
+ Found 16 diagnostics

git-revise (https://github.com/mystor/git-revise)
- error[lint:unresolved-attribute] gitrevise/merge.py:420:14: Type `<module 'hashlib'>` has no attribute `sha1`
- error[lint:unresolved-attribute] gitrevise/odb.py:81:18: Type `<module 'hashlib'>` has no attribute `sha1`
- Found 16 diagnostics
+ Found 14 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- error[lint:unresolved-attribute] aioredis/client.py:4692:20: Type `<module 'hashlib'>` has no attribute `sha1`
- Found 29 diagnostics
+ Found 28 diagnostics

starlette (https://github.com/encode/starlette)
- error[lint:unresolved-attribute] starlette/responses.py:340:20: Type `<module 'hashlib'>` has no attribute `md5`
- Found 206 diagnostics
+ Found 205 diagnostics

pyjwt (https://github.com/jpadilla/pyjwt)
- error[lint:unresolved-attribute] jwt/algorithms.py:312:37: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] jwt/algorithms.py:313:37: Type `<module 'hashlib'>` has no attribute `sha384`
- error[lint:unresolved-attribute] jwt/algorithms.py:314:37: Type `<module 'hashlib'>` has no attribute `sha512`
- Found 188 diagnostics
+ Found 185 diagnostics

alerta (https://github.com/alerta/alerta)
- error[lint:unresolved-attribute] alerta/utils/key.py:27:82: Type `<module 'hashlib'>` has no attribute `sha256`
- Found 483 diagnostics
+ Found 482 diagnostics

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
- error[lint:unresolved-import] pyi.py:10:70: Module `collections.abc` has no member `Set`
- Found 13 diagnostics
+ Found 12 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- error[lint:unresolved-attribute] dedupe/api.py:1554:22: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] dedupe/canonical.py:19:23: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] dedupe/canonical.py:31:24: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] dedupe/clustering.py:68:32: Type `<module 'numpy'>` has no attribute `unique`
- error[lint:unresolved-attribute] dedupe/clustering.py:73:25: Type `<module 'numpy'>` has no attribute `min`
- error[lint:unresolved-attribute] dedupe/clustering.py:88:25: Type `<module 'numpy'>` has no attribute `searchsorted`
- error[lint:unresolved-attribute] dedupe/clustering.py:142:27: Type `<module 'numpy'>` has no attribute `unique`
- error[lint:unresolved-attribute] dedupe/clustering.py:170:12: Type `<module 'numpy'>` has no attribute `cumsum`
- error[lint:unresolved-attribute] dedupe/clustering.py:170:25: Type `<module 'numpy'>` has no attribute `unique`
- error[lint:unresolved-attribute] dedupe/clustering.py:194:21: Type `<module 'numpy'>` has no attribute `unique`
- error[lint:unresolved-attribute] dedupe/clustering.py:207:27: Type `<module 'numpy'>` has no attribute `ones`
- error[lint:unresolved-attribute] dedupe/clustering.py:277:14: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] dedupe/clustering.py:320:21: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] dedupe/clustering.py:320:33: Type `<module 'numpy'>` has no attribute `roll`
- error[lint:unresolved-attribute] dedupe/clustering.py:321:21: Type `<module 'numpy'>` has no attribute `split`
- error[lint:unresolved-attribute] dedupe/convenience.py:40:24: Type `<module 'numpy'>` has no attribute `arange`
- error[lint:unresolved-attribute] dedupe/convenience.py:43:28: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] dedupe/convenience.py:71:24: Type `<module 'numpy'>` has no attribute `arange`
- error[lint:unresolved-attribute] dedupe/convenience.py:73:24: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] dedupe/convenience.py:75:12: Type `<module 'numpy'>` has no attribute `unravel_index`
- error[lint:unresolved-attribute] dedupe/convenience.py:75:12: Type `<module 'numpy'>` has no attribute `unravel_index`
- error[lint:unresolved-attribute] dedupe/convenience.py:75:12: Type `<module 'numpy'>` has no attribute `unravel_index`
- error[lint:unresolved-attribute] dedupe/convenience.py:86:26: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] dedupe/convenience.py:94:26: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] dedupe/core.py:85:27: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] dedupe/core.py:166:24: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] dedupe/core.py:202:15: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] dedupe/core.py:209:32: Type `<module 'numpy'>` has no attribute `empty`
- error[lint:unresolved-attribute] dedupe/datamodel.py:108:10: Type `<module 'numpy'>` has no attribute `typing`
- error[lint:unresolved-attribute] dedupe/datamodel.py:111:21: Type `<module 'numpy'>` has no attribute `empty`
- error[lint:unresolved-attribute] dedupe/datamodel.py:127:26: Type `<module 'numpy'>` has no attribute `typing`
- error[lint:unresolved-attribute] dedupe/datamodel.py:128:10: Type `<module 'numpy'>` has no attribute `typing`
- error[lint:unresolved-attribute] dedupe/datamodel.py:132:44: Type `<module 'numpy'>` has no attribute `prod`
- error[lint:unresolved-attribute] dedupe/labeler.py:88:55: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] dedupe/labeler.py:93:26: Type `<module 'numpy'>` has no attribute `delete`
- error[lint:unresolved-attribute] dedupe/labeler.py:129:35: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] dedupe/labeler.py:168:35: Type `<module 'numpy'>` has no attribute `delete`
- error[lint:unresolved-attribute] dedupe/labeler.py:191:34: Type `<module 'numpy'>` has no attribute `fromiter`
- error[lint:unresolved-attribute] dedupe/labeler.py:194:19: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] dedupe/labeler.py:344:52: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] dedupe/labeler.py:346:20: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] dedupe/labeler.py:353:17: Type `<module 'numpy'>` has no attribute `concatenate`
- error[lint:unresolved-attribute] dedupe/labeler.py:357:34: Type `<module 'numpy'>` has no attribute `any`
- error[lint:unresolved-attribute] dedupe/labeler.py:380:17: Type `<module 'numpy'>` has no attribute `argmin`
- error[lint:unresolved-attribute] dedupe/labeler.py:386:23: Type `<module 'numpy'>` has no attribute `std`
- error[lint:unresolved-attribute] dedupe/labeler.py:410:18: Type `<module 'numpy'>` has no attribute `concatenate`
- Found 116 diagnostics
+ Found 70 diagnostics

trio (https://github.com/python-trio/trio)
- error[lint:invalid-type-form] src/trio/_abc.py:87:34: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_abc.py:96:36: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_abc.py:108:38: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_abc.py:117:37: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_abc.py:126:33: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_channel.py:152:29: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_channel.py:154:32: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_channel.py:175:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_generated_run.py:36:29: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_generated_run.py:92:28: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_generated_run.py:105:22: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_generated_run.py:131:32: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_generated_run.py:132:19: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_generated_run.py:135:6: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_io_epoll.py:25:16: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_io_epoll.py:26:17: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-argument-type] src/trio/_core/_parking_lot.py:100:34: Argument to this function is incorrect: Expected `Coroutine[Any, Any, Any]`, found `CoroutineType[Any, Outcome[object], Any]`
- error[lint:invalid-type-form] src/trio/_core/_parking_lot.py:91:34: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_parking_lot.py:94:35: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_parking_lot.py:110:38: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_parking_lot.py:153:26: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_parking_lot.py:154:21: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_parking_lot.py:192:60: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_parking_lot.py:205:57: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_parking_lot.py:221:34: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_parking_lot.py:279:31: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:376:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:409:34: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1160:22: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1174:29: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1185:40: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1191:30: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1208:15: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1271:36: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1272:23: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1714:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1715:16: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1719:16: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1722:16: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1752:37: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1803:36: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1816:32: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1851:36: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1852:28: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1858:10: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1927:33: Variable of type `Never` is not allowed in a type expression
+ warning[lint:possibly-unbound-attribute] src/trio/_core/_run.py:1971:13: Attribute `_child_finished` on type `Nursery | None` is possibly unbound
- error[lint:invalid-type-form] src/trio/_core/_run.py:1983:36: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1984:23: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:1987:10: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:2050:36: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:2051:28: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:2317:32: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:2318:19: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:2607:32: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:2608:24: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run.py:2865:23: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_run_context.py:12:11: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:19:29: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:24:36: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:27:38: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:31:37: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:38:45: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:38:75: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:167:38: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:170:37: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:173:25: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:187:40: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:196:25: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:222:22: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_instrumentation.py:252:37: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_parking_lot.py:230:35: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_parking_lot.py:307:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_parking_lot.py:357:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_parking_lot.py:379:39: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-argument-type] src/trio/_core/_tests/test_run.py:241:26: Argument to this function is incorrect: Expected `Task`, found `Task | None`
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:230:9: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:231:9: Variable of type `Never` is not allowed in a type expression
+ error[lint:invalid-argument-type] src/trio/_core/_tests/test_run.py:248:26: Argument to this function is incorrect: Expected `Task`, found `Task | None`
+ warning[lint:possibly-unbound-attribute] src/trio/_core/_tests/test_run.py:286:16: Attribute `parent_task` on type `Nursery | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] src/trio/_core/_tests/test_run.py:294:12: Attribute `parent_nursery` on type `Unknown | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/trio/_core/_tests/test_run.py:294:12: Attribute `parent_nursery` on type `Task | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] src/trio/_core/_tests/test_run.py:294:35: Attribute `eventual_parent_nursery` on type `Unknown | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/trio/_core/_tests/test_run.py:294:35: Attribute `eventual_parent_nursery` on type `Task | None` is possibly unbound
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:867:20: Variable of type `Never` is not allowed in a type expression
- warning[lint:unused-ignore-comment] src/trio/_core/_tests/test_run.py:930:43: Unused blanket `type: ignore` directive
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:1128:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:1156:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:1189:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:1219:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:1566:22: Variable of type `Never` is not allowed in a type expression
+ warning[lint:possibly-unbound-attribute] src/trio/_core/_tests/test_run.py:1613:16: Attribute `parent_task` on type `Nursery | None` is possibly unbound
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:2304:11: Variable of type `Never` is not allowed in a type expression
- error[lint:unresolved-attribute] src/trio/_core/_tests/test_run.py:2345:9: Type `None` has no attribute `coro`
+ error[lint:invalid-argument-type] src/trio/_core/_tests/test_run.py:2382:17: Argument to this function is incorrect: Expected `Task`, found `Task | None`
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:2358:21: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:2359:11: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_tests/test_run.py:2407:11: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_core/_traps.py:282:52: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_sync.py:70:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_sync.py:160:21: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_sync.py:225:30: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_sync.py:227:39: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_sync.py:227:45: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_sync.py:287:53: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_sync.py:326:52: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_sync.py:366:46: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_sync.py:544:12: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_sync.py:551:13: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_tests/test_sync.py:631:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_tests/test_threads.py:566:56: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_tests/test_threads.py:570:50: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_tests/test_threads.py:584:56: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_tests/test_threads.py:587:50: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] src/trio/_tests/test_threads.py:919:39: Variable of type `Never` is not allowed in a type expression
- Found 1095 diagnostics
+ Found 998 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[lint:unresolved-attribute] strawberry/extensions/tracing/datadog.py:78:16: Type `<module 'hashlib'>` has no attribute `md5`
- Found 452 diagnostics
+ Found 451 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- error[lint:unresolved-attribute] src/check_jsonschema/cachedownloader.py:111:16: Type `<module 'hashlib'>` has no attribute `sha256`
- Found 63 diagnostics
+ Found 62 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[lint:unresolved-attribute] comtypes/_npsupport.py:108:27: Type `<module 'numpy'>` has no attribute `sctypeDict`
- error[lint:unresolved-attribute] comtypes/_npsupport.py:114:13: Unresolved attribute `_typecodes` on type `<module 'numpy.ctypeslib'>`.
- error[lint:unresolved-attribute] comtypes/_npsupport.py:115:16: Type `<module 'numpy.ctypeslib'>` has no attribute `_typecodes`
- error[lint:unresolved-attribute] comtypes/test/test_npsupport.py:121:17: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] comtypes/test/test_npsupport.py:187:18: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] comtypes/test/test_npsupport.py:202:18: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] comtypes/test/test_npsupport.py:209:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] comtypes/test/test_npsupport.py:217:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] comtypes/test/test_npsupport.py:283:17: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] comtypes/test/test_npsupport.py:306:17: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] comtypes/test/test_npsupport.py:323:17: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] comtypes/test/test_npsupport.py:349:13: Type `<module 'numpy'>` has no attribute `array`
- Found 690 diagnostics
+ Found 678 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[lint:unresolved-attribute] pydantic/_internal/_generate_schema.py:129:62: Type `<module 'collections.abc'>` has no attribute `Set`
- error[lint:unresolved-attribute] pydantic/_internal/_serializers.py:28:5: Type `<module 'collections.abc'>` has no attribute `Set`
- error[lint:unresolved-attribute] pydantic/_internal/_validators.py:442:13: Type `<module 'collections.abc'>` has no attribute `Set`
- error[lint:unresolved-attribute] pydantic/fields.py:81:26: Type `<module 'types'>` has no attribute `Discriminator`
- error[lint:unresolved-attribute] pydantic/fields.py:149:26: Type `<module 'types'>` has no attribute `Discriminator`
- error[lint:unresolved-attribute] pydantic/fields.py:193:19: Type `<module 'types'>` has no attribute `Strict`
- error[lint:unresolved-attribute] pydantic/fields.py:207:22: Type `<module 'types'>` has no attribute `FailFast`
- error[lint:unresolved-attribute] pydantic/fields.py:792:26: Type `<module 'types'>` has no attribute `Discriminator`
+ error[lint:invalid-parameter-default] pydantic/fields.py:792:5: Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- error[lint:unresolved-attribute] pydantic/fields.py:831:26: Type `<module 'types'>` has no attribute `Discriminator`
+ error[lint:invalid-parameter-default] pydantic/fields.py:831:5: Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- error[lint:unresolved-attribute] pydantic/fields.py:870:26: Type `<module 'types'>` has no attribute `Discriminator`
+ error[lint:invalid-parameter-default] pydantic/fields.py:870:5: Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- error[lint:unresolved-attribute] pydantic/fields.py:908:26: Type `<module 'types'>` has no attribute `Discriminator`
+ error[lint:invalid-parameter-default] pydantic/fields.py:908:5: Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- error[lint:unresolved-attribute] pydantic/fields.py:947:26: Type `<module 'types'>` has no attribute `Discriminator`
+ error[lint:invalid-parameter-default] pydantic/fields.py:947:5: Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- error[lint:invalid-type-form] pydantic/v1/fields.py:907:16: Variable of type `Never` is not allowed in a type expression
- Found 799 diagnostics
+ Found 791 diagnostics

imagehash (https://github.com/JohannesBuchner/imagehash)
- error[lint:unresolved-attribute] imagehash/__init__.py:112:10: Type `<module 'numpy'>` has no attribute `count_nonzero`
+ warning[lint:unused-ignore-comment] imagehash/__init__.py:118:72: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] imagehash/__init__.py:124:76: Unused blanket `type: ignore` directive
- error[lint:unresolved-attribute] imagehash/__init__.py:182:15: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] imagehash/__init__.py:190:15: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] imagehash/__init__.py:230:19: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] imagehash/__init__.py:233:43: Type `<module 'numpy'>` has no attribute `mean`
- error[lint:unresolved-attribute] imagehash/__init__.py:252:11: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] imagehash/__init__.py:276:11: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] imagehash/__init__.py:279:8: Type `<module 'numpy'>` has no attribute `median`
- error[lint:unresolved-attribute] imagehash/__init__.py:296:11: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] imagehash/__init__.py:320:11: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] imagehash/__init__.py:339:11: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] imagehash/__init__.py:376:11: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] imagehash/__init__.py:390:8: Type `<module 'numpy'>` has no attribute `median`
- error[lint:unresolved-attribute] imagehash/__init__.py:411:14: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] imagehash/__init__.py:412:13: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] imagehash/__init__.py:426:13: Type `<module 'numpy'>` has no attribute `linspace`
- error[lint:unresolved-attribute] imagehash/__init__.py:428:23: Type `<module 'numpy'>` has no attribute `histogram`
- error[lint:unresolved-attribute] imagehash/__init__.py:428:23: Type `<module 'numpy'>` has no attribute `histogram`
- error[lint:unresolved-attribute] imagehash/__init__.py:428:23: Type `<module 'numpy'>` has no attribute `histogram`
- error[lint:unresolved-attribute] imagehash/__init__.py:430:20: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] imagehash/__init__.py:432:24: Type `<module 'numpy'>` has no attribute `histogram`
- error[lint:unresolved-attribute] imagehash/__init__.py:432:24: Type `<module 'numpy'>` has no attribute `histogram`
- error[lint:unresolved-attribute] imagehash/__init__.py:432:24: Type `<module 'numpy'>` has no attribute `histogram`
- error[lint:unresolved-attribute] imagehash/__init__.py:434:21: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] imagehash/__init__.py:446:19: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] imagehash/__init__.py:555:21: Type `<module 'numpy'>` has no attribute `transpose`
- error[lint:unresolved-attribute] imagehash/__init__.py:555:37: Type `<module 'numpy'>` has no attribute `nonzero`
- error[lint:unresolved-attribute] imagehash/__init__.py:602:22: Type `<module 'numpy'>` has no attribute `full`
- error[lint:unresolved-attribute] imagehash/__init__.py:669:11: Type `<module 'numpy'>` has no attribute `array`
- Found 40 diagnostics
+ Found 13 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- error[lint:unresolved-import] tests/test_stl.py:678:33: Module `collections.abc` has no member `Set`
- Found 257 diagnostics
+ Found 256 diagnostics

jinja (https://github.com/pallets/jinja)
- error[lint:unresolved-import] src/jinja2/bccache.py:18:21: Module `hashlib` has no member `sha1`
- error[lint:unresolved-import] src/jinja2/loaders.py:13:21: Module `hashlib` has no member `sha1`
- Found 383 diagnostics
+ Found 381 diagnostics

websockets (https://github.com/aaugustin/websockets)
- error[lint:unresolved-attribute] src/websockets/utils.py:32:12: Type `<module 'hashlib'>` has no attribute `sha1`
- Found 110 diagnostics
+ Found 109 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[lint:unresolved-attribute] mkosi/__init__.py:3607:25: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] mkosi/__init__.py:3639:27: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] mkosi/installer/zypper.py:73:27: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] mkosi/qemu.py:120:12: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] mkosi/util.py:214:9: Type `<module 'hashlib'>` has no attribute `sha256`
- Found 327 diagnostics
+ Found 322 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[lint:unresolved-attribute] porcupine/tabs.py:686:16: Type `<module 'hashlib'>` has no attribute `md5`
- Found 102 diagnostics
+ Found 101 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[lint:unresolved-import] ignite/engine/deterministic.py:115:25: Module `hashlib` has no member `md5`
- error[lint:unresolved-attribute] ignite/utils.py:451:16: Type `<module 'hashlib'>` has no attribute `sha256`
- Found 2265 diagnostics
+ Found 2263 diagnostics

pip (https://github.com/pypa/pip)
- error[lint:unresolved-attribute] src/pip/_internal/cache.py:30:12: Type `<module 'hashlib'>` has no attribute `sha224`
- error[lint:unresolved-attribute] src/pip/_internal/operations/build/build_tracker.py:86:18: Type `<module 'hashlib'>` has no attribute `sha224`
- error[lint:unresolved-attribute] src/pip/_internal/self_outdated_check.py:44:12: Type `<module 'hashlib'>` has no attribute `sha224`
- error[lint:unresolved-attribute] src/pip/_internal/utils/misc.py:612:9: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/pip/_vendor/cachecontrol/caches/file_cache.py:56:16: Type `<module 'hashlib'>` has no attribute `sha224`
- error[lint:unresolved-attribute] src/pip/_vendor/distlib/database.py:513:22: Type `<module 'hashlib'>` has no attribute `md5`
- error[lint:unresolved-attribute] src/pip/_vendor/distlib/database.py:1007:20: Type `<module 'hashlib'>` has no attribute `md5`
- error[lint:unresolved-attribute] src/pip/_vendor/distlib/index.py:269:22: Type `<module 'hashlib'>` has no attribute `md5`
- error[lint:unresolved-attribute] src/pip/_vendor/distlib/index.py:270:25: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/pip/_vendor/requests/auth.py:148:24: Type `<module 'hashlib'>` has no attribute `md5`
- error[lint:unresolved-attribute] src/pip/_vendor/requests/auth.py:156:24: Type `<module 'hashlib'>` has no attribute `sha1`
- error[lint:unresolved-attribute] src/pip/_vendor/requests/auth.py:164:24: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/pip/_vendor/requests/auth.py:172:24: Type `<module 'hashlib'>` has no attribute `sha512`
- error[lint:unresolved-attribute] src/pip/_vendor/requests/auth.py:205:18: Type `<module 'hashlib'>` has no attribute `sha1`
- Found 1028 diagnostics
+ Found 1014 diagnostics

nox (https://github.com/wntrblm/nox)
- error[lint:unresolved-attribute] nox/sessions.py:97:20: Type `<module 'hashlib'>` has no attribute `sha1`
- Found 44 diagnostics
+ Found 43 diagnostics

black (https://github.com/psf/black)
- error[lint:unresolved-attribute] src/black/cache.py:92:16: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-import] src/black/mode.py:9:21: Module `hashlib` has no member `sha256`
- Found 133 diagnostics
+ Found 131 diagnostics

itsdangerous (https://github.com/pallets/itsdangerous)
- error[lint:unresolved-attribute] src/itsdangerous/signer.py:45:12: Type `<module 'hashlib'>` has no attribute `sha1`
- Found 7 diagnostics
+ Found 6 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[lint:unresolved-attribute] src/poetry/masonry/builders/editable.py:272:19: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-import] src/poetry/packages/locker.py:9:21: Module `hashlib` has no member `sha256`
- error[lint:unresolved-attribute] src/poetry/plugins/plugin_manager.py:126:29: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/poetry/publishing/hash_manager.py:23:29: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/poetry/publishing/hash_manager.py:28:32: Type `<module 'hashlib'>` has no attribute `md5`
- error[lint:unresolved-attribute] src/poetry/repositories/http_repository.py:383:29: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/poetry/utils/cache.py:51:13: Type `<module 'hashlib'>` has no attribute `md5`
- error[lint:unresolved-attribute] src/poetry/utils/cache.py:52:14: Type `<module 'hashlib'>` has no attribute `sha1`
- error[lint:unresolved-attribute] src/poetry/utils/cache.py:53:16: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/poetry/utils/cache.py:210:15: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/poetry/utils/env/env_manager.py:625:19: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-import] tests/integration/test_utils_vcs_git.py:7:21: Module `hashlib` has no member `sha1`
- error[lint:unresolved-import] tests/packages/test_locker.py:10:21: Module `hashlib` has no member `sha256`
- error[lint:unresolved-attribute] tests/repositories/fixtures/pypi.org/generate.py:106:23: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] tests/repositories/fixtures/pypi.org/generate.py:107:20: Type `<module 'hashlib'>` has no attribute `md5`
- Found 1136 diagnostics
+ Found 1121 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[lint:unresolved-attribute] src/werkzeug/datastructures/headers.py:495:64: Type `<module 'collections.abc'>` has no attribute `Set`
- error[lint:unresolved-attribute] src/werkzeug/datastructures/headers.py:501:61: Type `<module 'collections.abc'>` has no attribute `Set`
- error[lint:unresolved-attribute] src/werkzeug/datastructures/headers.py:538:60: Type `<module 'collections.abc'>` has no attribute `Set`
- error[lint:unresolved-attribute] src/werkzeug/datastructures/headers.py:551:73: Type `<module 'collections.abc'>` has no attribute `Set`
- error[lint:unresolved-attribute] src/werkzeug/debug/__init__.py:45:12: Type `<module 'hashlib'>` has no attribute `sha1`
- error[lint:unresolved-attribute] src/werkzeug/debug/__init__.py:196:9: Type `<module 'hashlib'>` has no attribute `sha1`
- error[lint:unresolved-import] src/werkzeug/http.py:13:21: Module `hashlib` has no member `sha1`
- Found 605 diagnostics
+ Found 598 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- error[lint:unresolved-attribute] src/bandersnatch/mirror.py:888:20: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/bandersnatch/tests/plugins/test_storage_plugins.py:72:17: Type `<module 'hashlib'>` has no attribute `md5`
- error[lint:unresolved-attribute] src/bandersnatch/tests/plugins/test_storage_plugins.py:216:21: Type `<module 'hashlib'>` has no attribute `md5`
- error[lint:unresolved-attribute] src/bandersnatch_storage_plugins/s3.py:260:22: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/bandersnatch_storage_plugins/s3.py:261:22: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:660:22: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:661:22: Type `<module 'hashlib'>` has no attribute `sha256`
- Found 181 diagnostics
+ Found 174 diagnostics

vision (https://github.com/pytorch/vision)
- error[lint:unresolved-attribute] gallery/others/plot_optical_flow.py:44:23: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] gallery/others/plot_repurposing_annotations.py:39:26: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] gallery/others/plot_visualization_utils.py:33:26: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] packaging/wheel/relocate.py:72:9: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] packaging/wheel/relocate.py:127:15: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] references/classification/train.py:108:9: Type `<module 'hashlib'>` has no attribute `sha1`
- error[lint:unresolved-attribute] references/classification/utils.py:381:19: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:unresolved-attribute] references/detection/coco_eval.py:29:24: Type `<module 'numpy'>` has no attribute `unique`
- error[lint:unresolved-attribute] references/detection/coco_eval.py:46:40: Type `<module 'numpy'>` has no attribute `concatenate`
- error[lint:unresolved-attribute] references/detection/coco_eval.py:107:34: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] references/detection/coco_eval.py:169:22: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] references/detection/coco_eval.py:170:24: Type `<module 'numpy'>` has no attribute `concatenate`
- error[lint:unresolved-attribute] references/detection/coco_eval.py:173:27: Type `<module 'numpy'>` has no attribute `unique`
- error[lint:unresolved-attribute] references/detection/coco_eval.py:173:27: Type `<module 'numpy'>` has no attribute `unique`
- error[lint:unresolved-attribute] references/detection/coco_eval.py:173:27: Type `<module 'numpy'>` has no attribute `unique`
- error[lint:unresolved-attribute] references/detection/coco_eval.py:192:32: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] references/detection/group_by_aspect_ratio.py:189:18: Type `<module 'numpy'>` has no attribute `linspace`
- error[lint:unresolved-attribute] references/detection/group_by_aspect_ratio.py:192:14: Type `<module 'numpy'>` has no attribute `unique`
- error[lint:invalid-type-form] references/detection/transforms.py:298:24: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] references/detection/transforms.py:419:24: Variable of type `Never` is not allowed in a type expression
- error[lint:unresolved-attribute] references/segmentation/transforms.py:80:34: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] references/video_classification/train.py:123:9: Type `<module 'hashlib'>` has no attribute `sha1`
- error[lint:unresolved-attribute] test/builtin_dataset_mocks.py:522:19: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/builtin_dataset_mocks.py:1492:18: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/builtin_dataset_mocks.py:1493:18: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/builtin_dataset_mocks.py:1509:18: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/builtin_dataset_mocks.py:1513:18: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/builtin_dataset_mocks.py:1552:12: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/builtin_dataset_mocks.py:1553:15: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/common_utils.py:181:20: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/common_utils.py:195:20: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/conftest.py:111:24: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/conftest.py:119:5: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_datasets.py:38:9: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] test/test_datasets.py:389:20: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_datasets.py:473:18: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_datasets.py:1642:16: Type `<module 'numpy'>` has no attribute `concatenate`
- error[lint:unresolved-attribute] test/test_datasets.py:1644:17: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] test/test_datasets.py:1645:17: Type `<module 'numpy'>` has no attribute `ones`
- error[lint:unresolved-attribute] test/test_datasets.py:1648:9: Type `<module 'numpy'>` has no attribute `save`
- error[lint:unresolved-attribute] test/test_datasets.py:1815:18: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] test/test_datasets.py:1816:19: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] test/test_datasets.py:1873:29: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] test/test_datasets.py:2083:25: Type `<module 'numpy'>` has no attribute `arange`
- error[lint:unresolved-attribute] test/test_datasets.py:2088:17: Type `<module 'numpy'>` has no attribute `testing`
- error[lint:unresolved-attribute] test/test_datasets.py:2215:25: Type `<module 'numpy'>` has no attribute `arange`
- error[lint:unresolved-attribute] test/test_datasets.py:2220:17: Type `<module 'numpy'>` has no attribute `testing`
- error[lint:unresolved-attribute] test/test_datasets.py:2273:25: Type `<module 'numpy'>` has no attribute `arange`
- error[lint:unresolved-attribute] test/test_datasets.py:2274:25: Type `<module 'numpy'>` has no attribute `flip`
- error[lint:unresolved-attribute] test/test_datasets.py:2281:17: Type `<module 'numpy'>` has no attribute `testing`
- error[lint:unresolved-attribute] test/test_datasets.py:2777:19: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_datasets.py:2842:20: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_datasets.py:2846:21: Type `<module 'numpy'>` has no attribute `arange`
- error[lint:unresolved-attribute] test/test_datasets.py:2847:9: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_datasets.py:2872:22: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_datasets.py:2876:22: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_datasets.py:3113:17: Type `<module 'numpy'>` has no attribute `ones`
- error[lint:unresolved-attribute] test/test_functional_tensor.py:113:43: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_functional_tensor.py:212:43: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_functional_tensor.py:246:43: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_functional_tensor.py:317:43: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_functional_tensor.py:397:39: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_functional_tensor.py:1032:30: Type `<module 'numpy'>` has no attribute `arange`
- error[lint:unresolved-attribute] test/test_functional_tensor.py:1035:35: Type `<module 'numpy'>` has no attribute `arange`
- error[lint:unresolved-attribute] test/test_image.py:69:33: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_image.py:100:36: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_image.py:204:36: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_image.py:244:32: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_image.py:250:32: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_image.py:277:32: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_image.py:284:36: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_image.py:362:14: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_image.py:388:14: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_ops.py:1861:25: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_prototype_datasets_utils.py:31:11: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_prototype_datasets_utils.py:35:37: Type `<module 'numpy'>` has no attribute `fromfile`
- error[lint:unresolved-attribute] test/test_transforms.py:215:18: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_transforms.py:241:18: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_transforms.py:254:18: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_transforms.py:271:18: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_transforms.py:299:18: Type `<module 'numpy'>` has no attribute `random`
- error[lint:unresolved-attribute] test/test_transforms.py:517:29: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:518:41: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:525:32: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:526:44: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:533:34: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:534:46: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:541:37: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:542:38: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:543:49: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:544:50: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:657:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:657:79: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:674:12: Type `<module 'numpy'>` has no attribute `issubdtype`
- error[lint:unresolved-attribute] test/test_transforms.py:678:55: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:692:59: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:756:12: Type `<module 'numpy'>` has no attribute `issubdtype`
- error[lint:unresolved-attribute] test/test_transforms.py:758:9: Type `<module 'numpy'>` has no attribute `testing`
- error[lint:unresolved-attribute] test/test_transforms.py:801:59: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:858:59: Type `<module 'numpy'>` has no attribute `asarray`
- error[lint:unresolved-attribute] test/test_transforms.py:876:19: Type `<module 'numpy'>` has no attribute `ones`
- error[lint:unresolved-attribute] test/test_transforms.py:878:19: Type `<module 'numpy'>` has no attribute `ones`
- error[lint:unresolved-attribute] test/test_transforms.py:880:19: Type `<module 'numpy'>` has no attribute `ones`
- error[lint:unresolved-attribute] test/test_transforms.py:883:37: Type `<module 'numpy'>` has no attribute `ones`
- error[lint:unresolved-attribute] test/test_transforms.py:885:37: Type `<module 'numpy'>` has no attribute `ones`
- error[lint:unresolved-attribute] test/test_transforms.py:897:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:902:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:907:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:909:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:914:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:916:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:923:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:928:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:933:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:935:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:940:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:942:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:949:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:959:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:961:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:966:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:968:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:973:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:975:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1031:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1036:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1041:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1092:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1097:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1148:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1154:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1158:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1166:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1171:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1176:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1178:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1183:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1185:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1192:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1205:9: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] test/test_transforms.py:1215:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1215:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1215:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1215:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1222:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1222:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1222:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1222:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1229:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1229:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1229:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1229:16: Type `<module 'numpy'>` has no attribute `where`
- error[lint:unresolved-attribute] test/test_transforms.py:1237:18: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1237:38: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1242:26: Type `<module 'numpy'>` has no attribute `ones`
- error[lint:unresolved-attribute] test/test_transforms.py:1261:14: Type `<module 'numpy'>` has no attribute `ones`
- error[lint:unresolved-attribute] test/test_transforms.py:1322:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1325:15: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1331:17: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1339:17: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1349:17: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1357:17: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1436:15: Type `<module 'numpy'>` has no attribute `linalg`
- error[lint:unresolved-attribute] test/test_transforms.py:1436:15: Type `<module 'numpy'>` has no attribute `linalg`
- error[lint:unresolved-attribute] test/test_transforms.py:1436:15: Type `<module 'numpy'>` has no attribute `linalg`
- error[lint:unresolved-attribute] test/test_transforms.py:1436:15: Type `<module 'numpy'>` has no attribute `linalg`
- error[lint:unresolved-attribute] test/test_transforms.py:1438:22: Type `<module 'numpy'>` has no attribute `diag`
- error[lint:unresolved-attribute] test/test_transforms.py:1451:16: Type `<module 'numpy'>` has no attribute `dot`
- error[lint:unresolved-attribute] test/test_transforms.py:1452:17: Type `<module 'numpy'>` has no attribute `sum`
- error[lint:unresolved-attribute] test/test_transforms.py:1455:28: Type `<module 'numpy'>` has no attribute `identity`
- error[lint:unresolved-attribute] test/test_transforms.py:1771:12: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:1988:21: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] test/test_transforms.py:2004:25: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] test/test_transforms.py:2005:32: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:2007:16: Type `<module 'numpy'>` has no attribute `linalg`
- error[lint:unresolved-attribute] test/test_transforms.py:2020:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:2021:13: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:2022:16: Type `<module 'numpy'>` has no attribute `linalg`
- error[lint:unresolved-attribute] test/test_transforms.py:2024:14: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:2032:15: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:2034:15: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:2043:16: Type `<module 'numpy'>` has no attribute `sum`
- error[lint:unresolved-attribute] test/test_transforms.py:2045:23: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] test/test_transforms.py:2046:27: Type `<module 'numpy'>` has no attribute `linalg`
- error[lint:unresolved-attribute] test/test_transforms.py:2052:28: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:2053:32: Type `<module 'numpy'>` has no attribute `dot`
- error[lint:unresolved-attribute] test/test_transforms.py:2061:21: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms.py:2062:25: Type `<module 'numpy'>` has no attribute `sum`
- error[lint:unresolved-attribute] test/test_transforms.py:2173:9: Type `<module 'numpy'>` has no attribute `zeros`
- error[lint:unresolved-attribute] test/test_transforms_tensor.py:504:46: Type `<module 'numpy'>` has no attribute `linspace`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:349:15: Type `<module 'numpy'>` has no attribute `empty`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:517:18: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:529:23: Type `<module 'numpy'>` has no attribute `min`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:530:23: Type `<module 'numpy'>` has no attribute `min`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:531:23: Type `<module 'numpy'>` has no attribute `max`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:532:23: Type `<module 'numpy'>` has no attribute `max`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:756:25: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1075:25: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1305:20: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1306:20: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1307:24: Type `<module 'numpy'>` has no attribute `linalg`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1308:21: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1315:26: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1316:26: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1525:25: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1716:29: Type `<module 'numpy'>` has no attribute `array`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1727:28: Type `<module 'numpy'>` has no attribute `min`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1728:28: Type `<module 'numpy'>` has no attribute `min`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1730:28: Type `<module 'numpy'>` has no attribute `max`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1731:29: Type `<module 'numpy'>` has no attribute `max`
- error[lint:unresolved-attribute] test/test_transforms_v2.py:1752:25: Type `<modu...*[Comment body truncated]*

---

_Comment by @github-actions[bot] on 2025-05-06 23:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>




---

_Marked ready for review by @dhruvmanila on 2025-05-07 13:39_

---

_Review requested from @carljm by @dhruvmanila on 2025-05-07 13:39_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-05-07 13:39_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-05-07 13:39_

---

_Review requested from @dcreager by @dhruvmanila on 2025-05-07 13:39_

---

_@MichaReiser reviewed on 2025-05-07 13:57_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/dunder_all.rs`:35 on 2025-05-07 13:57_

I don't know what it does to performance but we could consider returning a `BtreeSet` if order matters (I saw a sort somewhere further down)

---

_@dhruvmanila reviewed on 2025-05-07 14:01_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/dunder_all.rs`:35 on 2025-05-07 14:01_

That's mainly for the `ty_extensions.dunder_all_names` extension function and not for any semantic usages.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/dunder_all.md`:380 on 2025-05-07 14:26_

I guess the case where we are importing a non-re-export, which is listed in the dynamic `__all__`, doesn't  count as a false positive that we need to care about, since a non-re-export can only occur in a stub file, and in a stub file there is no reason to ever use dynamic `__all__` construction that type checkers don't understand.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/dunder_all.rs`:66 on 2025-05-07 14:27_

This is mainly to reduce false positives because there are many different idioms that are being used in the ecosystem that dynamically generates the values of `__all__` in the module.

---

_@dhruvmanila reviewed on 2025-05-07 14:27_

---

_@dhruvmanila reviewed on 2025-05-07 14:29_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/dunder_all.rs`:133 on 2025-05-07 14:29_

I'm resolving this TODO by returning `false`. Ideally, I think it might be useful to differentiate between "`__all__` not present" vs "invalid `__all__`" in the return type for `dunder_all_names` query but for now I've used a simplified approach here.

---

_@dhruvmanila reviewed on 2025-05-07 14:35_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/dunder_all.rs`:258 on 2025-05-07 14:35_

I think I'll need to mark here as `invalid` (`self.invalid = true`) as well if `dunder_all_names_for_import_from` returns `None` as otherwise it adds new false positives.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/dunder_all.rs`:126 on 2025-05-07 14:52_

I assume this TODO comes from the spec's use of `submodule.__all__` in its example? I don't think this is a clear specification that it must be a submodule, and I don't really see any advantage to that requirement. In fact I think typeshed does this in non-submodule cases (`collections.abc` reuses the `__all__` from `_collections_abc`, which is not a submodule -- I guess this isn't "extending", but it doesn't make sense to have different rules for which external `__all__` can be used.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/dunder_all.rs`:100 on 2025-05-07 14:55_

weak preference for not using "submodule" in all these wordings, even though the spec does, since I don't think it's actually required to be a submodule

---

_Review comment by @carljm on `crates/ty_python_semantic/src/dunder_all.rs`:258 on 2025-05-07 14:56_

Is there a reason you didn't do this yet? Should we at least have a TODO comment for it, or do you plan to add it in this PR?

---

_@carljm approved on 2025-05-07 14:58_

Awesome work, this looks great!

I've reviewed everything except haven't done a thorough reading of the new visitor code yet. Have to jump into some other meetings for a bit, so wanted to submit what I have. Given that the tested behaviors look great, I think you can feel free to go ahead and land this; if there are any issues in the details of the visitor code, I'm sure they can be fixed as follow-up.

---

_@dhruvmanila reviewed on 2025-05-07 15:02_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/dunder_all.rs`:258 on 2025-05-07 15:02_

I'm planning to do it in this PR itself.

---

_@dhruvmanila reviewed on 2025-05-07 15:06_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/dunder_all.rs`:126 on 2025-05-07 15:06_

Yeah, it comes from the usage of "submodule" and I wasn't sure about whether that's a requirement or not which is why I marked it as TODO.

That makes sense. I'll remove the TODO and update all usages of "submodule" to just use "module" instead.

---

_Comment by @dhruvmanila on 2025-05-07 16:12_

I'm going to merge this, I'm still looking into a couple of false positives that I've noted down but don't want it to block this from merging this. I'll take this and any reviews that this PR receives as follow-up.

---

_Merged by @dhruvmanila on 2025-05-07 16:12_

---

_Closed by @dhruvmanila on 2025-05-07 16:12_

---

_Branch deleted on 2025-05-07 16:12_

---
