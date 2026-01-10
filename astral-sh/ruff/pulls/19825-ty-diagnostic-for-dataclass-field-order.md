```yaml
number: 19825
title: "[ty] diagnostic for dataclass field order"
type: pull_request
state: open
author: PrettyWood
labels:
  - ty
assignees: []
base: main
head: dataclass-field-order
created_at: 2025-08-08T11:46:51Z
updated_at: 2025-08-21T17:46:17Z
url: https://github.com/astral-sh/ruff/pull/19825
synced_at: 2026-01-10T17:46:21Z
```

# [ty] diagnostic for dataclass field order

---

_Pull request opened by @PrettyWood on 2025-08-08 11:46_

## Summary
Emit diagnostic when defining a field without a default after a field with a default, see [existing TODO](https://github.com/astral-sh/ruff/blob/dbc137c9516808baa292da7a928e50ba36a14d39/crates/red_knot_python_semantic/resources/mdtest/dataclasses.md?plain=1#L120)

<img width="875" height="450" alt="Screenshot 2025-08-08 at 13 46 29" src="https://github.com/user-attachments/assets/60c353f6-4f7a-41ab-bfb1-b4fd230fadf5" />

Part of https://github.com/astral-sh/ty/issues/111

## Test Plan
Updated Markdown tests

---

_Review requested from @carljm by @PrettyWood on 2025-08-08 11:46_

---

_Review requested from @AlexWaygood by @PrettyWood on 2025-08-08 11:46_

---

_Review requested from @sharkdp by @PrettyWood on 2025-08-08 11:46_

---

_Review requested from @dcreager by @PrettyWood on 2025-08-08 11:46_

---

_Label `ty` added by @AlexWaygood on 2025-08-08 11:49_

---

_Comment by @github-actions[bot] on 2025-08-08 11:51_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-21 13:57:29.996336756 +0000
+++ new-output.txt	2025-08-21 13:57:32.501345368 +0000
@@ -214,9 +214,6 @@
 dataclasses_kwonly.py:23:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_kwonly.py:38:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_kwonly.py:53:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
-dataclasses_kwonly.py:61:1: error[missing-argument] No argument provided for required parameter `c`
-dataclasses_kwonly.py:61:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
-dataclasses_kwonly.py:61:14: error[parameter-already-assigned] Multiple values provided for parameter `b`
 dataclasses_order.py:50:4: error[unsupported-operator] Operator `<` is not supported for types `DC1` and `DC2`
 dataclasses_postinit.py:28:7: error[unresolved-attribute] Type `DC1` has no attribute `x`
 dataclasses_postinit.py:29:7: error[unresolved-attribute] Type `DC1` has no attribute `y`
@@ -270,11 +267,17 @@
 dataclasses_transform_func.py:77:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@create_model_frozen`
 dataclasses_transform_func.py:97:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
 dataclasses_transform_meta.py:60:36: error[unknown-argument] Argument `other_name` does not match any known parameter
+dataclasses_transform_meta.py:66:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_meta.py:73:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
 dataclasses_transform_meta.py:79:6: error[unsupported-operator] Operator `<` is not supported for types `Customer2` and `Customer2`
+dataclasses_transform_meta.py:83:8: error[missing-argument] No argument provided for required parameter `id`
+dataclasses_transform_meta.py:83:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_usage.py:50:6: error[missing-argument] No argument provided for required parameter `unit_price`
 dataclasses_usage.py:51:28: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["price"]`
 dataclasses_usage.py:52:36: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
+dataclasses_usage.py:61:5: error[dataclass-field-order] Required field `b` cannot be defined after fields with default values
+dataclasses_usage.py:67:5: error[dataclass-field-order] Required field `b` cannot be defined after fields with default values
+dataclasses_usage.py:73:5: error[dataclass-field-order] Required field `b` cannot be defined after fields with default values
 dataclasses_usage.py:83:13: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_usage.py:88:5: error[invalid-assignment] Object of type `dataclasses.Field[Literal[""]]` is not assignable to `int`
 dataclasses_usage.py:127:8: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
@@ -850,5 +853,5 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 851 diagnostics
+Found 854 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-08 11:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ tests/test_annotations.py:91:13: error[dataclass-field-order] Required field `y` cannot be defined after fields with default values
+ tests/test_hooks.py:68:13: error[dataclass-field-order] Required field `y` cannot be defined after fields with default values
- Found 556 diagnostics
+ Found 558 diagnostics

svcs (https://github.com/hynek/svcs)
+ src/svcs/_core.py:73:5: error[dataclass-field-order] Required field `takes_container` cannot be defined after fields with default values
+ src/svcs/_core.py:74:5: error[dataclass-field-order] Required field `enter` cannot be defined after fields with default values
- Found 102 diagnostics
+ Found 104 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_run.py:1497:5: error[dataclass-field-order] Required field `_ki_protected` cannot be defined after fields with default values
+ src/trio/_dtls.py:164:5: error[dataclass-field-order] Required field `epoch_seqno` cannot be defined after fields with default values
+ src/trio/_dtls.py:328:5: error[dataclass-field-order] Required field `msg_type` cannot be defined after fields with default values
+ src/trio/_dtls.py:329:5: error[dataclass-field-order] Required field `msg_seq` cannot be defined after fields with default values
+ src/trio/_dtls.py:338:5: error[dataclass-field-order] Required field `content_type` cannot be defined after fields with default values
+ src/trio/_tests/test_highlevel_open_tcp_listeners.py:145:5: error[dataclass-field-order] Required field `_proto` cannot be defined after fields with default values
- Found 656 diagnostics
+ Found 662 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- tests/debugging/utils.py:134:12: error[missing-argument] No arguments provided for required parameters `rate`, `condition`, `template`, `segments`, `take_snapshot`, `probe_id`, `version`
+ tests/debugging/utils.py:134:12: error[missing-argument] No arguments provided for required parameters `rate`, `condition`, `condition_error_rate`, `template`, `segments`, `take_snapshot`, `limits`, `source_file`, `line`, `probe_id`, `version`, `tags`
- tests/debugging/utils.py:142:12: error[missing-argument] No arguments provided for required parameters `rate`, `condition`, `template`, `segments`, `take_snapshot`, `evaluate_at`, `probe_id`, `version`
+ tests/debugging/utils.py:142:12: error[missing-argument] No arguments provided for required parameters `rate`, `condition`, `condition_error_rate`, `template`, `segments`, `take_snapshot`, `limits`, `evaluate_at`, `module`, `func_qname`, `probe_id`, `version`, `tags`
- tests/debugging/utils.py:149:12: error[missing-argument] No arguments provided for required parameters `rate`, `condition`, `template`, `segments`, `take_snapshot`, `probe_id`, `version`
+ tests/debugging/utils.py:149:12: error[missing-argument] No arguments provided for required parameters `rate`, `condition`, `condition_error_rate`, `template`, `segments`, `take_snapshot`, `limits`, `source_file`, `line`, `probe_id`, `version`, `tags`
- tests/debugging/utils.py:157:12: error[missing-argument] No arguments provided for required parameters `rate`, `condition`, `template`, `segments`, `take_snapshot`, `evaluate_at`, `probe_id`, `version`
+ tests/debugging/utils.py:157:12: error[missing-argument] No arguments provided for required parameters `rate`, `condition`, `condition_error_rate`, `template`, `segments`, `take_snapshot`, `limits`, `evaluate_at`, `module`, `func_qname`, `probe_id`, `version`, `tags`
- tests/debugging/utils.py:164:12: error[missing-argument] No arguments provided for required parameters `condition`, `kind`, `name`, `value`, `probe_id`, `version`
+ tests/debugging/utils.py:164:12: error[missing-argument] No arguments provided for required parameters `condition`, `condition_error_rate`, `kind`, `name`, `value`, `source_file`, `line`, `probe_id`, `version`, `tags`
- tests/debugging/utils.py:172:12: error[missing-argument] No arguments provided for required parameters `condition`, `kind`, `name`, `value`, `evaluate_at`, `probe_id`, `version`
+ tests/debugging/utils.py:172:12: error[missing-argument] No arguments provided for required parameters `condition`, `condition_error_rate`, `kind`, `name`, `value`, `evaluate_at`, `module`, `func_qname`, `probe_id`, `version`, `tags`
- tests/debugging/utils.py:179:12: error[missing-argument] No arguments provided for required parameters `condition`, `probe_id`, `version`
+ tests/debugging/utils.py:179:12: error[missing-argument] No arguments provided for required parameters `condition`, `condition_error_rate`, `module`, `func_qname`, `probe_id`, `version`, `tags`
- tests/debugging/utils.py:185:12: error[missing-argument] No arguments provided for required parameters `target_span`, `decorations`, `probe_id`, `version`
+ tests/debugging/utils.py:185:12: error[missing-argument] No arguments provided for required parameters `target_span`, `decorations`, `source_file`, `line`, `probe_id`, `version`, `tags`
- tests/debugging/utils.py:192:12: error[missing-argument] No arguments provided for required parameters `target_span`, `decorations`, `evaluate_at`, `probe_id`, `version`
+ tests/debugging/utils.py:192:12: error[missing-argument] No arguments provided for required parameters `target_span`, `decorations`, `evaluate_at`, `module`, `func_qname`, `probe_id`, `version`, `tags`
- tests/debugging/utils.py:199:12: error[missing-argument] No arguments provided for required parameters `rate`, `condition`, `session_id`, `level`, `probe_id`, `version`
+ tests/debugging/utils.py:199:12: error[missing-argument] No arguments provided for required parameters `rate`, `condition`, `condition_error_rate`, `session_id`, `level`, `module`, `func_qname`, `probe_id`, `version`, `tags`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-docker/prefect_docker/worker.py:950:35: error[missing-argument] No argument provided for required parameter `root`
- src/integrations/prefect-docker/tests/test_worker.py:1187:9: error[missing-argument] No argument provided for required parameter `root`
- src/integrations/prefect-kubernetes/tests/test_observer.py:78:42: error[missing-argument] No argument provided for required parameter `root`
- src/integrations/prefect-kubernetes/tests/test_observer.py:155:42: error[missing-argument] No argument provided for required parameter `root`
- src/integrations/prefect-redis/tests/test_messaging.py:452:30: error[missing-argument] No argument provided for required parameter `root`
- src/integrations/prefect-redis/tests/test_ordering.py:23:12: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/client/schemas/responses.py:469:16: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/events/related.py:36:9: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/events/related.py:51:12: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/runner/runner.py:1048:17: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/runner/runner.py:1064:26: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/runner/runner.py:1097:17: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/runner/runner.py:1106:13: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/runner/runner.py:1122:26: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/actions.py:142:20: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/actions.py:200:20: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/actions.py:216:30: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/actions.py:232:30: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/schemas/automations.py:432:25: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/schemas/automations.py:448:21: error[missing-argument] No argument provided for required parameter `root`
- Found 2951 diagnostics
+ Found 2931 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Review requested from @MichaReiser by @PrettyWood on 2025-08-08 12:15_

---

_@carljm approved on 2025-08-14 01:00_

This is great, thank you! I rebased and pushed a couple additional tests, plus reworked the AST-node-finding so it uses our existing indexing of assignments and can handle more complex cases (e.g. a version-conditional field, as shown in the added test).

---

_Comment by @carljm on 2025-08-14 01:08_

Looking at the conformance suite results, I think we need to take `kw_only` into account here (keyword-only fields don't participate in this check, since they go to the end of the `__init__` signature and are not positional). I think also `init=False` fields should be ignored?

So I think we should land https://github.com/astral-sh/ruff/pull/19677 and then take those into account.

---

_Comment by @PrettyWood on 2025-08-14 06:44_

Great thank you so much for the review + improvements @carljm! I’m still getting familiar with the codebase, so your feedback is really helpful.
You’re right about `kw_only` and `init=False` fields. I reckon we should merge this first and I'll come back to this PR afterward. Tell me if I can help

---

_Comment by @carljm on 2025-08-14 15:08_

We already supported `init` arg, and I just merged the PR for `kw_only` support, so I think we are ready to add consideration of those to this PR! If you are up for doing that, that would be great.

---

_Comment by @PrettyWood on 2025-08-14 16:27_

Sure I'll work on it this weekend

---

_Comment by @PrettyWood on 2025-08-17 17:53_

@carljm Should be good. Feel free to update directly the branch!

---

_Comment by @github-actions[bot] on 2025-08-18 08:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm requested changes on 2025-08-19 00:53_

Thanks! Looking at the conformance test suite, I see that this diff introduces a false positive at https://github.com/python/typing/blob/main/conformance/tests/dataclasses_kwonly.py#L58

The problem appears to be that when emitting this field-ordering diagnostic, we aren't respecting the dataclass-level `kw_only` parameter, only field-level `kw_only`.

Similarly, the many new diagnostics in `freqtrade` on this PR are also false positives. `freqtrade` uses Pydantic, and Pydantic uses `dataclass_transform` with `kw_only_default=True`: https://github.com/pydantic/pydantic/blob/main/pydantic/_internal/_model_construction.py#L78

So we need to add some test cases using `@dataclass(kw_only=True)` and `@dataclass_transform(kw_only_default=True)` and make sure we are respecting those as well, when considering which fields are kw-only.

I think there is also a somewhat-related existing bug with the handling of `KW_ONLY`, where its effects are wrongly considered to span from a base class to an inheriting class: https://github.com/astral-sh/ty/issues/1047 That existing bug doesn't necessarily need to be fixed in this PR, but we need to make sure not to recreate it in a new location, and I think the current version of this PR does.

I think maybe a common thread in all the above is that we need some better intermediate abstration layers; right now `own_fields` and `fields` give us "raw" information from each field's direct annotation, not considering any dataclass-level settings, and then we process that raw information separately in `own_synthesized_member` (to construct the `__init__` signature) and also in the dataclass-checking code. I suspect we want an intermediate API to return "processed fields" that already accounts for dataclass-level settings, too.

cc @thejchap and @sharkdp 

---

_Comment by @PrettyWood on 2025-08-19 07:57_

@carljm Ok I'll take a step back on this PR. Probably best to first look at https://github.com/astral-sh/ty/issues/1047 (I suggested a fix https://github.com/astral-sh/ruff/pull/19986) and wait for feedbacks from others.
Thank you very much for the very precise reviews.

---

_Comment by @carljm on 2025-08-19 18:55_

@PrettyWood Your fix for astral-sh/ty#1047 looked good, I merged it!

FWIW I don't think there's any need to take a step back here or wait for other input, especially with that fix merged. I think this PR is close, we just need to add some tests for the cases I mentioned above (where there is dataclass-level `kw_only` kwarg or dataclass-transform-level `kw_only_default` kwarg) and figure out how to handle them best. If you don't have time and would rather hand this off to someone else, that's of course fine, just let us know. But if you want to propose a refactor that allows us to handle those cases, I think that would be the next step here.

Really appreciate your contributions!

---

_Comment by @PrettyWood on 2025-08-19 19:27_

Perfect thank you! Happy to keep working on it for sure! I'll work on the follow-up tomorrow

---

_Comment by @PrettyWood on 2025-08-20 12:47_

@carljm I kept extending `own_fields` directly (since I already added some contextual logic with `_: KW_ONLY` that sets `field.kw_only = Some(true)`). I don’t think we need a separate “processed fields” yet (if ever) but if you’d prefer me to split it I'll do it

---

_@PrettyWood reviewed on 2025-08-20 13:20_

---

_Review comment by @PrettyWood on `crates/ty_python_semantic/src/types/infer.rs`:1424 on 2025-08-20 13:20_

This is to support 

```py
@dataclass(kw_only=True)
class KwOnlyClassWithPositionalField:
    x: int = 1
    y: str = field(kw_only=False, default="hello")
    # error: [dataclass-field-order] "Required field `z` cannot be defined after fields with default values"
    z: float = field(kw_only=False)
```

because when `dataclasses.field` is used, `default` is not `None` but `Some(Dynamic(Unknown))`

TBH I think it's a bit weird. I would expect in this example to have both `x` and `y`
with `default_ty = None`

```py
@dataclass
class A:
    x: int
    y: int = field(metadata={"random": "info"})
```

But it meant refactoring `FieldInstance` to have `default_type` being an `Option`
So in the meantime I think it's ok like this. Happy to go further if needed

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:568 on 2025-08-20 20:58_

Shouldn't this say the opposite? No fields (of a `kw_only=True` dataclass) participate in field ordering checks.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:1424 on 2025-08-20 21:02_

I do think we should fix this properly in this PR by making `default_ty` be an `Option`. It is possible for `default_ty` to be `Unknown` for other reasons, like if there was an unknown import for example. These cases shouldn't be treated as "no default".

I think this problem is currently causing false positives in the ecosystem report for this PR, for example [in trio](https://github.com/python-trio/trio/blob/main/src/trio/_channel.py#L152) where we consider the return type of `attrs.Factory` to be `Unknown` (due to limitations in our current constraint solver), and so we treat that wrongly as "no default value".

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:1424 on 2025-08-20 21:09_

Looking at the conformance suite results, the one remaining false positive we add in this PR is at https://github.com/python/typing/blob/main/conformance/tests/dataclasses_usage.py#L186

I think if we fix `default_ty` to be an `Option`, then we can easily fix this false positive also, by ensuring that we set it to `Some` if `default_factory` is specified. We could for now just make that `Some(Unknown)`, although I think adding proper support for `default_factory` would actually be quite easy -- we just need to take the argument type of `default_factory` and `Type::try_call` it with no arguments, and use the return type of that call. This could also be done as a separate PR, though I'd probably prefer that PR to land first in that case, so we avoid adding a new false positive with this diagnostic.

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/ty_walltime.rs`:145 on 2025-08-20 21:17_

I think we should revert this change, and if it causes problems that's a sign that means this PR isn't ready to land yet. Freqtrade does not have a large number of true positives for this diagnostic, so if we are causing a large number of new diagnostics, they are almost certainly false positives, and we don't want to land new diagnostics with large numbers of false positives on real ecosystem code.

The freqtrade false positives are weird, because I would have expected them to be fixed with this PR; they are false positives on lines like https://github.com/freqtrade/freqtrade/blob/develop/freqtrade/rpc/api_server/api_schemas.py#L19, because we see previous fields on [pydantic.BaseModel](https://github.com/pydantic/pydantic/blob/f7a9b73517afecf25bf898e3b5f591dffe669778/pydantic/main.py#L121), and for some reason we aren't respecting `kw_only_default` [on their dataclass-transform metaclass](https://github.com/pydantic/pydantic/blob/f7a9b73517afecf25bf898e3b5f591dffe669778/pydantic/_internal/_model_construction.py#L78).

Boiling this down to [a minimal example](https://play.ty.dev/15f3c09e-ad52-45d3-8a1c-1d7b1bf64d8f), it looks to me like our support for `kw_only_default` just currently doesn't work for metaclass-based dataclass-transforms, only for decorator-based ones.

Unfortunately I think this needs to be fixed (whether in this PR or a separate one that lands first) before we can land this PR; it just causes too many false positives in real projects otherwise.

---

_@carljm requested changes on 2025-08-20 21:42_

Thank you! The updates look great, and I agree we don't need a new abstraction layer (yet?) -- it's fine to just ensure that the fields returned from `own_fields` already have a fully correct `kw_only` value. My main concern is just that we be clear about the contract: do the Field objects returned from `own_fields` represent exactly what is explicitly specified in that Field definition, or do they represent our full understanding of the field, including what's inherited from dataclass arguments and dataclass-transform arguments? I think it's fine if it's the latter, so long as that's consistent.

Sorry for requesting changes again -- in many cases we are happy to land PRs with remaining TODOs, but adding new false positives in the ecosystem is something we are very reluctant to do, and at the moment this PR adds a lot of them. It turns out we need to improve our support for other dataclass features significantly in order to add this diagnostic without false positives.

---

_Review comment by @PrettyWood on `crates/ruff_benchmark/benches/ty_walltime.rs`:145 on 2025-08-20 22:08_

Yes sorry this should have been reverted.
And no problem happy to take the time to fully tackle everything. I'll look at it tomorrow 

---

_@PrettyWood reviewed on 2025-08-20 22:08_

---

_@PrettyWood reviewed on 2025-08-20 22:09_

---

_Review comment by @PrettyWood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:568 on 2025-08-20 22:09_

🙈 oops

---

_Comment by @PrettyWood on 2025-08-20 22:11_

> think it's fine if it's the latter, so long as that's consistent.

For me yes. Happy to add a docstring on `own_fields` to make it explicit

> I think if we fix default_ty to be an Option

Perfect glad we agree. I didn't want to put too much in a single PR but you're right, better that than having new false positives. I'll work on it tomorrow 



---

_Comment by @PrettyWood on 2025-08-21 09:34_

@carljm Better :) Commits should be self-explanatory.

---

_Comment by @carljm on 2025-08-21 15:06_

Thank you! The conformance suite results look great here now! The ecosystem results look mostly great :) The one case where it looks like we are still adding false positives is that [we don't support `field_specifiers` on `dataclass_transform`](https://github.com/astral-sh/ty/issues/1068), so we interpret `= attrs.field(...)` as a default value rather than as a field specifier, leading to the false positives we see on trio, svcs, and the attrs tests.

Ideally, we would support `field_specifiers` before we land this, so we avoid adding false positives. Trying to decide if we can go in the other direction and land this first (just to avoid collecting merge conflicts here in the meantime), but I think it would make it important that we address `field_specifiers` soon.



---

_Comment by @PrettyWood on 2025-08-21 15:47_

I'll fix the `field_specifiers` in a dedicated PR and we'll rebase/merge this one after

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:113 on 2025-08-21 15:52_

This test looks great. Nit: I think this section is to show basic usage, and arguments are tested below. I think we should move this down with the rest of the `kw_only_default` tests below, and just add an example with metaclass there. (I think the example here with decorator is probably redundant with the test we already have down there, and we don't need both?)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:260 on 2025-08-21 16:20_

The test immediately above here I thought should be fixed by logic you added in this PR, but then I realized why it isn't -- commented below.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2496 on 2025-08-21 17:44_

There's a test (I commented above) that I would expect this logic to fix, but it doesn't fix it. And I think I understand why. Currently `DataclassParams` is just a bitset, but this isn't really adequate when we have to compose it with `DataclassTransformerParams`. With the bitset representation, we can't distinguish "no `kw_only` argument" from `kw_only=False` -- they are both represented by the bit being off. I think we will need to change `DataclassParams` so that at least `kw_only` can be tri-valued. Because "no `kw_only` arg" should default to the dataclass-transform's `kw_only_default` value, but `kw_only=False` should potentially override it.

I think this doesn't need to be fixed in this PR, but it does impact the correctness of this code, so I think we should add a TODO comment here.
 
```suggestion
                        // TODO `kw_only=False` should override `kw_only_default=True`, this will require
                        // a tri-valued representation for `DataclassParams::KW_ONLY`
                        .is_some_and(|params| params.contains(DataclassParams::KW_ONLY))
                        || self.try_metaclass(db).is_ok_and(|(_, transformer_params)| {
                            transformer_params.is_some_and(|params| {
                                params.contains(DataclassTransformerParams::KW_ONLY_DEFAULT)
                            })
                        })
                    {
                        // `@dataclass(kw_only=True)` or `@dataclass_transform(kw_only_default=True)`
                        field.kw_only = Some(true);
```

---

_@carljm approved on 2025-08-21 17:46_

Code here looks great, just a few minor comments! I think once we resolve the `field_specifiers` false positives, this is ready to go.

---
