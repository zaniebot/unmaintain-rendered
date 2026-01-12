```yaml
number: 17800
title: "[ty] Check overloaded function implementation consistency"
type: pull_request
state: open
author: LaBatata101
labels:
  - ty
assignees: []
draft: true
base: main
head: redknot-overload
created_at: 2025-05-02T21:45:34Z
updated_at: 2025-07-10T20:14:17Z
url: https://github.com/astral-sh/ruff/pull/17800
synced_at: 2026-01-12T15:56:05Z
```

# [ty] Check overloaded function implementation consistency

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
I've separated the implementation of `is_assignable_to` into `is_parameters_assignable_to` and `is_return_type_assignable_to` as discussed in Discord, that solved the problem I was having.  But, there are still a few issues that I need some help with. 

The first one is, some tests are failing in `dataclass_transform.md`, specifically when the function is annotated with `dataclass_transform` like this one:
```python
@overload
@dataclass_transform()
def versioned_class(
    cls: T,
    *,
    version: int = 1,
) -> T: ...
@overload
def versioned_class(
    *,
    version: int = 1,
) -> Callable[[T], T]: ...
def versioned_class(
    cls: T | None = None,
    *,
    version: int = 1,
) -> T | Callable[[T], T]:
    raise NotImplementedError
```
I guess this is related to this part of the spec:

> When a type checker checks the implementation for consistency with overloads, it should first apply any transforms that change the effective type of the implementation including the presence of a yield statement in the implementation body, the use of async def, and the presence of additional decorators.
>
> https://typing.python.org/en/latest/spec/overload.html#implementation-consistency

Not sure how these transforms should be handled.

~~The second issue I have is that in this python code:~~
```python
@overload
def x(y: None = ...) -> None: ...
@overload
def x(y: int) -> str: ...


def x(y: int | None = None): ...
```
~~My implementation is not creating the diagnostic for the function implementation when it's missing the return type.~~

I'll add more tests after I solve these issues.

Closes astral-sh/ty#109
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Markddown tests.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-02 21:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- warning[lint:unused-ignore-comment] beartype/_decor/decormain.py:100:20: Unused blanket `type: ignore` directive
- Found 554 diagnostics
+ Found 553 diagnostics

anyio (https://github.com/agronholm/anyio)
+ error[lint:invalid-overload] src/anyio/_core/_tempfile.py:538:11: Overloaded implementation is not consistent with signature of overload 1 `(suffix: str | None = None, prefix: str | None = None, dir: str | None = None, text: bool = Literal[False]) -> @Todo(generic types.CoroutineType)`
+ error[lint:invalid-overload] src/anyio/_core/_tempfile.py:538:11: Overloaded implementation is not consistent with signature of overload 2 `(suffix: bytes | None = None, prefix: bytes | None = None, dir: bytes | None = None, text: bool = Literal[False]) -> @Todo(generic types.CoroutineType)`
+ error[lint:invalid-overload] src/anyio/_core/_tempfile.py:576:11: Overloaded implementation is not consistent with signature of overload 1 `(suffix: str | None = None, prefix: str | None = None, dir: str | None = None) -> @Todo(generic types.CoroutineType)`
+ error[lint:invalid-overload] src/anyio/_core/_tempfile.py:576:11: Overloaded implementation is not consistent with signature of overload 2 `(suffix: bytes | None = None, prefix: bytes | None = None, dir: bytes | None = None) -> @Todo(generic types.CoroutineType)`
- Found 128 diagnostics
+ Found 132 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ error[lint:invalid-overload] pydantic/config.py:1157:5: Overloaded implementation is not consistent with signature of overload 1 `(*, config: ConfigDict) -> (_TypeT, /) -> _TypeT`
+ error[lint:invalid-overload] pydantic/config.py:1157:5: Overloaded implementation is not consistent with signature of overload 2 `(**config: @Todo(`Unpack[]` special form)) -> (_TypeT, /) -> _TypeT`
+ error[lint:invalid-overload] pydantic/fields.py:1385:5: Overloaded implementation is not consistent with signature of overload 1 `(*, alias: str | None = None, alias_priority: int | None = None, title: str | None = None, field_title_generator: ((str, ComputedFieldInfo, /) -> str) | None = None, description: str | None = None, deprecated: deprecated | str | bool | None = None, examples: @Todo(specialized non-generic class) | None = None, json_schema_extra: @Todo(Support for `typing.TypeAlias`) | ((@Todo(Support for `typing.TypeAlias`), /) -> None) | None = None, repr: bool = Literal[True], return_type: Any = PydanticUndefinedType) -> (PropertyT, /) -> PropertyT`
+ error[lint:invalid-overload] pydantic/functional_serializers.py:346:5: Overloaded implementation is not consistent with signature of overload 1 `(*, mode: Literal["wrap"], when_used: Unknown = Literal["always"], return_type: Any = ellipsis) -> (_ModelWrapSerializerT, /) -> _ModelWrapSerializerT`
+ error[lint:invalid-overload] pydantic/functional_serializers.py:346:5: Overloaded implementation is not consistent with signature of overload 2 `(*, mode: Literal["plain"] = ellipsis, when_used: Unknown = Literal["always"], return_type: Any = ellipsis) -> (_ModelPlainSerializerT, /) -> _ModelPlainSerializerT`
+ error[lint:invalid-overload] pydantic/validate_call_decorator.py:82:5: Overloaded implementation is not consistent with signature of overload 1 `(*, config: ConfigDict | None = None, validate_return: bool = Literal[False]) -> (AnyCallableT, /) -> AnyCallableT`
- Found 781 diagnostics
+ Found 787 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ error[lint:invalid-overload] src/werkzeug/http.py:584:5: Overloaded implementation is not consistent with signature of overload 1 `(value: str | None) -> Accept`
+ error[lint:invalid-overload] src/werkzeug/http.py:655:5: Overloaded implementation is not consistent with signature of overload 1 `(value: str | None, on_update: ((Unknown, /) -> None) | None = None) -> RequestCacheControl`
+ error[lint:invalid-overload] src/werkzeug/http.py:703:5: Overloaded implementation is not consistent with signature of overload 1 `(value: str | None, on_update: ((ContentSecurityPolicy, /) -> None) | None = None) -> ContentSecurityPolicy`
- Found 449 diagnostics
+ Found 452 diagnostics

Expression (https://github.com/cognitedata/Expression)
- warning[lint:unused-ignore-comment] expression/extra/result/catch.py:28:13: Unused blanket `type: ignore` directive
- Found 507 diagnostics
+ Found 506 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ error[lint:invalid-overload] src/_pytest/fixtures.py:1302:5: Overloaded implementation is not consistent with signature of overload 1 `(fixture_function: (...) -> object, *, scope: Unknown | ((str, Config, /) -> Unknown) = ellipsis, params: @Todo(specialized non-generic class) | None = ellipsis, autouse: bool = ellipsis, ids: @Todo(specialized non-generic class) | ((Any, /) -> object) | None = ellipsis, name: str | None = ellipsis) -> FixtureFunctionDefinition`
- Found 737 diagnostics
+ Found 738 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ error[lint:invalid-overload] scrapy/utils/defer.py:380:5: Overloaded implementation is not consistent with signature of overload 1 `(o: _CT) -> Unknown`
- Found 1388 diagnostics
+ Found 1389 diagnostics

arviz (https://github.com/arviz-devs/arviz)
+ error[lint:invalid-overload] arviz/data/inference_data.py:1992:5: Overloaded implementation is not consistent with signature of overload 1 `(ids: @Todo(specialized non-generic class), dim: str | None = None, *, copy: bool = Literal[True], inplace: Literal[False], reset_dim: bool = Literal[True]) -> InferenceData`
+ error[lint:invalid-overload] arviz/data/inference_data.py:1992:5: Overloaded implementation is not consistent with signature of overload 2 `(ids: @Todo(specialized non-generic class), dim: str | None = None, *, copy: bool = Literal[True], inplace: Literal[True], reset_dim: bool = Literal[True]) -> None`
+ error[lint:invalid-overload] arviz/data/inference_data.py:1992:5: Overloaded implementation is not consistent with signature of overload 3 `(ids: @Todo(specialized non-generic class), dim: str | None = None, *, copy: bool = Literal[True], inplace: bool = Literal[False], reset_dim: bool = Literal[True]) -> InferenceData | None`
- Found 835 diagnostics
+ Found 838 diagnostics

pyodide (https://github.com/pyodide/pyodide)
+ error[lint:invalid-overload] src/py/_pyodide/_core_docs.py:1298:5: Overloaded implementation is not consistent with signature of overload 1 `(obj: (...) -> Unknown, *, /, capture_this: bool = Literal[False], roundtrip: bool = Literal[True]) -> @Todo(specialized non-generic class)`
- Found 1046 diagnostics
+ Found 1047 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ error[lint:invalid-overload] lib/streamlit/runtime/connection_factory.py:205:5: Overloaded implementation is not consistent with signature of overload 1 `(name: Literal["sql"], max_entries: int | None = None, ttl: int | float | timedelta | None = None, autocommit: bool = Literal[False], **kwargs) -> Unknown`
+ error[lint:invalid-overload] lib/streamlit/runtime/connection_factory.py:205:5: Overloaded implementation is not consistent with signature of overload 2 `(name: str, type: Literal["sql"], max_entries: int | None = None, ttl: int | float | timedelta | None = None, autocommit: bool = Literal[False], **kwargs) -> Unknown`
+ error[lint:invalid-overload] lib/streamlit/runtime/connection_factory.py:205:5: Overloaded implementation is not consistent with signature of overload 3 `(name: Literal["snowflake"], max_entries: int | None = None, ttl: int | float | timedelta | None = None, autocommit: bool = Literal[False], **kwargs) -> Unknown`
+ error[lint:invalid-overload] lib/streamlit/runtime/connection_factory.py:205:5: Overloaded implementation is not consistent with signature of overload 4 `(name: str, type: Literal["snowflake"], max_entries: int | None = None, ttl: int | float | timedelta | None = None, autocommit: bool = Literal[False], **kwargs) -> Unknown`
+ error[lint:invalid-overload] lib/streamlit/runtime/connection_factory.py:205:5: Overloaded implementation is not consistent with signature of overload 5 `(name: Literal["snowpark"], max_entries: int | None = None, ttl: int | float | timedelta | None = None, **kwargs) -> Unknown`
- Found 3275 diagnostics
+ Found 3280 diagnostics

jax (https://github.com/google/jax)
+ error[lint:invalid-overload] jax/experimental/pallas/ops/tpu/splash_attention/splash_attention_kernel.py:148:5: Overloaded implementation is not consistent with signature of overload 2 `(mask: Array, q: Array, k: Array, v: Array, segment_ids: SegmentIds | None, save_residuals: Literal[True], mask_value: int | float, custom_type: str, attn_logits_soft_cap: int | float | None) -> tuple[Array, tuple[Array]]`
+ error[lint:invalid-overload] jax/experimental/pallas/ops/tpu/splash_attention/splash_attention_kernel.py:148:5: Overloaded implementation is not consistent with signature of overload 1 `(mask: Array, q: Array, k: Array, v: Array, segment_ids: SegmentIds | None, save_residuals: Literal[False], mask_value: int | float, custom_type: str, attn_logits_soft_cap: int | float | None) -> Array`
+ error[lint:invalid-overload] jax/experimental/pallas/ops/tpu/splash_attention/splash_attention_kernel.py:873:5: Overloaded implementation is not consistent with signature of overload 1 `(fwd_mask_info: MaskInfo, q: Array, k: Array, v: Array, segment_ids: SegmentIds | None, mask_value: int | float, is_mqa: bool, block_sizes: BlockSizes, residual_checkpoint_name: str | None, mask_function: @Todo(Inference of subscript on special form) | None, save_residuals: Literal[False] = Literal[False], attn_logits_soft_cap: int | float | None = None) -> Array`
+ error[lint:invalid-overload] jax/experimental/pallas/ops/tpu/splash_attention/splash_attention_kernel.py:873:5: Overloaded implementation is not consistent with signature of overload 2 `(fwd_mask_info: MaskInfo, q: Array, k: Array, v: Array, segment_ids: SegmentIds | None, mask_value: int | float, is_mqa: bool, block_sizes: BlockSizes, residual_checkpoint_name: str | None, mask_function: @Todo(Inference of subscript on special form) | None, save_residuals: Literal[True], attn_logits_soft_cap: int | float | None = None) -> @Todo(Inference of subscript on special form)`
- Found 3216 diagnostics
+ Found 3220 diagnostics

scipy (https://github.com/scipy/scipy)
- warning[lint:unused-ignore-comment] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:60:19: Unused blanket `type: ignore` directive
- Found 7370 diagnostics
+ Found 7369 diagnostics

```
</details>


---

_Comment by @carljm on 2025-05-02 22:01_

The second "issue" looks like correct behavior to me. A missing annotation is equivalent to an annotation of `Unknown/Any`, which is assignable to any type, and any type is assignable to it. So there should not be any diagnostic emitted on that code. [Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=a76906b41281d1ab44728a5f396f0d75) does not emit any diagnostic; [pyright](https://pyright-play.net/?pyrightVersion=1.1.352&strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMpgBuApiADZgCGAJgFC0ACRpFNt1xwUAHgBRwAuKADkwKYlAC8UAHRyAlFAC0APhFjiQuTMbNyVOhy59BmFDEWqoAZxggtc%2Bu049%2BQ1PgA%2B68VJ-F5Bx0gA) does emit one, but it doesn't make any sense to me; it looks like a bug.

---

_Comment by @LaBatata101 on 2025-05-02 22:04_

> The second "issue" looks like correct behavior to me. A missing annotation is equivalent to an annotation of `Unknown/Any`, which is assignable to any type, and any type is assignable to it. So there should not be any diagnostic emitted on that code. [Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=a76906b41281d1ab44728a5f396f0d75) does not emit any diagnostic; [pyright](https://pyright-play.net/?pyrightVersion=1.1.352&strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMpgBuApiADZgCGAJgFC0ACRpFNt1xwUAHgBRwAuKADkwKYlAC8UAHRyAlFAC0APhFjiQuTMbNyVOhy59BmFDEWqoAZxggtc%2Bu049%2BQ1PgA%2B68VJ-F5Bx0gA) does emit one, but it doesn't make any sense to me; it looks like a bug.

Ah okay, I was checking with `pyright`  forgot to mention that.

---

_Comment by @carljm on 2025-05-02 22:10_

I don't know off the top of my head why the `dataclass_transform` tests are failing. I don't think `@dataclass_transform` changes the signature of the function it's applied to, so I doubt there's an issue of us failing to apply decorator transforms, it seems more like something going wrong in the handling of the special `DataclassTransformer` type that results from usage of `@dataclass_transform`? It will probably just require more step-by-step debugging of those tests to understand what's happening.

---

_Label `red-knot` added by @AlexWaygood on 2025-05-02 22:35_

---

_Comment by @MichaReiser on 2025-05-03 18:01_

@LaBatata101 I went ahead and rebased your PR passed the Red Knot renaming change. Make sure you pull (force) before trying to push new changes.

---

_Comment by @LaBatata101 on 2025-05-03 18:50_

> @LaBatata101 I went ahead and rebased your PR passed the Red Knot renaming change. Make sure you pull (force) before trying to push new changes.

Thanks!

---

_Comment by @LaBatata101 on 2025-05-05 14:26_

@carljm could it be that tests failing for `dataclass_transform` are related to astral-sh/ruff#17541 not being implemented yet?

---

_Comment by @carljm on 2025-05-06 00:07_

> could it be that tests failing for `dataclass_transform` are related to astral-sh/ruff#17541 not being implemented yet?

I don't think so. I don't think the use of `@dataclass_transform` is even related to why these tests are failing. It looks to me like the signatures used in these tests are triggering a possible bug in our signature assignability checking. I added this debug code:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer.rs b/crates/ty_python_semantic/src/types/infer.rs
index 543d8ea060..8fc98276c9 100644
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -1251,6 +1251,13 @@ impl<'db> TypeInferenceBuilder<'db> {
         for (idx, overloaded_signature) in overloaded_signatures
             .iter()
             .filter(|overloaded_signature| {
+                eprintln!(
+                    "{} -- {}",
+                    impl_signature.display(self.db()),
+                    overloaded_signature.display(self.db())
+                );
+                dbg!(impl_signature.is_parameters_assignable_to(self.db(), overloaded_signature));
+                dbg!(overloaded_signature.is_return_type_assignable_to(self.db(), impl_signature));
                 !(impl_signature.is_parameters_assignable_to(self.db(), overloaded_signature)
                     && overloaded_signature.is_return_type_assignable_to(self.db(), impl_signature))
             })
```

And got this output on a failing test:

```
(cls: T | None = None, *, version: int = Literal[1]) -> T | ((T, /) -> T) -- (cls: T, *, version: int = Literal[1]) -> T
[crates/ty_python_semantic/src/types/infer.rs:1259:17] impl_signature.is_parameters_assignable_to(self.db(), overloaded_signature) = false
[crates/ty_python_semantic/src/types/infer.rs:1260:17] overloaded_signature.is_return_type_assignable_to(self.db(), impl_signature) = true
(cls: T | None = None, *, version: int = Literal[1]) -> T | ((T, /) -> T) -- (*, version: int = Literal[1]) -> (T, /) -> T
[crates/ty_python_semantic/src/types/infer.rs:1259:17] impl_signature.is_parameters_assignable_to(self.db(), overloaded_signature) = true
[crates/ty_python_semantic/src/types/infer.rs:1260:17] overloaded_signature.is_return_type_assignable_to(self.db(), impl_signature) = true
```

What this shows is that we are getting accurate signatures off of all the callables involved (and once we have a `Signature`, it's just like any other `Signature` -- there's no trace of `@dataclass_transform` left), but for some reason we do not consider the parameters `(cls: T | None = None, *, version: int = Literal[1])` assignable to the parameters `(cls: T, *, version: int = Literal[1])`. Which looks wrong to me, unless I'm missing something.

And I can reproduce this behavior without involving `dataclass_transform`: https://types.ruff.rs/c1777070-e36c-4859-9796-6f32d34fe056

So I think the next step here is to debug that minimal test case and fix it in `is_assignable_to_impl` (maybe even in a separate PR).

---

_Comment by @LaBatata101 on 2025-05-06 00:42_

> What this shows is that we are getting accurate signatures off of all the callables involved (and once we have a Signature, it's just like any other Signature -- there's no trace of @dataclass_transform left), but for some reason we do not consider the parameters (cls: T | None = None, *, version: int = Literal[1]) assignable to the parameters (cls: T, *, version: int = Literal[1]). Which looks wrong to me, unless I'm missing something.

> And I can reproduce this behavior without involving dataclass_transform: https://types.ruff.rs/c1777070-e36c-4859-9796-6f32d34fe056

> So I think the next step here is to debug that minimal test case and fix it in is_assignable_to_impl (maybe even in a separate PR).

I will look into that this week

---

_Comment by @dhruvmanila on 2025-05-06 05:17_

I don't think the issue is in the logic of `is_assignable_to_impl` because using concrete types in place of the generic `T` seems to work correctly (https://types.ruff.rs/a10c8ac9-76a6-447d-965f-af23d1fa468a). There must be somewhere where the specialization is not taking place? The `internal_signature` method that's used to get the `Signature` for each of the overloads and implementation does seem to apply the specializations though.

---

_Review requested from @dhruvmanila by @dhruvmanila on 2025-05-06 05:20_

---

_Comment by @carljm on 2025-05-06 23:41_

I don't think there should be any specialization happening here? These functions are unspecialized generics (as seen by the fact that the signatures printed out in my debugging above still have `T` in them), and they should be compared for assignability as unspecialized generics; they would only be specialized in the context of a call.

You're right that it looks like the issue is specific to typevars; it also looks like it's specific to a bound typevar: https://play.ty.dev/7bca2116-a4f4-4e07-a518-b1077403aec4

And it looks like it's specific to comparing `T | None` vs `T`: https://play.ty.dev/1c1e1872-163b-4661-b2fd-cea0434e7e61

Ok, I just figured out what I think it is. Pushed the fix in https://github.com/astral-sh/ruff/pull/17901

Rebasing this PR on top of that should fix the dataclass-transform tests, I think.

---

_Comment by @LaBatata101 on 2025-05-07 20:30_

So, I've pulled the new changes made in #17910, but this case still failing
```python
@overload
def func() -> None: ...
@overload
def func[T](x: T) -> T: ...
def func[T](x: T | None = None) -> T | None:
    return x
```
I've printed the output of `is_parameters_assignable_to` and it's returning `false` for these signatures: `x: T`, `x: T | None = None`. Wasn't #17910 supposed to fix that issue? Or did I misunderstand?

https://play.ty.dev/0fc474f1-7189-4a71-92ad-5f0cd6b5e123

---

_Comment by @carljm on 2025-05-07 23:06_

Ugh, sorry, this is https://github.com/astral-sh/ty/issues/95 -- it impacts the PEP 695 version but not the legacy-typevar version, because in this case we consider them different typevars.

I'm a little hesitant to land this overload consistency check without fixing that bug first, since it will cause more false positives than we currently do. Sorry... any chance you're interested in trying to tackle that bug?

---

_Comment by @LaBatata101 on 2025-05-07 23:16_

> Ugh, sorry, this is [astral-sh/ty#95](https://github.com/astral-sh/ty/issues/95) -- it impacts the PEP 695 version but not the legacy-typevar version, because in this case we consider them different typevars.
> 
> I'm a little hesitant to land this overload consistency check without fixing that bug first, since it will cause more false positives than we currently do. Sorry... any chance you're interested in trying to tackle that bug?

I can try, but I don't understand much about type theory or the python type system, do you have any resources that I can take a look at? Much of the stuff you guys talk about goes over my head.

---

_Comment by @carljm on 2025-05-08 00:25_

Probably the most useful written resource I can recommend (and this isn't saying a great deal) is the typing spec, particularly starting with the "core concepts" section. But that issue may not be the simplest place to start. Hopefully one of us can get to it before long, because I would like to land this PR -- your work here looks great.

---

_Renamed from "[red-knot] Check overloaded function implementation consistency" to "[ty] Check overloaded function implementation consistency" by @MichaReiser on 2025-05-08 05:12_

---

_Comment by @MichaReiser on 2025-07-10 14:12_

Thanks for your contribution. I'll close this as it seems to have gone stale. Anyone interested in picking this up. Feel free to open a new PR which addresses the feedback.

---

_Closed by @MichaReiser on 2025-07-10 14:12_

---

_Comment by @LaBatata101 on 2025-07-10 19:37_

> Thanks for your contribution. I'll close this as it seems to have gone stale. Anyone interested in picking this up. Feel free to open a new PR which addresses the feedback.

This PR is just waiting for https://github.com/astral-sh/ruff/pull/18634 to get merged. I've been a little busy the last couple of days, but I'll start working on https://github.com/astral-sh/ruff/pull/18634 again soon.

---

_Reopened by @MichaReiser on 2025-07-10 20:14_

---
