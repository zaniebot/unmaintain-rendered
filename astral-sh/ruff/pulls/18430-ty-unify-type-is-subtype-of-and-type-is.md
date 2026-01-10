```yaml
number: 18430
title: "[ty] Unify `Type::is_subtype_of()` and `Type::is_assignable_to()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/assignability
created_at: 2025-06-02T14:50:55Z
updated_at: 2025-06-11T10:51:15Z
url: https://github.com/astral-sh/ruff/pull/18430
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Unify `Type::is_subtype_of()` and `Type::is_assignable_to()`

---

_Pull request opened by @AlexWaygood on 2025-06-02 14:50_

## Summary

There's lots of duplication between `Type::is_subtype_of()` and `Type::is_assignable_to()`, and we've had multiple bugs in the past because we've remembered to update `Type::is_subtype_of()` but have not realised that `Type::is_assignable_to()` also needed to be updated. This PR unifies the two methods: they are now both thin wrappers over a `Type::has_assignability_relation` method.

## Test Plan

- Existing tests
- New tests.
	- Some of these cover latent bugs that were accidentally fixed in this refactor
	- Some of them cover bugs that early versions of this PR introduced but were revealed by mypy_primer and have since been fixed.
- Property tests
- mypy_primer


---

_Label `ty` added by @AlexWaygood on 2025-06-02 14:51_

---

_Comment by @github-actions[bot] on 2025-06-02 14:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
aioredis (https://github.com/aio-libs/aioredis)
- error[invalid-parameter-default] aioredis/connection.py:1541:9: Default value of type `<class 'LifoQueue'>` is not assignable to annotated parameter type `type[Queue]`
- Found 26 diagnostics
+ Found 25 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ error[invalid-assignment] src/graphql/execution/execute.py:180:5: Object of type `staticmethod` is not assignable to `(Any, /) -> bool`
- Found 440 diagnostics
+ Found 441 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ error[invalid-assignment] pydantic/json_schema.py:1681:9: Object of type `Any | list[Any]` is not assignable to `ConfigDict`
- Found 761 diagnostics
+ Found 762 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-argument-type] src/hydra_zen/structured_configs/_implementations.py:2952:54: Argument to function `parse_strict_dataclass_options` is incorrect: Expected `Mapping[str, Any]`, found `DataclassOptions`
- error[invalid-argument-type] src/hydra_zen/structured_configs/_implementations.py:3229:21: Argument is incorrect: Expected `InitVar[ZenConvert | None]`, found `ZenConvert`
- error[invalid-argument-type] src/hydra_zen/structured_configs/_implementations.py:3241:21: Argument is incorrect: Expected `InitVar[ZenConvert | None]`, found `ZenConvert`
- error[invalid-argument-type] src/hydra_zen/structured_configs/_implementations.py:3311:13: Argument to function `parse_strict_dataclass_options` is incorrect: Expected `Mapping[str, Any]`, found `DataclassOptions`
- Found 613 diagnostics
+ Found 609 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-assignment] pwndbg/dbg/lldb/repl/readline.py:24:9: Object of type `(*args) -> Unknown` is not assignable to attribute `set_completion_display_matches_hook` on type `<module 'readline'> & ~<Protocol with members 'set_completion_display_matches_hook'>`
- Found 2301 diagnostics
+ Found 2300 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[invalid-argument-type] static_frame/test/property/test_archive_npy.py:36:13: Argument to function `get_frame` is incorrect: Expected `type[Index]`, found `<class 'IndexDate'>`
- error[invalid-argument-type] static_frame/test/property/test_archive_npy.py:38:13: Argument to function `get_frame` is incorrect: Expected `type[Index]`, found `<class 'IndexDate'>`
- error[invalid-argument-type] static_frame/test/property/test_index.py:22:25: Argument to function `property_values` is incorrect: Expected `type[Index]`, found `<class 'IndexGO'>`
- error[invalid-argument-type] static_frame/test/property/test_index.py:34:25: Argument to function `property_values` is incorrect: Expected `type[Index]`, found `<class 'IndexGO'>`
- error[invalid-argument-type] static_frame/test/property/test_index.py:46:38: Argument to function `property_loc_to_iloc_element` is incorrect: Expected `type[Index]`, found `<class 'IndexGO'>`
- error[invalid-argument-type] static_frame/test/property/test_index.py:63:36: Argument to function `property_loc_to_iloc_slice` is incorrect: Expected `type[Index]`, found `<class 'IndexGO'>`
- Found 1931 diagnostics
+ Found 1925 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-argument-type] testing/typing_checks.py:48:25: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["x"], Literal[2]]`, found `Foo`
- error[invalid-argument-type] testing/typing_checks.py:49:25: Argument to bound method `delitem` is incorrect: Expected `Mapping[Literal["y"], Unknown]`, found `Foo`
- Found 778 diagnostics
+ Found 776 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- error[invalid-argument-type] discord/message.py:2436:40: Argument to bound method `from_dict` is incorrect: Expected `Mapping[str, Any]`, found `Embed`
+ warning[unused-ignore-comment] discord/state.py:1151:70: Unused blanket `type: ignore` directive

meson (https://github.com/mesonbuild/meson)
- error[invalid-argument-type] mesonbuild/interpreter/interpreter.py:3398:50: Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["dependencies"], Unknown]`, found `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- error[invalid-argument-type] mesonbuild/modules/gnome.py:2223:56: Argument to bound method `append_holder_map` is incorrect: Expected `type[ObjectHolder]`, found `<class 'CustomTargetHolder'>`
- error[invalid-argument-type] mesonbuild/modules/gnome.py:2224:62: Argument to bound method `append_holder_map` is incorrect: Expected `type[ObjectHolder]`, found `<class 'CustomTargetHolder'>`
- error[invalid-argument-type] mesonbuild/modules/gnome.py:2225:50: Argument to bound method `append_holder_map` is incorrect: Expected `type[ObjectHolder]`, found `<class 'CustomTargetHolder'>`
- error[invalid-argument-type] mesonbuild/modules/gnome.py:2226:54: Argument to bound method `append_holder_map` is incorrect: Expected `type[ObjectHolder]`, found `<class 'CustomTargetHolder'>`
- error[invalid-argument-type] mesonbuild/modules/gnome.py:2227:51: Argument to bound method `append_holder_map` is incorrect: Expected `type[ObjectHolder]`, found `<class 'CustomTargetHolder'>`
- error[invalid-argument-type] mesonbuild/modules/hotdoc.py:485:53: Argument to bound method `append_holder_map` is incorrect: Expected `type[ObjectHolder]`, found `<class 'HotdocTargetHolder'>`
- error[invalid-argument-type] mesonbuild/modules/python.py:160:45: Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["dependencies"], Unknown]`, found `ExtensionModuleKw`
- error[invalid-argument-type] mesonbuild/modules/python.py:180:51: Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["c_args"], Unknown]`, found `ExtensionModuleKw`
- error[invalid-argument-type] mesonbuild/modules/python.py:184:53: Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["cpp_args"], Unknown]`, found `ExtensionModuleKw`
- error[invalid-argument-type] mesonbuild/modules/python.py:209:58: Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["link_args"], Unknown]`, found `ExtensionModuleKw`
- error[invalid-argument-type] mesonbuild/modules/python.py:558:62: Argument to bound method `append_holder_map` is incorrect: Expected `type[ObjectHolder]`, found `<class 'PythonInstallation'>`
- Found 1322 diagnostics
+ Found 1310 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- error[invalid-argument-type] openlibrary/core/lists/model.py:465:27: Argument to bound method `__init__` is incorrect: Expected `Thing | str | AnnotatedSeed`, found `str | ThingReferenceDict | AnnotatedSeedDict`
- Found 726 diagnostics
+ Found 725 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- error[invalid-raise] aiohttp/connector.py:1164:19: Cannot raise object of type `ClientConnectorCertificateError` (must be a `BaseException` subclass or instance)
- error[invalid-raise] aiohttp/connector.py:1166:19: Cannot raise object of type `ClientConnectorSSLError` (must be a `BaseException` subclass or instance)
- error[invalid-raise] aiohttp/connector.py:1273:19: Cannot raise object of type `ClientConnectorCertificateError` (must be a `BaseException` subclass or instance)
- error[invalid-raise] aiohttp/connector.py:1275:19: Cannot raise object of type `ClientConnectorSSLError` (must be a `BaseException` subclass or instance)
- Found 178 diagnostics
+ Found 174 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ warning[unused-ignore-comment] src/prefect/utilities/callables.py:321:67: Unused blanket `type: ignore` directive
- Found 4482 diagnostics
+ Found 4483 diagnostics

zulip (https://github.com/zulip/zulip)
- error[invalid-assignment] corporate/lib/remote_billing_util.py:155:5: Object of type `RemoteBillingIdentityDict | LegacyServerIdentityDict | None` is not assignable to `LegacyServerIdentityDict | None`
- error[invalid-argument-type] corporate/views/upgrade.py:222:66: Argument to function `render` is incorrect: Expected `Mapping[str, Any] | None`, found `UpgradePageContext | None`
- error[invalid-argument-type] corporate/views/upgrade.py:257:66: Argument to function `render` is incorrect: Expected `Mapping[str, Any] | None`, found `UpgradePageContext | None`
- error[invalid-argument-type] corporate/views/upgrade.py:292:66: Argument to function `render` is incorrect: Expected `Mapping[str, Any] | None`, found `UpgradePageContext | None`
- error[invalid-argument-type] zerver/actions/message_delete.py:94:33: Argument to function `send_event_on_commit` is incorrect: Expected `Mapping[str, Any]`, found `DeleteMessagesEvent`
- error[invalid-argument-type] zerver/actions/message_edit.py:864:33: Argument to function `send_event_on_commit` is incorrect: Expected `Mapping[str, Any]`, found `DeleteMessagesEvent`
- error[invalid-argument-type] zerver/views/users.py:888:34: Argument to function `json_success` is incorrect: Expected `Mapping[str, Any]`, found `APIUserDict`
- Found 6933 diagnostics
+ Found 6926 diagnostics

```
</details>


---

_Closed by @AlexWaygood on 2025-06-03 13:18_

---

_Reopened by @AlexWaygood on 2025-06-03 13:18_

---

_Comment by @AlexWaygood on 2025-06-03 18:05_

> ```diff
> + error[invalid-assignment] src/graphql/execution/execute.py:180:5: Object of type `staticmethod` is not assignable to `(Any, /) -> bool`
> ```

I'm not totally sure why this diagnostic is being added by this PR. Note that `staticmethod.__call__` was added in Python 3.10; I'm not sure what Python version we're inferring for this project in CI. I do know that we don't really understand the stub for `builtins.staticmethod` at all right now, because of the fact that it's generic over a legacy `ParamSpec`, which seems to do quite odd things to our inference (we don't even think that's valid right now).

> ```diff
> - error[invalid-assignment] pwndbg/dbg/lldb/repl/readline.py:24:9: Object of type `(*args) -> Unknown` is not assignable to attribute `set_completion_display_matches_hook` on type `<module 'readline'> & ~<Protocol with members 'set_completion_display_matches_hook'>`
> ```

This error goes away because we now infer the type of the module `readline` in this branch as being `Never`, since we know that the module `readline` always has a `set_completion_display_matches_hook` attribute:

```py
import readline

if not hasattr(readline, 'set_completion_display_matches_hook'):
    reveal_type(readline)
```

This seems like we're doing a better job now according to the information we've been given from typeshed's stubs.

> ```diff
> + warning[possibly-unbound-attribute] ddtrace/propagation/_database_monitoring.py:74:51: Attribute `tracer` on type `<module 'ddtrace'>` is possibly unbound
> + warning[possibly-unbound-attribute] ddtrace/propagation/_database_monitoring.py:76:17: Attribute `tracer` on type `<module 'ddtrace'>` is possibly unbound
> ```

This is a pre-existing bug: https://github.com/astral-sh/ty/issues/578. The reason it's showing up now is that we didn't previously ever consider module-literal types to be subtypes of protocol-instance types, but now we do.

---

_Marked ready for review by @AlexWaygood on 2025-06-03 18:07_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-03 18:07_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-03 18:07_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-03 18:07_

---

_Comment by @AlexWaygood on 2025-06-03 18:07_

(I don't love the name `Type::has_assignability_relation`. Better suggestions are welcome!)

---

_Comment by @jelle-openai on 2025-06-03 18:37_

Did you steal this from https://github.com/JelleZijlstra/pycroscope/blob/385397ac151199638c65d298997fd8298eb1abb2/pycroscope/relations.py#L154

---

_Comment by @AlexWaygood on 2025-06-03 18:40_

great minds think alike 😜

in all seriousness, though -- no, I hadn't seen that before writing this!

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/intersection_types.md`:318 on 2025-06-03 19:00_

`S` doesn't seem to be used?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1512 on 2025-06-03 19:08_

very nit: I feel like `has_assignability_relation` should appear after `is_assignable_to`. That mimics the pattern in a couple of other places, where the two methods that use the helper are defined before the helper.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1073 on 2025-06-03 19:11_

nit: Could this move into an `AssignabilityPolicy::is_equivalent_to` method? That would be nicely symmetric with the `is_satisfied_by` check above

```suggestion
        if policy.is_equivalent_to(db, self, target) {
            return true;
        }
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1194 on 2025-06-03 19:12_

Should `FunctionType` (and `Signature`) get the new refactored methods too?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1253 on 2025-06-03 19:13_

ditto for `ProtocolInstanceType`

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1307 on 2025-06-03 19:13_

ditto `CallableType`

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1352 on 2025-06-03 19:14_

Is "gradual equivalent" correct here, or should this call equivalent/gradual-equivalent based on the policy? (If so, and if you factor that our into an `AssignabilityPolicy` per above then you won't have to copy/paste the match here)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1347 on 2025-06-03 19:15_

I don't disagree, but is this just so that you don't have to perform a similar refactoring on `ClassType`?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1403 on 2025-06-03 19:15_

Should this be `has_assignability_relation_to`?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1493 on 2025-06-03 19:16_

ditto `NominalInstanceType`

---

_@dcreager reviewed on 2025-06-03 19:17_

I love this overall! My main questions are about whether we should go all the way and add `has_assignability_relation_to` to all of the subsidiary `Type` types

---

_Comment by @AlexWaygood on 2025-06-03 19:29_

> I love this overall! My main questions are about whether we should go all the way and add `has_assignability_relation_to` to all of the subsidiary `Type` types

I think we should! I deliberately held off from that here though, as it was already a large change that was tricky to get right, and I didn't want to make it harder to review. I _can_ make that change as part of this PR, but I feel like I'd prefer to do it as a followup?

---

_Comment by @dcreager on 2025-06-03 20:00_

> I think we should! I deliberately held off from that here though, as it was already a large change that was tricky to get right, and I didn't want to make it harder to review. I _can_ make that change as part of this PR, but I feel like I'd prefer to do it as a followup?

I have a slight preference for doing it all in one PR, though I'm happy to defer to you as the person doing the work! (It's a bit easier to turn off my brain and look for ~every call site to become `has_assignability_relation`, instead of having to decide for each hunk what the right change should be.)

Also note that `Signature` is already refactored like this (though it takes in a callback closure not the new enum), so it might not be as much extra effort.

But again, happy to defer to you!

> (I don't love the name `Type::has_assignability_relation`. Better suggestions are welcome!)

It's called `is_assignable_to_impl` in `Signature`:

https://github.com/astral-sh/ruff/blob/0079cc6817d070ff42bfc568c6652e260485983b/crates/ty_python_semantic/src/types/signatures.rs#L127




---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/intersection_types.md`:333 on 2025-06-03 20:23_

I think the right answer here depends on variance.

We might infer the current definition of `R` as bivariant in `T` (since `T` is unused in the body of `R`), and given that, `R[P]` and `R[Q]` are equivalent, so this should perhaps simplify to `Never`? But it would probably be fine if we didn't consider that, since bivariance is not common.

If `R` is invariant in `T`, then `R[P]` and `R[Q]` are disjoint and this should simplify to just `R[P]`.

If `R` is covariant or contravariant in `T`, then I think the unsimplified answer you have here is correct; `R[P]` and `R[Q]` are not disjoint.

(Not suggesting any of this needs to be done in this PR, but I think we could either add a comment here, or make these tests use explicit variance legacy typevars instead; if the test used an explicitly covariant typevar, then this wouldn't require comment.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/intersection_types.md`:363 on 2025-06-03 20:27_

Similar to above, this depends on variance. If bivariant (as the current definition of `R` would be) this is `object`; if invariant it should simplify to `~R[Q]`, if covariant or contravariant it should stay unsimplified.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1051 on 2025-06-03 20:37_

Naming nit: the current naming approach in this PR (both in the name `has_assignability_relation_to` and the name `AssignabilityPolicy::OnlyFullyStatic`) is to mostly stop using the term "subtype", and instead treat subtyping as "assignability, but for fully static types."

I don't prefer that naming, both because it strays further from the typing spec (in type theory terms, subtyping is the more fundamental relation), and because we don't use subtyping when actually assigning something to something else (so it is never used directly to determine "assignability" in any context).

How would you feel about naming the enum `TypeRelation` (with variants `TypeRelation::Subtype` and `TypeRelation::Assignable`), and the method `has_relation_to`? I realize the method and the enum wouldn't (for now at least) cover _all_ relations, but I don't think that's a strong argument against the naming?

---

_@carljm reviewed on 2025-06-03 20:49_

This is awesome!

Not taking the time to do a full detailed review, since @dcreager is on it; just a few comments on the tests, and on naming.

---

_@AlexWaygood reviewed on 2025-06-03 21:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/intersection_types.md`:333 on 2025-06-03 21:18_

Thanks, great point — I'll switch to using legacy typevars for now!

---

_@AlexWaygood reviewed on 2025-06-03 21:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1051 on 2025-06-03 21:19_

That all SGTM, I'm happy to make that change!

---

_@AlexWaygood reviewed on 2025-06-04 12:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1347 on 2025-06-04 12:43_

Hmm, I don't think so -- we're comparing between two different variants here rather than between two instances of the same variant

---

_Comment by @AlexWaygood on 2025-06-04 13:26_

Okay, I think that's all the review comments addressed! There are still one or two `Type::is_subtype_of` and `Type::is_assignable_to` methods on this branch that are not thin wrappers around `Type::has_relation_to` methods. But I'm unwilling to make further refactors as part of _this PR_, since some of these methods are in areas of ty's codebase that I'm not an expert in (e.g. signature subtyping).

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-04 13:26_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-04 13:26_

---

_Comment by @dhruvmanila on 2025-06-05 06:25_

> But I'm unwilling to make further refactors as part of _this PR_, since some of these methods are in areas of ty's codebase that I'm not an expert in (e.g. signature subtyping).

I'm happy to take this up as a follow-up unless someone else is interested in this :)

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:6755 on 2025-06-05 06:30_

nit: this isn't a large method so feel free to ignore but I was wondering whether we should enforce `type_1: Type<'db>` instead of the `Into` trait and make the changes at the call side `.applies_to(db, self.into(), other.into())`. This is mainly to avoid monomorphization of this method to the different combinations of types used with this method. 

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:6768 on 2025-06-05 06:31_

nit: `are_equivalent` since the parameter types are `Type`?

---

_@dhruvmanila reviewed on 2025-06-05 06:33_

I haven't looked too closely as there are already 2 reviewers but a couple of things I noticed while glancing through the changes.

---

_Comment by @AlexWaygood on 2025-06-05 10:48_

> ```diff
> + warning[possibly-unbound-attribute] ddtrace/propagation/_database_monitoring.py:74:51: Attribute `tracer` on type `<module 'ddtrace'>` is possibly unbound
> + warning[possibly-unbound-attribute] ddtrace/propagation/_database_monitoring.py:76:17: Attribute `tracer` on type `<module 'ddtrace'>` is possibly unbound
> ```
> 
> This is a pre-existing bug: astral-sh/ty#578. The reason it's showing up now is that we didn't previously ever consider module-literal types to be subtypes of protocol-instance types, but now we do.

Confirmed that this has now gone from the mypy_primer report following https://github.com/astral-sh/ruff/pull/18466, which fixed astral-sh/ty#578!


---

_@AlexWaygood reviewed on 2025-06-05 16:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1347 on 2025-06-05 16:13_

I think I now see what you mean -- does https://github.com/astral-sh/ruff/pull/18430/commits/0385c7761e0f7c0050b68e9d0d46c1649af8d668 make the changes you were looking for here?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:226 on 2025-06-06 16:21_

Should we add some counter-examples here as well? E.g. `Foo` specializations not being assignable to `Bar` specializations, and `Bar[int]` not being assignable to `Foo[bool]`?

I'm always wary of tests that would pass if everything were just `Unknown` 😆 

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1217 on 2025-06-06 16:37_

This looks wrong -- shouldn't we be looking up `__call__` only on the class, not on the instance?

It looks like this pre-exists this PR, and we probably shouldn't add more changes to this PR, but maybe we should add a TODO here, while we're here?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1236 on 2025-06-06 16:38_

This comment looks out of place here, because the logic implementing this comment is inside `satisfies_protocol`, not here.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1392 on 2025-06-06 16:41_

It doesn't seem right that this pattern is symmetrical, but the `has_relation_to` check is not symmetrical.

---

_@carljm approved on 2025-06-06 16:43_

This is great!

---

_@AlexWaygood reviewed on 2025-06-06 16:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1392 on 2025-06-06 16:50_

I think this is correct. This branch upholds two properties:
- For any type `T` that is assignable to `type`, `T` shall be assignable to `type[Any]`.
- For any type `T` that is assignable to `type`, `type[Any]` shall be assignable to `T`.

This is really the same as the very first branch in this `match` statement that handles dynamic types. That branch upholds two properties:
- For any type `S` that is assignable to `object` (which is _all_ types), `S` shall be assignable to `Any`
- For any type `S` that is assignable to `object` (which is _all_ types), `Any` shall be assignable to `S`.

The only difference between this branch and the first branch is that the first branch deals with the type `object & Any` (which simplifies to `Any`!) whereas this branch deals with the type `type & Any`

---

_@AlexWaygood reviewed on 2025-06-06 17:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:226 on 2025-06-06 17:08_

Ugh, good callout. It turns out all `type[SomeSubscript[]]` type expressions are still `@Todo` types for us :-( So most of these are not testing the bug I wanted them to test. Which may not even be testable right now if we currently only ever synthesize these types, and don't yet support reading them from type expressions.

Still, these assertions seem useful to prevent regressions. I'll add the failing counter-examples with TODO comments.

---

_@AlexWaygood reviewed on 2025-06-06 17:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:226 on 2025-06-06 17:10_

hmm, maybe I can test this in a slightly more roundabout way, come to think of it (edit: no, that didn't work either)

---

_@AlexWaygood reviewed on 2025-06-06 17:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1236 on 2025-06-06 17:20_

no? The logic implementing this comment is the

```rs
            (Type::ProtocolInstance(_), _) => false,
```

branch immediately below this comment. The "protocol types can be subtypes of `object`" special case is handled in this branch higher up:

```rs
            (_, Type::NominalInstance(instance)) if instance.class.is_object(db) => true,
```

but the branch immediately below this comment implements the "and it cannot be a subtype of any other nominal type" bit

---

_@AlexWaygood reviewed on 2025-06-06 17:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1217 on 2025-06-06 17:24_

this is okay because of the fact that we're passing the `NO_INSTANCE_FALLBACK` policy:

https://github.com/astral-sh/ruff/blob/1274521f9fda19507921c85442d0ca991b67b846/crates/ty_python_semantic/src/types.rs#L192-L200

---

_@AlexWaygood reviewed on 2025-06-06 17:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1392 on 2025-06-06 17:26_

I added this explanation as a comment in https://github.com/astral-sh/ruff/pull/18430/commits/40e68d09ef8b9a88fbdb173f34dd387360f07c83

---

_Merged by @AlexWaygood on 2025-06-06 17:28_

---

_Closed by @AlexWaygood on 2025-06-06 17:28_

---

_Branch deleted on 2025-06-06 17:28_

---

_@carljm reviewed on 2025-06-07 01:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1217 on 2025-06-07 01:56_

Ah right, I forgot the details of how this works

---

_Label `internal` added by @dhruvmanila on 2025-06-11 10:51_

---
