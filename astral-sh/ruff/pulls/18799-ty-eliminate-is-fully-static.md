```yaml
number: 18799
title: "[ty] eliminate is_fully_static"
type: pull_request
state: merged
author: carljm
labels:
  - great writeup
  - ty
assignees: []
merged: true
base: main
head: cjm/nofullystatic
created_at: 2025-06-19T17:52:09Z
updated_at: 2025-06-25T01:02:06Z
url: https://github.com/astral-sh/ruff/pull/18799
synced_at: 2026-01-12T15:56:25Z
```

# [ty] eliminate is_fully_static

---

_@carljm_

## Summary

Having a recursive type method to check whether a type is fully static is inefficient, unnecessary, and makes us overly strict about subtyping relations.

It's inefficient because we end up re-walking the same types many times to check for fully-static-ness.

It's unnecessary because we can check relations involving the dynamic type appropriately, depending whether the relation is subtyping or assignability.

We use the subtyping relation to simplify unions and intersections. We can usefully consider that `S <: T` for gradual types also, as long as it remains true that `S | T` is equivalent to `T` and `S & T` is equivalent to `S`.

One conservative definition (implemented here) that satisfies this requirement is that we consider `S <: T` if, for every possible pair of materializations `S'` and `T'`, `S' <: T'`. Or put differently the top materialization of `S` (`S+` -- the union of all possible materializations of `S`) is a subtype of the bottom materialization of `T` (`T-` -- the intersection of all possible materializations of `T`). In the most basic cases we can usefully say that `Any <: object` and that `Never <: Any`, and we can handle more complex cases inductively from there.

This definition of subtyping for gradual subtypes is not reflexive (`Any` is not a subtype of `Any`).

As a corollary, we also remove `is_gradual_equivalent_to` -- `is_equivalent_to` now has the meaning that `is_gradual_equivalent_to` used to have. If necessary, we could restore an `is_fully_static_equivalent_to` or similar (which would not do an `is_fully_static` pre-check of the types, but would instead pass a relation-kind enum down through a recursive equivalence check, similar to `has_relation_to`), but so far this doesn't appear to be necessary.

Credit to @JelleZijlstra for the observation that `is_fully_static` is unnecessary and overly restrictive on subtyping.

There is another possible definition of gradual subtyping: instead of requiring that `S+ <: T-`, we could instead require that `S+ <: T+` and `S- <: T-`. In other words, instead of requiring all materializations of `S` to be a subtype of every materialization of `T`, we just require that every materialization of `S` be a subtype of _some_ materialization of `T`, and that every materialization of `T` be a supertype of some materialization of `S`. This definition also preserves the core invariant that `S <: T` implies that `S | T = T` and `S & T = S`, and it restores reflexivity: under this definition, `Any` is a subtype of `Any`, and for any equivalent types `S` and `T`, `S <: T` and `T <: S`. But unfortunately, this definition breaks transitivity of subtyping, because nominal subclasses in Python use assignability ("consistent subtyping") to define acceptable overrides. This means that we may have a class `A` with `def method(self) -> Any` and a subtype `B(A)` with `def method(self) -> int`, since `int` is assignable to `Any`. This means that if we have a protocol `P` with `def method(self) -> Any`, we would have `B <: A` (from nominal subtyping) and `A <: P` (`Any` is a subtype of `Any`), but not `B <: P` (`int` is not a subtype of `Any`). Breaking transitivity of subtyping is not tenable, so we don't use this definition of subtyping.

## Test Plan

Existing tests (modified in some cases to account for updated semantics.)

Stable property tests pass at a million iterations: `QUICKCHECK_TESTS=1000000 cargo test -p ty_python_semantic -- --ignored types::property_tests::stable`

### Changes to property test type generation

Since we no longer have a method of categorizing built types as fully-static or not-fully-static, I had to add a previously-discussed feature to the property tests so that some tests can build types that are known by construction to be fully static, because there are still properties that only apply to fully-static types (for example, reflexiveness of subtyping.)

## Changes to handling of `*args, **kwargs` signatures

This PR "discovered" that, once we allow non-fully-static types to participate in subtyping under the above definitions, `(*args: Any, **kwargs: Any) -> Any` is now a subtype of `() -> object`. This is true, if we take a literal interpretation of the former signature: all materializations of the parameters `*args: Any, **kwargs: Any` can accept zero arguments, making the former signature a subtype of the latter. But the spec actually says that `*args: Any, **kwargs: Any` should be interpreted as equivalent to `...`, and that makes a difference here: `(...) -> Any` is not a subtype of `() -> object`, because (unlike a literal reading of `(*args: Any, **kwargs: Any)`), `...` can materialize to _any_ signature, including a signature with required positional arguments.

This matters for this PR because it makes the "any two types are both assignable to their union" property test fail if we don't implement the equivalence to `...`. Because `FunctionType.__call__` has the signature `(*args: Any, **kwargs: Any) -> Any`, and if we take that at face value it's a subtype of `() -> object`, making `FunctionType` a subtype of `() -> object)` -- but then a function with a required argument is also a subtype of `FunctionType`, but not a subtype of `() -> object`. So I went ahead and implemented the equivalence to `...` in this PR.

## Ecosystem analysis

* Most of the ecosystem report are cases of improved union/intersection simplification. For example, we can now simplify a union like `bool | (bool & Unknown) | Unknown` to simply `bool | Unknown`, because we can now observe that every possible materialization of `bool & Unknown` is still a subtype of `bool` (whereas before we would set aside `bool & Unknown` as a not-fully-static type.) This is clearly an improvement.
* The `possibly-unresolved-reference` errors in sockeye, pymongo, ignite, scrapy and others are true positives for conditional imports that were formerly silenced by bogus conflicting-declarations (which we currently don't issue a diagnostic for), because we considered two different declarations of `Unknown` to be conflicting (we used `is_equivalent_to` not `is_gradual_equivalent_to`). In this PR that distinction disappears and all equivalence is gradual, so a declaration of `Unknown` no longer conflicts with a declaration of `Unknown`, which then results in us surfacing the possibly-unbound error.
* We will now issue "redundant cast" for casting from a typevar with a gradual bound to the same typevar (the hydra-zen diagnostic). This seems like an improvement.
* The new diagnostics in bandersnatch are interesting. For some reason primer in CI seems to be checking bandersnatch on Python 3.10 (not yet sure why; this doesn't happen when I run it locally). But bandersnatch uses `enum.StrEnum`, which doesn't exist on 3.10. That makes the `class SimpleDigest(StrEnum)` a class that inherits from `Unknown` (and bypasses our current TODO handling for accessing attributes on enum classes, since we don't recognize it as an enum class at all). This PR improves our understanding of assignability to classes that inherit from `Any` / `Unknown`, and we now recognize that a string literal is not assignable to a class inheriting `Any` or `Unknown`.


---

_Comment by @github-actions[bot] on 2025-06-19 17:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- error[invalid-argument-type] src/trio/testing/_fake_net.py:330:56: Argument to bound method `from_python_sockaddr` is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `(@Todo(Support for `typing.TypeAlias`) & None) | None | @Todo(generic `typing.Awaitable` type)`
+ error[invalid-argument-type] src/trio/testing/_fake_net.py:330:56: Argument to bound method `from_python_sockaddr` is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `None | @Todo(generic `typing.Awaitable` type)`

comtypes (https://github.com/enthought/comtypes)
- error[unresolved-attribute] comtypes/_vtbl.py:123:5: Unresolved attribute `has_outargs` on type `def call_with_this(*args, **kw) -> Unknown`.
+ error[unresolved-attribute] comtypes/_vtbl.py:123:5: Unresolved attribute `has_outargs` on type `def call_with_this(...) -> Unknown`.

sockeye (https://github.com/awslabs/sockeye)
+ warning[possibly-unresolved-reference] sockeye/model.py:512:22: Name `faiss` used when possibly not defined
- Found 340 diagnostics
+ Found 341 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- error[invalid-return-type] bson/regex.py:83:16: Return type does not match returned value: expected `Regex[_T]`, found `Regex[str]`
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:235:31: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:236:50: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:243:31: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:245:30: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:251:27: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:251:72: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:253:22: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:261:16: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:268:23: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:279:26: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:283:27: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:292:30: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:299:16: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:302:16: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:302:48: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:305:23: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:314:13: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/asynchronous/auth.py:316:12: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:232:31: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:233:50: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:240:31: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:242:30: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:248:27: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:248:72: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:250:22: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:258:16: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:265:23: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:276:26: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:280:27: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:289:30: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:296:16: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:299:16: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:299:48: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:302:23: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:311:13: Name `kerberos` used when possibly not defined
+ warning[possibly-unresolved-reference] pymongo/synchronous/auth.py:313:12: Name `kerberos` used when possibly not defined
- Found 509 diagnostics
+ Found 544 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[conflicting-declarations] strawberry/relay/types.py:863:21: Conflicting declared types for `edges`: list[Edge[Unknown]], list[Edge[Unknown]]
- error[conflicting-declarations] strawberry/relay/types.py:869:21: Conflicting declared types for `edges`: list[Edge[Unknown]], list[Edge[Unknown]]
- Found 371 diagnostics
+ Found 369 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ warning[redundant-cast] src/hydra_zen/third_party/beartype.py:130:12: Value is already of type `_T`
- Found 597 diagnostics
+ Found 598 diagnostics

asynq (https://github.com/quora/asynq)
- error[unresolved-attribute] asynq/decorators.py:92:9: Unresolved attribute `is_pure_async_fn` on type `def sync_to_async_fn_wrapper(*args, **kwargs) -> Unknown`.
+ error[unresolved-attribute] asynq/decorators.py:92:9: Unresolved attribute `is_pure_async_fn` on type `def sync_to_async_fn_wrapper(...) -> Unknown`.

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[unresolved-attribute] tests/test_misc.py:187:13: Unresolved attribute `communicate` on type `def fake_popen(*args, **kwargs) -> Unknown`.
+ error[unresolved-attribute] tests/test_misc.py:187:13: Unresolved attribute `communicate` on type `def fake_popen(...) -> Unknown`.
- error[unresolved-attribute] tests/test_misc.py:189:13: Unresolved attribute `returncode` on type `def fake_popen(*args, **kwargs) -> Unknown`.
+ error[unresolved-attribute] tests/test_misc.py:189:13: Unresolved attribute `returncode` on type `def fake_popen(...) -> Unknown`.
- error[unresolved-attribute] tests/test_misc.py:190:13: Unresolved attribute `called` on type `def fake_popen(*args, **kwargs) -> Unknown`.
+ error[unresolved-attribute] tests/test_misc.py:190:13: Unresolved attribute `called` on type `def fake_popen(...) -> Unknown`.

bandersnatch (https://github.com/pypa/bandersnatch)
+ error[invalid-argument-type] src/bandersnatch/tests/test_configuration.py:141:13: Argument is incorrect: Expected `SimpleDigest`, found `Unknown | Literal["sha256"]`
+ error[invalid-argument-type] src/bandersnatch/tests/test_configuration.py:161:13: Argument is incorrect: Expected `SimpleDigest`, found `Unknown | Literal["sha256"]`
+ error[invalid-argument-type] src/bandersnatch/tests/test_configuration.py:184:13: Argument is incorrect: Expected `SimpleDigest`, found `Unknown | Literal["sha256"]`
- Found 125 diagnostics
+ Found 128 diagnostics

ignite (https://github.com/pytorch/ignite)
+ warning[possibly-unresolved-reference] ignite/distributed/comp_models/horovod.py:154:13: Name `hvd_mp_spawn` used when possibly not defined
- Found 2351 diagnostics
+ Found 2352 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[conflicting-declarations] src/urllib3/response.py:27:5: Conflicting declared types for `brotli`: Unknown, Unknown
+ warning[possibly-unbound-attribute] src/urllib3/response.py:137:25: Attribute `Decompressor` on type `Unknown | None` is possibly unbound
+ warning[possibly-unresolved-reference] src/urllib3/response.py:157:25: Name `zstd` used when possibly not defined
+ warning[possibly-unresolved-reference] src/urllib3/response.py:165:29: Name `zstd` used when possibly not defined
+ warning[possibly-unresolved-reference] src/urllib3/response.py:195:29: Name `zstd` used when possibly not defined
+ warning[possibly-unresolved-reference] src/urllib3/response.py:203:33: Name `zstd` used when possibly not defined
- error[conflicting-declarations] test/__init__.py:26:5: Conflicting declared types for `brotli`: Unknown, Unknown
+ warning[possibly-unbound-attribute] test/test_response.py:394:16: Attribute `compress` on type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_response.py:402:16: Attribute `compress` on type `Unknown | None` is possibly unbound
- Found 509 diagnostics
+ Found 514 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ warning[possibly-unresolved-reference] scrapy/utils/_compression.py:87:20: Name `brotli` used when possibly not defined
- Found 1154 diagnostics
+ Found 1155 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- error[no-matching-overload] freqtrade/strategy/strategy_wrapper.py:26:21: No overload of function `getattr` matches arguments
- Found 437 diagnostics
+ Found 436 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- warning[possibly-unbound-attribute] github/MainClass.py:542:39: Attribute `strftime` on type `@Todo(unknown type subscript) | (_NotSetType & Unknown) | _NotSetType | (@Todo(unknown type subscript) & datetime)` is possibly unbound
+ warning[possibly-unbound-attribute] github/MainClass.py:542:39: Attribute `strftime` on type `@Todo(unknown type subscript) | _NotSetType | (@Todo(unknown type subscript) & datetime)` is possibly unbound
- warning[possibly-unbound-attribute] github/MainClass.py:904:42: Attribute `_identity` on type `@Todo(unknown type subscript) | (_NotSetType & Unknown) | _NotSetType` is possibly unbound
+ warning[possibly-unbound-attribute] github/MainClass.py:904:42: Attribute `_identity` on type `@Todo(unknown type subscript) | _NotSetType` is possibly unbound
- warning[possibly-unbound-attribute] github/Milestone.py:200:41: Attribute `strftime` on type `@Todo(unknown type subscript) | (_NotSetType & Unknown) | _NotSetType | (@Todo(unknown type subscript) & date)` is possibly unbound
+ warning[possibly-unbound-attribute] github/Milestone.py:200:41: Attribute `strftime` on type `@Todo(unknown type subscript) | _NotSetType | (@Todo(unknown type subscript) & date)` is possibly unbound
- warning[possibly-unbound-attribute] github/NamedUser.py:444:39: Attribute `strftime` on type `@Todo(unknown type subscript) | (_NotSetType & Unknown) | _NotSetType | (@Todo(unknown type subscript) & datetime)` is possibly unbound
+ warning[possibly-unbound-attribute] github/NamedUser.py:444:39: Attribute `strftime` on type `@Todo(unknown type subscript) | _NotSetType | (@Todo(unknown type subscript) & datetime)` is possibly unbound

discord.py (https://github.com/Rapptz/discord.py)
- error[invalid-argument-type] discord/ext/commands/converter.py:1122:16: Argument to function `len` is incorrect: Expected `Sized`, found `(tuple[T] & tuple[Unknown, ...]) | (T & tuple[Unknown, ...]) | tuple[T] | T`
+ error[invalid-argument-type] discord/ext/commands/converter.py:1122:16: Argument to function `len` is incorrect: Expected `Sized`, found `tuple[T] | T`
+ warning[possibly-unresolved-reference] discord/voice_client.py:388:15: Name `nacl` used when possibly not defined
+ warning[possibly-unresolved-reference] discord/voice_client.py:399:15: Name `nacl` used when possibly not defined
+ warning[possibly-unresolved-reference] discord/voice_client.py:408:15: Name `nacl` used when possibly not defined
+ warning[possibly-unresolved-reference] discord/voice_client.py:409:17: Name `nacl` used when possibly not defined
+ warning[possibly-unresolved-reference] discord/voice_client.py:409:35: Name `nacl` used when possibly not defined
+ warning[possibly-unresolved-reference] discord/voice_client.py:416:15: Name `nacl` used when possibly not defined
- Found 566 diagnostics
+ Found 572 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- warning[possibly-unbound-attribute] src/_pytest/python.py:1310:37: Attribute `stash` on type `Node | None | (Unknown & Module) | @Todo(map_with_boundness: intersections with negative contributions)` is possibly unbound
+ warning[possibly-unbound-attribute] src/_pytest/python.py:1310:37: Attribute `stash` on type `Node | None | @Todo(map_with_boundness: intersections with negative contributions)` is possibly unbound

cwltool (https://github.com/common-workflow-language/cwltool)
- warning[possibly-unbound-attribute] cwltool/main.py:455:9: Attribute `update` on type `(Unknown & None) | None | Unknown | MutableMapping[str, @Todo(Inference of subscript on special form) | None] | dict[Unknown, Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] cwltool/main.py:455:9: Attribute `update` on type `None | Unknown | MutableMapping[str, @Todo(Inference of subscript on special form) | None] | dict[Unknown, Unknown]` is possibly unbound

aiohttp (https://github.com/aio-libs/aiohttp)
+ warning[possibly-unresolved-reference] aiohttp/compression_utils.py:280:21: Name `brotli` used when possibly not defined
- Found 137 diagnostics
+ Found 138 diagnostics

paasta (https://github.com/yelp/paasta)
- error[invalid-argument-type] paasta_tools/api/views/autoscaler.py:106:17: Argument to bound method `set_autoscaled_instances` is incorrect: Expected `int`, found `(Unknown & int) | int | None`
+ error[invalid-argument-type] paasta_tools/api/views/autoscaler.py:106:17: Argument to bound method `set_autoscaled_instances` is incorrect: Expected `int`, found `int | None`
- error[conflicting-declarations] paasta_tools/clusterman.py:18:9: Conflicting declared types for `clusterman_metrics`: Unknown, Unknown
- Found 937 diagnostics
+ Found 936 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- error[type-assertion-failure] tests/test_frame.py:1090:9: Argument does not have asserted type `Series[Any]`
- error[type-assertion-failure] tests/test_frame.py:1094:9: Argument does not have asserted type `Series[Any]`
- error[type-assertion-failure] tests/test_frame.py:1098:9: Argument does not have asserted type `Series[Any]`
- error[type-assertion-failure] tests/test_frame.py:1157:9: Argument does not have asserted type `Series[Any]`
- error[type-assertion-failure] tests/test_frame.py:1163:9: Argument does not have asserted type `Series[Any]`
- error[type-assertion-failure] tests/test_frame.py:3893:11: Argument does not have asserted type `Series[Any]`
- error[type-assertion-failure] tests/test_series.py:3660:9: Argument does not have asserted type `Series[Any]`
- Found 2779 diagnostics
+ Found 2772 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- error[conflicting-declarations] lib/Crypto/Math/Numbers.py:47:9: Conflicting declared types for `_implementation`: Unknown, Unknown
- Found 1543 diagnostics
+ Found 1542 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- error[conflicting-declarations] openlibrary/core/helpers.py:21:5: Conflicting declared types for `genshi`: Unknown, Unknown
- Found 719 diagnostics
+ Found 718 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ error[invalid-argument-type] lib/streamlit/elements/lib/built_in_chart_utils.py:195:9: Argument is incorrect: Expected `PrepDataColumns`, found `dict[Unknown, Unknown]`
- error[invalid-return-type] lib/streamlit/runtime/fragment.py:450:12: Return type does not match returned value: expected `((F, /) -> F) | F`, found `((F | None, /) -> F | None) | F | None`
- error[invalid-return-type] lib/streamlit/runtime/fragment.py:478:12: Return type does not match returned value: expected `((F, /) -> F) | F`, found `((F | None, /) -> F | None) | F | None`
- Found 3275 diagnostics
+ Found 3274 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- warning[possibly-unbound-attribute] src/integrations/prefect-dbt/prefect_dbt/cloud/models.py:20:44: Attribute `name` on type `(FlowRun & Unknown) | (None & Unknown) | Unknown | FlowRun | None` is possibly unbound
+ warning[possibly-unbound-attribute] src/integrations/prefect-dbt/prefect_dbt/cloud/models.py:20:44: Attribute `name` on type `FlowRun | None | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/server/orchestration/global_policy.py:460:13: Attribute `state_details` on type `Unknown | (Unknown & None) | (State & @Todo(specialized non-generic class)) | None` is possibly unbound
+ warning[possibly-unbound-attribute] src/prefect/server/orchestration/global_policy.py:460:13: Attribute `state_details` on type `Unknown | None | (State & @Todo(specialized non-generic class))` is possibly unbound
- error[invalid-argument-type] src/prefect/testing/utilities.py:245:9: Argument to bound method `aread` is incorrect: Expected `str`, found `str | None | (Any & str) | (Any & None) | @Todo(map_with_boundness: intersections with negative contributions)`
+ error[invalid-argument-type] src/prefect/testing/utilities.py:245:9: Argument to bound method `aread` is incorrect: Expected `str`, found `str | None | @Todo(map_with_boundness: intersections with negative contributions)`
- error[invalid-argument-type] src/prefect/testing/utilities.py:262:50: Argument to bound method `read_block_document` is incorrect: Expected `UUID`, found `UUID | None | (Any & UUID) | (Any & None)`
+ error[invalid-argument-type] src/prefect/testing/utilities.py:262:50: Argument to bound method `read_block_document` is incorrect: Expected `UUID`, found `UUID | None`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[unresolved-attribute] benchmarks/bm/iast_fixtures/str_methods.py:703:13: Unresolved attribute `s_variables` on type `def wrapper(*args, **kwargs) -> Unknown`.
+ error[unresolved-attribute] benchmarks/bm/iast_fixtures/str_methods.py:703:13: Unresolved attribute `s_variables` on type `def wrapper(...) -> Unknown`.
+ warning[possibly-unresolved-reference] ddtrace/contrib/internal/aiobotocore/patch.py:161:77: Name `ClientResponseContentProxy` used when possibly not defined
- error[conflicting-declarations] ddtrace/contrib/internal/langchain/patch.py:36:9: Conflicting declared types for `get_openai_token_cost_for_model`: Unknown, Unknown
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/lex.py:221:31: Attribute `_lextokens` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/lex.py:222:31: Attribute `_lexreflags` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/lex.py:223:31: Attribute `_lexliterals` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/lex.py:225:31: Attribute `_lexstateinfo` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/lex.py:226:31: Attribute `_lexstateignore` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/lex.py:229:31: Attribute `_lexstatere` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/lex.py:233:47: Attribute `_lexreflags` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/lex.py:239:30: Attribute `_lexstateerrorf` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/lex.py:243:30: Attribute `_lexstateeoff` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
+ error[unresolved-attribute] ddtrace/vendor/ply/lex.py:221:31: Type `ModuleType` has no attribute `_lextokens`
+ error[unresolved-attribute] ddtrace/vendor/ply/lex.py:222:31: Type `ModuleType` has no attribute `_lexreflags`
+ error[unresolved-attribute] ddtrace/vendor/ply/lex.py:223:31: Type `ModuleType` has no attribute `_lexliterals`
+ error[unresolved-attribute] ddtrace/vendor/ply/lex.py:225:31: Type `ModuleType` has no attribute `_lexstateinfo`
+ error[unresolved-attribute] ddtrace/vendor/ply/lex.py:226:31: Type `ModuleType` has no attribute `_lexstateignore`
+ error[unresolved-attribute] ddtrace/vendor/ply/lex.py:229:31: Type `ModuleType` has no attribute `_lexstatere`
+ error[unresolved-attribute] ddtrace/vendor/ply/lex.py:233:47: Type `ModuleType` has no attribute `_lexreflags`
+ error[unresolved-attribute] ddtrace/vendor/ply/lex.py:239:30: Type `ModuleType` has no attribute `_lexstateerrorf`
+ error[unresolved-attribute] ddtrace/vendor/ply/lex.py:243:30: Type `ModuleType` has no attribute `_lexstateeoff`
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:1987:12: Attribute `_tabversion` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:1990:26: Attribute `_lr_action` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:1991:24: Attribute `_lr_goto` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:1994:18: Attribute `_lr_productions` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
+ error[unresolved-attribute] ddtrace/vendor/ply/yacc.py:1987:12: Type `ModuleType` has no attribute `_tabversion`
+ error[unresolved-attribute] ddtrace/vendor/ply/yacc.py:1990:26: Type `ModuleType` has no attribute `_lr_action`
+ error[unresolved-attribute] ddtrace/vendor/ply/yacc.py:1991:24: Type `ModuleType` has no attribute `_lr_goto`
+ error[unresolved-attribute] ddtrace/vendor/ply/yacc.py:1994:18: Type `ModuleType` has no attribute `_lr_productions`
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:1997:26: Attribute `_lr_method` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:1998:16: Attribute `_lr_signature` on type `(Unknown & ModuleType) | ModuleType` is possibly unbound
+ error[unresolved-attribute] ddtrace/vendor/ply/yacc.py:1997:26: Type `ModuleType` has no attribute `_lr_method`
+ error[unresolved-attribute] ddtrace/vendor/ply/yacc.py:1998:16: Type `ModuleType` has no attribute `_lr_signature`
- error[invalid-assignment] ddtrace/vendor/psutil/_compat.py:254:13: Object of type `Unknown` is not assignable to attribute `__wrapped__` on type `(def wrapper(*args, **kwds) -> Unknown) | (def wrapper(*args, **kwds) -> Unknown) | (def wrapper(*args, **kwds) -> Unknown)`
+ error[invalid-assignment] ddtrace/vendor/psutil/_compat.py:254:13: Object of type `Unknown` is not assignable to attribute `__wrapped__` on type `(def wrapper(...) -> Unknown) | (def wrapper(...) -> Unknown) | (def wrapper(...) -> Unknown)`
- error[invalid-assignment] ddtrace/vendor/psutil/_compat.py:255:13: Object of type `def cache_info() -> Unknown` is not assignable to attribute `cache_info` on type `(def wrapper(*args, **kwds) -> Unknown) | (def wrapper(*args, **kwds) -> Unknown) | (def wrapper(*args, **kwds) -> Unknown)`
+ error[invalid-assignment] ddtrace/vendor/psutil/_compat.py:255:13: Object of type `def cache_info() -> Unknown` is not assignable to attribute `cache_info` on type `(def wrapper(...) -> Unknown) | (def wrapper(...) -> Unknown) | (def wrapper(...) -> Unknown)`
- error[invalid-assignment] ddtrace/vendor/psutil/_compat.py:256:13: Object of type `def cache_clear() -> Unknown` is not assignable to attribute `cache_clear` on type `(def wrapper(*args, **kwds) -> Unknown) | (def wrapper(*args, **kwds) -> Unknown) | (def wrapper(*args, **kwds) -> Unknown)`
+ error[invalid-assignment] ddtrace/vendor/psutil/_compat.py:256:13: Object of type `def cache_clear() -> Unknown` is not assignable to attribute `cache_clear` on type `(def wrapper(...) -> Unknown) | (def wrapper(...) -> Unknown) | (def wrapper(...) -> Unknown)`
- warning[possibly-unbound-attribute] tests/profiling_v2/collector/test_stack.py:754:13: Attribute `ignore_profiler` on type `(Unknown & StackCollector) | StackCollector` is possibly unbound
+ error[unresolved-attribute] tests/profiling_v2/collector/test_stack.py:754:13: Type `StackCollector` has no attribute `ignore_profiler`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/tests/test_grower.py:155:12: Attribute `value` on type `(Unknown & None) | None` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/tests/test_grower.py:156:12: Attribute `left_child` on type `(Unknown & None) | None` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/tests/test_grower.py:157:12: Attribute `right_child` on type `(Unknown & None) | None` is possibly unbound
+ error[unresolved-attribute] sklearn/ensemble/_hist_gradient_boosting/tests/test_grower.py:155:12: Type `None` has no attribute `value`
+ error[unresolved-attribute] sklearn/ensemble/_hist_gradient_boosting/tests/test_grower.py:156:12: Type `None` has no attribute `left_child`
+ error[unresolved-attribute] sklearn/ensemble/_hist_gradient_boosting/tests/test_grower.py:157:12: Type `None` has no attribute `right_child`
- error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:511:25: Argument to function `len` is incorrect: Expected `Sized`, found `(tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]`
+ error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:511:25: Argument to function `len` is incorrect: Expected `Sized`, found `tuple[int, ...] | int`
- error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:512:28: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | int | (tuple[int, ...] & ~tuple[()])`
+ error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:512:28: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & ~tuple[()]) | int`
- error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:512:49: Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | int | (tuple[int, ...] & ~tuple[()])`
+ error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:512:49: Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & ~tuple[()]) | int`
- error[not-iterable] sklearn/externals/array_api_extra/_lib/_funcs.py:517:40: Object of type `(tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]` may not be iterable
+ error[not-iterable] sklearn/externals/array_api_extra/_lib/_funcs.py:517:40: Object of type `tuple[int, ...] | int` may not be iterable
- warning[possibly-unbound-attribute] sklearn/linear_model/tests/test_ridge.py:554:12: Attribute `shape` on type `(Unknown & float) | float` is possibly unbound
+ error[unresolved-attribute] sklearn/linear_model/tests/test_ridge.py:554:12: Type `float` has no attribute `shape`
- error[unresolved-attribute] sklearn/utils/_metadata_requests.py:1353:9: Unresolved attribute `__signature__` on type `def func(*args, **kw) -> Unknown`.
+ error[unresolved-attribute] sklearn/utils/_metadata_requests.py:1353:9: Unresolved attribute `__signature__` on type `def func(...) -> Unknown`.
+ warning[possibly-unresolved-reference] sklearn/utils/validation.py:924:42: Name `SparseDtype` used when possibly not defined
+ warning[possibly-unresolved-reference] sklearn/utils/validation.py:1002:42: Name `SparseDtype` used when possibly not defined
- Found 2293 diagnostics
+ Found 2295 diagnostics

scipy (https://github.com/scipy/scipy)
- error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:567:25: Argument to function `len` is incorrect: Expected `Sized`, found `(tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]`
+ error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:567:25: Argument to function `len` is incorrect: Expected `Sized`, found `tuple[int, ...] | int`
- error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:28: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | int | (tuple[int, ...] & ~tuple[()])`
+ error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:28: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & ~tuple[()]) | int`
- error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:49: Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | int | (tuple[int, ...] & ~tuple[()])`
+ error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:49: Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & ~tuple[()]) | int`
- error[not-iterable] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:573:40: Object of type `(tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]` may not be iterable
+ error[not-iterable] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:573:40: Object of type `tuple[int, ...] | int` may not be iterable
- error[invalid-assignment] scipy/_lib/decorator.py:239:5: Object of type `(Unknown & ~partial[Unknown]) | @Todo(specialized non-generic class)` is not assignable to attribute `__wrapped__` on type `(def fun(*args, **kw) -> Unknown) | (def fun(*args, **kw) -> Unknown) | (def fun(*args, **kw) -> Unknown)`
+ error[invalid-assignment] scipy/_lib/decorator.py:239:5: Object of type `(Unknown & ~partial[Unknown]) | @Todo(specialized non-generic class)` is not assignable to attribute `__wrapped__` on type `(def fun(...) -> Unknown) | (def fun(...) -> Unknown) | (def fun(...) -> Unknown)`
- error[invalid-assignment] scipy/_lib/decorator.py:240:5: Object of type `Signature` is not assignable to attribute `__signature__` on type `(def fun(*args, **kw) -> Unknown) | (def fun(*args, **kw) -> Unknown) | (def fun(*args, **kw) -> Unknown)`
+ error[invalid-assignment] scipy/_lib/decorator.py:240:5: Object of type `Signature` is not assignable to attribute `__signature__` on type `(def fun(...) -> Unknown) | (def fun(...) -> Unknown) | (def fun(...) -> Unknown)`
- warning[possibly-unbound-attribute] scipy/integrate/_quadrature.py:538:13: Attribute `reshape` on type `(Unknown & None) | None | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[possibly-unbound-attribute] scipy/integrate/_quadrature.py:538:13: Attribute `reshape` on type `None | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
- error[unresolved-attribute] scipy/optimize/tests/test_bracket.py:155:13: Type `def f(*args, **kwargs) -> Unknown` has no attribute `f_evals`
+ error[unresolved-attribute] scipy/optimize/tests/test_bracket.py:155:13: Type `def f(...) -> Unknown` has no attribute `f_evals`
- error[unresolved-attribute] scipy/optimize/tests/test_bracket.py:157:9: Unresolved attribute `f_evals` on type `def f(*args, **kwargs) -> Unknown`.
+ error[unresolved-attribute] scipy/optimize/tests/test_bracket.py:157:9: Unresolved attribute `f_evals` on type `def f(...) -> Unknown`.
- error[unresolved-attribute] scipy/optimize/tests/test_bracket.py:188:35: Type `def f(*args, **kwargs) -> Unknown` has no attribute `f_evals`
+ error[unresolved-attribute] scipy/optimize/tests/test_bracket.py:188:35: Type `def f(...) -> Unknown` has no attribute `f_evals`
- error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:228:13: Type `def f(*args, **kwargs) -> Unknown` has no attribute `f_evals`
+ error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:228:13: Type `def f(...) -> Unknown` has no attribute `f_evals`
- error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:230:9: Unresolved attribute `f_evals` on type `def f(*args, **kwargs) -> Unknown`.
+ error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:230:9: Unresolved attribute `f_evals` on type `def f(...) -> Unknown`.
- error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:247:36: Type `def f(*args, **kwargs) -> Unknown` has no attribute `f_evals`
+ error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:247:36: Type `def f(...) -> Unknown` has no attribute `f_evals`
- error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:248:35: Type `def f(*args, **kwargs) -> Unknown` has no attribute `f_evals`
+ error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:248:35: Type `def f(...) -> Unknown` has no attribute `f_evals`
- error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:581:13: Type `def f(*args, **kwargs) -> Unknown` has no attribute `f_evals`
+ error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:581:13: Type `def f(...) -> Unknown` has no attribute `f_evals`
- error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:583:9: Unresolved attribute `f_evals` on type `def f(*args, **kwargs) -> Unknown`.
+ error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:583:9: Unresolved attribute `f_evals` on type `def f(...) -> Unknown`.
- error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:610:40: Type `def f(*args, **kwargs) -> Unknown` has no attribute `f_evals`
+ error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:610:40: Type `def f(...) -> Unknown` has no attribute `f_evals`
- error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:619:39: Type `def f(*args, **kwargs) -> Unknown` has no attribute `f_evals`
+ error[unresolved-attribute] scipy/optimize/tests/test_chandrupatla.py:619:39: Type `def f(...) -> Unknown` has no attribute `f_evals`
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_eigen/lobpcg/lobpcg.py:121:16: Attribute `shape` on type `(Unknown & None) | None | @Todo(Type::Intersection.call())` is possibly unbound
+ warning[possibly-unbound-attribute] scipy/sparse/linalg/_eigen/lobpcg/lobpcg.py:121:16: Attribute `shape` on type `None | @Todo(Type::Intersection.call())` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_eigen/lobpcg/lobpcg.py:125:39: Attribute `shape` on type `(Unknown & None) | None | @Todo(Type::Intersection.call())` is possibly unbound
+ warning[possibly-unbound-attribute] scipy/sparse/linalg/_eigen/lobpcg/lobpcg.py:125:39: Attribute `shape` on type `None | @Todo(Type::Intersection.call())` is possibly unbound
- error[unresolved-attribute] scipy/stats/_mstats_basic.py:1919:7: Type `bool` has no attribute `filled`
+ warning[possibly-unbound-attribute] scipy/stats/_mstats_basic.py:1919:7: Attribute `filled` on type `bool | @Todo(Type::Intersection.call())` is possibly unbound

sympy (https://github.com/sympy/sympy)
- warning[possibly-unbound-attribute] sympy/core/exprtools.py:345:24: Attribute `p` on type `(Unknown & Number) | Number | @Todo(Type::Intersection.call())` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/exprtools.py:346:41: Attribute `p` on type `(Unknown & Number) | Number | @Todo(Type::Intersection.call())` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/exprtools.py:347:37: Attribute `q` on type `(Unknown & Number) | Number | @Todo(Type::Intersection.call())` is possibly unbound
+ error[unresolved-attribute] sympy/core/exprtools.py:345:24: Type `Number` has no attribute `p`
+ error[unresolved-attribute] sympy/core/exprtools.py:346:41: Type `Number` has no attribute `p`
+ error[unresolved-attribute] sympy/core/exprtools.py:347:37: Type `Number` has no attribute `q`
- error[unsupported-operator] sympy/core/tests/test_numbers.py:983:12: Operator `+` is unsupported between objects of type `(Unknown & ~Literal[1]) | (NaN & Unknown) | NaN` and `(Unknown & ~Literal[1]) | (NaN & Unknown) | NaN`
+ error[unsupported-operator] sympy/core/tests/test_numbers.py:983:12: Operator `+` is unsupported between objects of type `(Unknown & ~Literal[1]) | NaN` and `(Unknown & ~Literal[1]) | NaN`
- error[unsupported-operator] sympy/core/tests/test_numbers.py:984:19: Operator `*` is unsupported between objects of type `(Unknown & ~Literal[1]) | (NaN & Unknown) | NaN` and `Literal[-5]`
+ error[unsupported-operator] sympy/core/tests/test_numbers.py:984:19: Operator `*` is unsupported between objects of type `(Unknown & ~Literal[1]) | NaN` and `Literal[-5]`
- error[unsupported-operator] sympy/core/tests/test_numbers.py:1001:16: Operator `+` is unsupported between objects of type `(Unknown & ~Literal[1]) | (NaN & Unknown) | NaN` and `Literal[1] | float | Unknown | One | NegativeOne | Float`
+ error[unsupported-operator] sympy/core/tests/test_numbers.py:1001:16: Operator `+` is unsupported between objects of type `(Unknown & ~Literal[1]) | NaN` and `Literal[1] | float | Unknown | One | NegativeOne | Float`
- error[unsupported-operator] sympy/core/tests/test_numbers.py:1002:16: Operator `-` is unsupported between objects of type `(Unknown & ~Literal[1]) | (NaN & Unknown) | NaN` and `Literal[1] | float | Unknown | One | NegativeOne | Float`
+ error[unsupported-operator] sympy/core/tests/test_numbers.py:1002:16: Operator `-` is unsupported between objects of type `(Unknown & ~Literal[1]) | NaN` and `Literal[1] | float | Unknown | One | NegativeOne | Float`
- error[unsupported-operator] sympy/core/tests/test_numbers.py:1004:16: Operator `/` is unsupported between objects of type `(Unknown & ~Literal[1]) | (NaN & Unknown) | NaN` and `Literal[1] | float | Unknown | One | NegativeOne | Float`
+ error[unsupported-operator] sympy/core/tests/test_numbers.py:1004:16: Operator `/` is unsupported between objects of type `(Unknown & ~Literal[1]) | NaN` and `Literal[1] | float | Unknown | One | NegativeOne | Float`
- warning[possibly-unbound-attribute] sympy/geometry/line.py:1675:45: Attribute `x` on type `(Unknown & Point) | Point` is possibly unbound
- warning[possibly-unbound-attribute] sympy/geometry/line.py:1675:67: Attribute `x` on type `(Unknown & Point) | Point` is possibly unbound
- warning[possibly-unbound-attribute] sympy/geometry/line.py:1679:45: Attribute `y` on type `(Unknown & Point) | Point` is possibly unbound
- warning[possibly-unbound-attribute] sympy/geometry/line.py:1679:67: Attribute `y` on type `(Unknown & Point) | Point` is possibly unbound
+ error[unresolved-attribute] sympy/geometry/line.py:1675:45: Type `Point` has no attribute `x`
+ error[unresolved-attribute] sympy/geometry/line.py:1675:67: Type `Point` has no attribute `x`
+ error[unresolved-attribute] sympy/geometry/line.py:1679:45: Type `Point` has no attribute `y`
+ error[unresolved-attribute] sympy/geometry/line.py:1679:67: Type `Point` has no attribute `y`
- error[not-iterable] sympy/holonomic/holonomic.py:1055:22: Object of type `None | Unknown | (Unknown & None) | dict[Unknown, Unknown]` may not be iterable
+ error[not-iterable] sympy/holonomic/holonomic.py:1055:22: Object of type `None | Unknown | dict[Unknown, Unknown]` may not be iterable
- error[conflicting-declarations] sympy/interactive/printing.py:511:13: Conflicting declared types for `stringify_func`: Unknown, Unknown
- error[conflicting-declarations] sympy/interactive/printing.py:518:13: Conflicting declared types for `stringify_func`: Unknown, Unknown
- error[unsupported-operator] sympy/logic/algorithms/dpll2.py:44:8: Operator `in` is not supported for types `set[Unknown]` and `None`, in comparing `set[Unknown]` with `Unknown | (Unknown & None) | (Unknown & list[Unknown]) | None | list[Unknown]`
+ error[unsupported-operator] sympy/logic/algorithms/dpll2.py:44:8: Operator `in` is not supported for types `set[Unknown]` and `None`, in comparing `set[Unknown]` with `Unknown | None | list[Unknown]`
- error[unsupported-operator] sympy/logic/algorithms/dpll2.py:54:24: Operator `+` is unsupported between objects of type `Unknown | (Unknown & None) | (Unknown & list[Unknown]) | None | list[Unknown]` and `Unknown | list[Unknown]`
+ error[unsupported-operator] sympy/logic/algorithms/dpll2.py:54:24: Operator `+` is unsupported between objects of type `Unknown | None | list[Unknown]` and `Unknown | list[Unknown]`
- error[unsupported-operator] sympy/logic/algorithms/minisat22_wrapper.py:13:8: Operator `in` is not supported for types `set[Unknown]` and `None`, in comparing `set[Unknown]` with `Unknown | (Unknown & None) | (Unknown & list[Unknown]) | None | list[Unknown]`
+ error[unsupported-operator] sympy/logic/algorithms/minisat22_wrapper.py:13:8: Operator `in` is not supported for types `set[Unknown]` and `None`, in comparing `set[Unknown]` with `Unknown | None | list[Unknown]`
- error[unsupported-operator] sympy/logic/algorithms/pycosat_wrapper.py:12:8: Operator `in` is not supported for types `set[Unknown]` and `None`, in comparing `set[Unknown]` with `Unknown | (Unknown & None) | (Unknown & list[Unknown]) | None | list[Unknown]`
+ error[unsupported-operator] sympy/logic/algorithms/pycosat_wrapper.py:12:8: Operator `in` is not supported for types `set[Unknown]` and `None`, in comparing `set[Unknown]` with `Unknown | None | list[Unknown]`
- warning[possibly-unbound-attribute] sympy/physics/control/lti.py:2892:41: Attribute `is_StateSpace_object` on type `(Unknown & TransferFunction & ~AlwaysFalsy) | (Unknown & Series & ~AlwaysFalsy) | (Unknown & StateSpace & ~AlwaysFalsy) | (Unknown & Feedback & ~AlwaysFalsy) | TransferFunction` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/physics/control/lti.py:2892:41: Attribute `is_StateSpace_object` on type `TransferFunction | (Unknown & Series & ~AlwaysFalsy) | (Unknown & StateSpace & ~AlwaysFalsy) | (Unknown & Feedback & ~AlwaysFalsy)` is possibly unbound
- error[not-iterable] sympy/printing/numpy.py:120:51: Object of type `@Todo(Subscript expressions on intersections) | Basic` may not be iterable
+ error[not-iterable] sympy/printing/numpy.py:120:51: Object of type `Basic` is not iterable
- warning[possibly-unbound-attribute] sympy/solvers/ode/subscheck.py:157:17: Attribute `getO` on type `(Unknown & Basic) | Unknown | Basic` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/solvers/ode/subscheck.py:157:17: Attribute `getO` on type `Basic | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] sympy/solvers/ode/subscheck.py:158:18: Attribute `removeO` on type `(Unknown & Basic) | Unknown | Basic` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/solvers/ode/subscheck.py:158:18: Attribute `removeO` on type `Basic | Unknown` is possibly unbound
- error[unsupported-operator] sympy/solvers/ode/subscheck.py:165:20: Operator `-` is unsupported between objects of type `(Unknown & Basic) | Unknown | Basic` and `(Unknown & Basic) | Unknown | Basic`
+ error[unsupported-operator] sympy/solvers/ode/subscheck.py:165:20: Operator `-` is unsupported between objects of type `Basic | Unknown` and `Basic | Unknown`
- error[unsupported-operator] sympy/solvers/ode/subscheck.py:180:24: Operator `-` is unsupported between objects of type `(Unknown & Basic) | Unknown | Basic` and `(Unknown & Basic) | Unknown | Basic`
+ error[unsupported-operator] sympy/solvers/ode/subscheck.py:180:24: Operator `-` is unsupported between objects of type `Basic | Unknown` and `Basic | Unknown`
- error[unsupported-operator] sympy/solvers/pde.py:451:15: Operator `-` is unsupported between objects of type `(Unknown & Basic) | Unknown | Basic` and `(Unknown & Basic) | Unknown | Basic`
+ error[unsupported-operator] sympy/solvers/pde.py:451:15: Operator `-` is unsupported between objects of type `Basic | Unknown` and `Basic | Unknown`
- error[unsupported-operator] sympy/solvers/simplex.py:1068:77: Operator `*` is unsupported between objects of type `(Unknown & None) | None | @Todo(list comprehension type)` and `Unknown | MutableDenseMatrix`
+ error[unsupported-operator] sympy/solvers/simplex.py:1068:77: Operator `*` is unsupported between objects of type `None | @Todo(list comprehension type)` and `Unknown | MutableDenseMatrix`
- error[invalid-argument-type] sympy/solvers/simplex.py:1068:84: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & None) | None | @Todo(list comprehension type)`
+ error[invalid-argument-type] sympy/solvers/simplex.py:1068:84: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `None | @Todo(list comprehension type)`
- error[invalid-argument-type] sympy/testing/runtests_pytest.py:453:42: Argument to function `update_args_with_paths` is incorrect: Expected `tuple[str] | None`, found `tuple[Unknown, ...] | tuple[()]`
- error[index-out-of-bounds] sympy/vector/implicitregion.py:466:48: Index 0 is out of bounds for tuple `tuple[()]` with length 0
- error[index-out-of-bounds] sympy/vector/implicitregion.py:467:48: Index 1 is out of bounds for tuple `tuple[()]` with length 0
- error[index-out-of-bounds] sympy/vector/implicitregion.py:485:48: Index 0 is out of bounds for tuple `tuple[()]` with length 0
- error[index-out-of-bounds] sympy/vector/implicitregion.py:486:48: Index 1 is out of bounds for tuple `tuple[()]` with length 0
- error[index-out-of-bounds] sympy/vector/implicitregion.py:487:48: Index 2 is out of bounds for tuple `tuple[()]` with length 0
- Found 17919 diagnostics
+ Found 17911 diagnostics

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-06-19 18:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fnofullystatic?runnerMode=Instrumentation)

### Merging #18799 will **improve performances by 61.94%**

<sub>Comparing <code>cjm/nofullystatic</code> (271062e) with <code>main</code> (66f50fb)</sub>



### Summary

`⚡ 1` improvements  
`✅ 36` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_micro[many_tuple_assignments] `` | 207.4 ms | 128.1 ms | +61.94% |


---

_Label `ty` added by @AlexWaygood on 2025-06-19 21:06_

---

_Comment by @JelleZijlstra on 2025-06-20 03:29_

> we can usefully consider two gradual types S and T to have a subtype relation S <: T if all possible materializations of S are a subtype of all possible materializations of T

This seems to imply that Any is not a subtype of itself, because there are materializations of Any that are not subtypes of other materializations of Any.

I think the right formulation is the following: given two gradual types `A` and `B`,
`A` is a subtype of `B` if every materialization of `A` is a subtype of a materialization of `B`, and every materialization of `B` is a supertype of a materialization of `A`. But I'm still thinking about whether this is right; in particular, I want to prove that this definition allows us to derive that if A <: B, then A | B is equivalent to B.

---

_Comment by @carljm on 2025-06-20 03:57_

> This seems to imply that Any is not a subtype of itself

Yes, it didn't seem correct to me to consider `Any` a subtype of itself (presuming two unrelated `Any`), because we don't have enough information to say that either one is definitely a superset of the other. But I think you may be right that this is still a stricter approach than is necessary, for purposes of simplifying unions and intersections.

We do (in this PR, and before it) simplify `Any | Any` to `Any` in a union, but this simplification is not based on subtyping, it's based on gradual equivalence, which means that `Any | Any` doesn't provide any more information about the type than `Any` does.

> I think the right formulation is the following: given two gradual types `A` and `B`,
> `A` is a subtype of `B` if every materialization of `A` is a subtype of a materialization of `B`, and every materialization of `B` is a supertype of a materialization of `A`.

Yes, I can see the rationale here. If every materialization of `A` is a subtype of some materialization of `B`, then simplifying `A | B` to `B` cannot change the upper bound of possible materializations -- `A` did not contribute any materializations to the union that are not subsumed by some materialization of `B`. And the lower bound of possible materializations cannot change either, because for any small materialization we imagine for `A`, there is some materialization provided by `B` which is a supertype of it, and the union would resolve to that materialization from `B` anyway.

And I think dual arguments apply for simplifying `A & B` to `A`.

I will try out this definition tomorrow and see if there are any difficulties implementing it, and how it impacts this PR. It definitely has some nice properties, since it restores both reflexivity of subtyping, and also that equivalence implies subtyping. And it should make union/intersection simplification simpler, since it can be based only on subtyping, instead of requiring that we check both subtyping and equivalence.

---

_Comment by @carljm on 2025-06-20 04:02_

> `Any | Any` doesn't provide any more information about the type than `Any` does.

I think this is a key point. I was thinking of subtyping in terms of "can we say for sure that whatever type B materializes to is a subtype of whatever type A materializes to?" But if subtyping is really only used for simplifying unions and intersections, then we can instead define it in terms of "does this type contribute any information to the union/intersection that this other type doesn't?" or "can we simplify this union/intersection and still have an equivalent type?" -- and I think that leads us to your definition.

---

_Comment by @JelleZijlstra on 2025-06-20 05:34_

> If every materialization of A is a subtype of some materialization of B, then simplifying A | B to B cannot change the upper bound of possible materializations -- A did not contribute any materializations to the union that are not subsumed by some materialization of B. And the lower bound of possible materializations cannot change either, because for any small materialization we imagine for A, there is some materialization provided by B which is a supertype of it, and the union would resolve to that materialization from B anyway.

I feel a version of that argument would imply that we can also simplify `int | Any` to `Any`, which is wrong. That's why my definition includes the additional condition "every materialization of B is a supertype of a materialization of A" but I need to think more about why that makes sense.

Some other thoughts that I need to develop further but that may be of interest to you:

* I think gradual equivalence can be implemented as mutual gradual subtyping: that is, if A <: B and B <: A, then A is equivalent to B.
* I am using my definition of gradual subtyping in pycroscope for implementing overloads. I need to think about whether this algorithm is equivalent to that in the spec; if so, you can plausibly implement overloads without the "top materialization" trick.

And a general note: As you may have noticed, I recently nerdsniped myself into thinking a lot about the theory of the type system. It sounds like my ruminations have been useful for ty, but feel free to ignore my ideas if they distract you from building a practically useful type checker. :)

---

_Comment by @dcreager on 2025-06-20 12:48_

I'm excited about the performance improvement to `many_tuple_assignments`, since #18600 will claw some of it right back :sweat_smile: 

---

_Comment by @carljm on 2025-06-20 16:36_

> I feel a version of that argument would imply that we can also simplify `int | Any` to `Any`, which is wrong. That's why my definition includes the additional condition "every materialization of B is a supertype of a materialization of A" but I need to think more about why that makes sense.

Yes, I think my argument on the lower bound was incomplete; we need that additional condition to ensure the lower bound of the union type does not grow by the removal of `A`. If we imagine a small materialization of `B`, it must still be a super-type of some materialization of `A`, or else the lower bound of `A | B` grows by removing `A` from the union.

> * I think gradual equivalence can be implemented as mutual gradual subtyping: that is, if A <: B and B <: A, then A is equivalent to B.

Yes, with your definition of gradual subtyping, I think this is true.

> * I am using my definition of gradual subtyping in pycroscope for implementing overloads.

I am curious to hear more about how you use it. Is it just for step 5 (where we use top materialization)?

> It sounds like my ruminations have been useful for ty

Definitely!

---

_Comment by @JelleZijlstra on 2025-06-20 16:53_

> I am curious to hear more about how you use it. Is it just for step 5 (where we use top materialization)?

My approach predates the spec and doesn't follow it exactly, but it seems mostly compliant, though I need to test more. I haven't had time yet to verify whether it's actually equivalent to the spec. When I initially implemented this I wrote up an explanation at https://pyanalyze.readthedocs.io/en/latest/reference/signature.html#pyanalyze.signature.OverloadedSignature.check_call . Since then, I've changed the concept of a "match due to Any" to "a match that matches under assignability but not subtyping". This behaves similarly to step 5 in the spec but not sure if it's equivalent. I can write more about it when I get around to looking into this further.

---

_Comment by @carljm on 2025-06-20 19:23_

Recording a few observations here that arose in discussion with @sharkdp:

Let's write "top materialization of S" as `S+` and "bottom materialization of `S`" as `S-`.

The definition of subtyping I implemented here so far corresponds to saying that `S <: T` iff `S+ <: T-`. Your proposed definition corresponds to saying that `S <: T` iff `S+ <: T+` and `S- <: T-`.

It's interesting to note that neither definition is able to simplify `S | T` or `S & T` where `T- <: S-` and `S+ <: T+` (that is, `S` is a more precise type than `T`, with greater lower bound and lesser upper bound). In a system where all gradual types are represented by their top and bottom materialization, `S | T` could be simplified to a new gradual type defined by `(S-, T+)` and `S & T` could be simplified to a new gradual type defined by `(T-, S+)`.

(But this is all setting aside the fact that it's not clear how we can even define a bottom materialization for some Python gradual types, particularly invariant generics such as `list[Any]`.)

---

_Comment by @carljm on 2025-06-22 03:55_

I've implemented the expanded form of "gradual subtyping" discussed above. It mostly seems to work out well -- all tests pass, union/intersection builder can now be simplified to use only subtyping and not a separate equivalence check, reflexivity of subtyping is restored for all types.

But the property tests do reveal one problem: transitivity of subtyping is now broken by [`FunctionType.__call__`](https://github.com/python/typeshed/blob/main/stdlib/types.pyi#L114) and [`type.__call__`](https://github.com/python/typeshed/blob/main/stdlib/builtins.pyi#L214). We now consider both of these to be subtypes of `Callable[..., Any]` (because they have an equivalent signature), but we don't consider many particular types/functions to be subtypes of `Callable[..., Any]` (because they have a more specific signature that isn't a subtype of `Callable[..., Any]`). And of course any particular type/function must be a subtype of `type`/`FunctionType`.

We could maybe work around this problem specifically for `FunctionType.__call__` and `type.__call__`, but it seems like a more general problem. `FunctionType.__call__` and `type.__call__` are Liskov-valid, because Python uses assignability ("consistent subtyping") to decide if a subclass override is OK. Allowing overrides based on assignability, but then doing gradual structural subtyping according to the `S- <: T- && S+ <: T+` rule, unavoidably permits subtyping to not be transitive.

I'm not sure what can be done about this.

The stricter form of gradual subtyping that I originally implemented here (requiring that `S+ <: T-`) does not have this problem, because it ensures that any two gradual types that have a subtype relation cannot possibly materialize in any way that would violate that subtype relation. But of course the stricter form breaks reflexivity of subtyping.

Losing transitivity of subtyping is a serious problem for consistent simplification of unions and intersections, because it means the order in which types are added to a union/intersection can change the result of simplification.

---

_Comment by @carljm on 2025-06-23 14:47_

My plan here is to revert this PR back to the `S+ <: T-` rule that I originally implemented. I'm sad to give up reflexivity of subtyping (and the potential to implement gradual equivalence as bidirectional subtyping), but I think we can afford to give that up, and we can't afford to give up transitivity. And I don't see any way around the latter issue, barring a much deeper re-thinking of how subtyping applies to nominal types with gradual members in Python (essentially treating them as structural such that `class X(Y)` doesn't necessarily imply that `X <: Y`.)

---

_Comment by @JelleZijlstra on 2025-06-23 17:33_

Yes I think that unfortunately makes sense.

---

_Marked ready for review by @carljm on 2025-06-24 00:58_

---

_Review requested from @AlexWaygood by @carljm on 2025-06-24 00:58_

---

_Review requested from @sharkdp by @carljm on 2025-06-24 00:58_

---

_Review requested from @dcreager by @carljm on 2025-06-24 00:58_

---

_Review requested from @MichaReiser by @carljm on 2025-06-24 00:58_

---

_Label `great writeup` added by @sharkdp on 2025-06-24 06:18_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:18 on 2025-06-24 06:33_

Minor: Maybe we could add
```suggestion
static_assert(is_equivalent_to(Unknown, Unknown))
static_assert(is_equivalent_to(Any, Unknown))
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/is_gradual_equivalent_to.md`:1 on 2025-06-24 06:37_

I think it would be great if this file could be merged with `is_equivalent_to.md`, as a follow-up. Could probably also be a contributor task. Looks like there is quite a bit of duplication.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:326 on 2025-06-24 06:45_

It might be valuable to add a few positive cases in this section now? For example:

```py
static_assert(is_subtype_of(tuple[Any, ...], tuple[object, ...]))
static_assert(is_subtype_of(tuple[Never, ...], tuple[Any, ...]))
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:800 on 2025-06-24 06:52_

This sentence was a bit hard to understand, maybe add:
```suggestion
Instances of classes that inherit `Any` are not subtypes of some other `Arbitrary` class, because the
`Any` they inherit from could materialize to something (e.g. `object`) that is not a subclass of that class.
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:803 on 2025-06-24 06:56_

Maybe
```suggestion
Similarly, they are not subtypes of `Any`, because there are possible materializations of `Any` that
would not satisfy the subtype relation.
```

This is also something that is generally true (and could even be a property test) according to this new `S+ <: T-` subtyping rule: Nothing (except for `Never`) is a subtype of `T = Any`, because `T- = Never`.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:935 on 2025-06-24 07:10_

The doc comment above seems to imply that implicit `Unknown` "annotations" as in `(*args, **kwargs)` should also be considered equivalent to `...`? Should this be changed to the following? (similar for other checks above?)
```suggestion
            && value
                .iter()
                .any(|p| p.is_variadic() && p.annotated_type().is_none_or(|ty| ty.is_dynamic()))
            && value.iter().any(|p| {
                p.is_keyword_variadic() && p.annotated_type().is_none_or(|ty| ty.is_dynamic())
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/property_tests.rs`:238 on 2025-06-24 07:25_

Was this test deliberately removed? If so, why? It doesn't seem to be directly related to the changes here?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/builder.rs`:410 on 2025-06-24 07:31_

Did the previous behavior here cause problems (because other simplifications were not considered when simply swapping `bool` into the place of the pre-existing literal)? Or is the optimization just not needed anymore?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1193 on 2025-06-24 07:42_

At the call-site of `must_be_fully_static`, it looks like this is just an optimization/fast-path. But this `true` here seems to be required for correctness (some tests fail when I change it to `false`)?

If this is really just an optimization, should the behavior be fixed in `has_relation_to` instead? Or is this hard to fix... hence the TODO?

In any case, we could maybe document the problem in a test case:
```py
# TODO: `InheritsAny` is not a subtype of itself, this should not be an error
# error: [static-assert-error]
static_assert(not is_subtype_of(InheritsAny, InheritsAny))
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1169 on 2025-06-24 07:45_

If I understand the intention here correctly, should this maybe have a comment similar to "this function may have false negatives, but should not have false positives"?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1206 on 2025-06-24 07:45_

```suggestion
    /// `target`). In other words, for all possible pairs of materializations `self'` and
```

---

_@sharkdp approved on 2025-06-24 07:52_

This is great! Thank you for the detailed write-up and the doc comments. And for trying out the `S- <: T- && S+ <: T+` approach, even if it didn't work out. Very nice to see how this leads to a lot of simplification, too.

---

_Comment by @AlexWaygood on 2025-06-24 08:14_

I won't look in depth since @sharkdp already has, but lmk if there's anything specific you'd like my feedback on!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-24 08:15_

---

_@JelleZijlstra reviewed on 2025-06-24 12:25_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/property_tests.rs`:238 on 2025-06-24 12:25_

It uses `is_fully_static` which was removed. This property doesn't hold for gradual types; `~Any` is not disjoint from `Any`.

---

_@sharkdp reviewed on 2025-06-24 13:28_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/property_tests.rs`:238 on 2025-06-24 13:28_

> It uses `is_fully_static` which was removed.

Yes, but Carl introduced the option to quantify these property tests using `forall fully_static_types t. …`. We still test some properties that only hold true for fully static types.

---

_@carljm reviewed on 2025-06-24 17:01_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_gradual_equivalent_to.md`:1 on 2025-06-24 17:01_

I just went ahead and did this, it wasn't very hard.

---

_@carljm reviewed on 2025-06-24 17:10_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/property_tests.rs`:238 on 2025-06-24 17:10_

Oops, yeah, I meant to restore these tests after I added the fully-static quantification feature, but I missed this one.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:410 on 2025-06-24 17:17_

The previous behavior caused problems, because it assumed that `bool` has no subtypes other than `Literal[True]` and `Literal[False]`, but with this PR that is no longer the case: `bool & Any` is now a subtype of `bool`.

The tests requiring this change to pass are the "regression tests for complex nested simplifications" that I added at the end of this section: https://github.com/astral-sh/ruff/blob/cjm/nofullystatic/crates/ty_python_semantic/resources/mdtest/intersection_types.md#simplifications-of-bool-alwaystruthy-and-alwaysfalsy

(This was discovered by property tests.)

---

_@carljm reviewed on 2025-06-24 17:17_

---

_Converted to draft by @carljm on 2025-06-24 20:12_

---

_Comment by @codspeed-hq[bot] on 2025-06-24 21:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fnofullystatic?runnerMode=WallTime)

### Merging #18799 will **not alter performance**

<sub>Comparing <code>cjm/nofullystatic</code> (271062e) with <code>main</code> (66f50fb)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_@carljm reviewed on 2025-06-24 21:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1193 on 2025-06-24 21:57_

Thanks for pushing me to look at this more carefully! I renamed `must_be_fully_static` to `subtyping_is_reflexive`, to better reflect the invariant we actually need from it. It turns out that this `TODO` only applied to `GenericAlias` and `NominalInstance`, and not actually due to inheriting `Any` (that does make a type non-fully-static, but it still has reflexive subtyping) but due to the possibility that it is generic over `Any`, which means its subtyping is no longer reflexive. But in those two cases we already handled everything correctly later on, and this branch is unnecessary.

This branch was only necessary for correctness in the case of `ClassLiteral` -- and in the case of `ClassLiteral`, there is no `TODO` needed, it is correct to return `true` here. Subtyping is always reflexive for `ClassLiteral` types, whether they inherit `Any` or not.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1193 on 2025-06-24 21:59_

It is not quite true that this method is purely an optimization; we would need to add a new `ClassLiteral` vs `ClassLiteral` arm in `has_relation_to` for that to be true. But I'm not sure I see the value in adding that arm, when we can't observe its absence via any test. (And we do already have tests that would fail if we removed this `is_subtyping_reflexive` check and thus needed that arm.)

---

_@carljm reviewed on 2025-06-24 21:59_

---

_@AlexWaygood reviewed on 2025-06-24 21:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1197 on 2025-06-24 21:59_

I'd prefer to have an exhaustive match here, so that we don't forget to update this method if we add other variants in the future where subtyping is reflexive

---

_Marked ready for review by @carljm on 2025-06-25 00:59_

---

_Comment by @carljm on 2025-06-25 01:01_

Had to make some significant additional changes to UnionBuilder to fix issues found by property tests, but this now passes all tests and property tests. I'm going to go ahead and merge, but open to post-merge review comments!

---

_Merged by @carljm on 2025-06-25 01:02_

---

_Closed by @carljm on 2025-06-25 01:02_

---

_Branch deleted on 2025-06-25 01:02_

---
