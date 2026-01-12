```yaml
number: 22046
title: "[`airflow`] Passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed (`AIR303`)"
type: pull_request
state: merged
author: sjyangkevin
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-AIR303-function-signature-change
created_at: 2025-12-18T02:53:19Z
updated_at: 2025-12-30T16:39:09Z
url: https://github.com/astral-sh/ruff/pull/22046
synced_at: 2026-01-12T15:57:39Z
```

# [`airflow`] Passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed (`AIR303`)

---

_@sjyangkevin_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This is a follow up PR to https://github.com/astral-sh/ruff/pull/21096

The new code AIR303 is added for checking function signature change in Airflow 3.0. The new rule added to AIR303 will check if positional argument is passed into `airflow.lineage.hook.HookLineageCollector.create_asset`. Since this method is updated to accept only keywords argument, passing positional argument into it is not allowed, and will raise an error. The test is done by checking whether positional argument with 0 index can be found.

## Test Plan

A new test file is added to the fixtures for the code AIR303. Snapshot test is updated accordingly.

<img width="1444" height="513" alt="Screenshot from 2025-12-17 20-54-48" src="https://github.com/user-attachments/assets/bc235195-e986-4743-9bf7-bba65805fb87" />

<img width="981" height="433" alt="Screenshot from 2025-12-17 21-34-29" src="https://github.com/user-attachments/assets/492db71f-58f2-40ba-ad2f-f74852fa5a6b" />

---

_Renamed from "[airflow] passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed (AIR303)" to "[`airflow`] passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed (`AIR303`)" by @sjyangkevin on 2025-12-18 02:53_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1275 on 2025-12-18 07:20_

We probably can make a parent function like `airflow_3_function_signature_change_expr` and call `airflow_3_keyword_args_only_function` inside. IIRC, there're come other cases as well

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:11 on 2025-12-18 07:21_

```suggestion
#[violation_metadata(preview_since = "0.15.0")]
```

This is wrong. but will need to confirm which version we want to add this preview

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:19 on 2025-12-18 07:22_

We could extend the description to include any kind of functional siguature change

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:63 on 2025-12-18 07:25_

I'm not sure whether we want to make this a replacement. Or should we just make it a `Message`? Or this probably should be something like `make pos arg passed through keyword`, but we'll need to store the order of that function as well. it's not super hard but not sure whether fixing that worth the effort

---

_@Lee-W reviewed on 2025-12-18 07:25_

---

_@ntBre reviewed on 2025-12-18 16:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:11 on 2025-12-18 16:39_

Good catch. I think 0.14.11 would be a good start, assuming this might land next week. We can always bump the version again just before landing, if needed.

---

_Label `rule` added by @ntBre on 2025-12-18 16:39_

---

_Label `preview` added by @ntBre on 2025-12-18 16:39_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 16:50_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@sjyangkevin reviewed on 2025-12-18 17:49_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1275 on 2025-12-18 17:49_

Agree. The keyword argument enforcement is a specific case in this category. It's better to have a generic name for it, and then we can include more cases for function signature chage.

---

_@sjyangkevin reviewed on 2025-12-18 17:50_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:11 on 2025-12-18 17:50_

Thanks for catching it and thank @ntBre for the feedback. I think we could set it to 0.14.11, and bump the version if needed.

---

_@sjyangkevin reviewed on 2025-12-18 17:54_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:63 on 2025-12-18 17:54_

Yeah, I also came across thinking about this, and attempted to use a simple message. However, I am not sure if we want to do something like we can show the expected change to the user. for example,

fn("a", "b") -> fn(a="a", b="b"),

not sure if it is the case easier and can be done through a replacement. I might need to take a deeper look into this to understand it better.

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:63 on 2025-12-19 02:54_

I think I get what you suggest after looking again into the code. Just want to see if my understanding is correct.

The current `Replacement` can do 5 things, such as replace name, update import, show message, etc. The added `AttrNameWithMessage` replace the name and show additional message.

> Or should we just make it a Message?

In this check, since we check for argument type change (pos -> keyword), instead of name change, so show a message might be more appropriate and straight-forward to implement, rather than using `Replacement::AttrNameWithMessage`.

> Or this probably should be something like make pos arg passed through keyword, but we'll need to store the order of that function as well.

If we want to use Replacement, then we need to check the position of the argument, and somehow map it to the corresponding keyword argument. Do a "FULL" replacement, for both name and argument update.

Let me know if my understanding is correct. I could try to look into the second option, as I feel it could offer a streamlined update. However, it could take into the complexity in checking the order of the arguments; but this kind of update can also be straight-forward done manually.


---

_@sjyangkevin reviewed on 2025-12-19 02:54_

---

_@Lee-W reviewed on 2025-12-19 02:59_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:63 on 2025-12-19 02:59_

Yes, your understanding is correct 👍 

---

_Comment by @sjyangkevin on 2025-12-19 06:31_

Hi @Lee-W , @ntBre , I've made the following updates to the PR. The updates might be more than what we've discussed, but please let me know if you have further feedback. Thanks for your time to review.

### Updates
* created a parent function `airflow_3_function_signature_change_expr` to call `airflow_3_keyword_args_only_function`
* updated the `violation_metadata` to 0.14.11
* updated the struct document to be more generic for function signature change
* updated some namings and description for functions
* removed the check for `create_dataset`, which is checked by the `AIR301` for deprecation, implement only function signature change check for `create_asset` in `AIR303` (crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs **L107**)
* fixed the document generation issue, CI error for document generation should be fixed

> I'm not sure whether we want to make this a replacement. Or should we just make it a `Message`? Or this probably should be something like `make pos arg passed through keyword`, but we'll need to store the order of that function as well. it's not super hard but not sure whether fixing that worth the effort

I think again based on the following trade-off, I feel make it a `Message` probably a better option. @Lee-W let me know if you have different thoughts, or you think there is a better way to implement it.

Option 1: make it a message
* Implementation is straight-forward, and less maintenance efforts
* User experience might not be great, as they need to manually fix this, even though the fix is straight-forward
* Since it doesn't suggest any fix, no edge case to handle when suggesting/executing the fix

Option 2: make a "full replacement"
* In this case, we will need to store the order of the arguments for a function. Then every update (e.g., `create_asset(uri, extra)` to `create_asset(uri, metadata)`) would require an update to the rule. I am wondering if it could introduce long-term maintenance effort to ensure the rule work properly
* Wondering if the handling of starred args would be complicated (e.g., `collector.create_asset(*some_list, **some_dict)`). This might be an edge case. In `node.rs`, seems like there is a function can resolve the order for this case, so it could possibly be handled as well.

---

_Review requested from @Lee-W by @sjyangkevin on 2025-12-19 06:31_

---

_Review requested from @ntBre by @sjyangkevin on 2025-12-19 06:31_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:53 on 2025-12-19 07:10_

Do you thing we want a new enum or keep using `Replacement`? I feel like it's less likely to reuse things like `SourceModuleMoved` 

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:126 on 2025-12-19 07:11_

According to the description and the test output, we're not actually fixing anything. What is this for?

---

_@Lee-W reviewed on 2025-12-19 07:11_

Left a few comments, but looks way better! Thanks!

---

_@sjyangkevin reviewed on 2025-12-19 07:29_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:126 on 2025-12-19 07:29_

Right. Since I am referencing the AIR301 implementation, and that contains the fix. I should actually remove it

---

_Review requested from @Lee-W by @sjyangkevin on 2025-12-19 20:51_

---

_@sjyangkevin reviewed on 2025-12-19 20:53_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:53 on 2025-12-19 20:53_

I created a new enum which use similar pattern as `Replacement`. I think in future if there are different types of function signature change, we could extend that enum to define specific message or fix.

---

_@sjyangkevin reviewed on 2025-12-19 20:56_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:63 on 2025-12-19 20:56_

I am wondering whether we should put it onto the heap using `to_string()`.

The `message` is a fixed-size/static string as defined on L98.

---

_@sjyangkevin reviewed on 2025-12-19 22:56_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:63 on 2025-12-19 22:56_

I thinks as the definition `fn fix_title(&self) -> Option<String>`, we need to use `to_string()` to create a `String`.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/airflow/AIR303_args.py`:1 on 2025-12-25 18:08_

tiny nit: I would probably just call this AIR303.py unless you expect to have variants of the rule that don't apply to arguments.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:69 on 2025-12-25 18:09_

nit: Do we need the `_expr` suffix here? I'd usually only expect that if the rule were also applied to statements and there were a `_stmt` variant.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:84 on 2025-12-25 18:10_

nit: I think I would probably just inline these functions:


```suggestion
    let Expr::Call(call_expr @ ExprCall { arguments, .. }) = expr else {
        return;
    }

```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:97 on 2025-12-25 18:12_

I think it might be nicer just to emit the diagnostic here and avoid the trail of `else { return }` arms.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/helpers.rs`:41 on 2025-12-25 18:13_

Could we include the `message: &'static str` directly in the `Airflow3FunctionSignatureChange` struct? This enum doesn't look too helpful unless you have more variants planned for the future. We could at least include it directly in the `Airflow3FunctionSignatureChange` file if it's not shared with other rules.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:89 on 2025-12-25 18:15_

Another small nit, but we typically avoid abbreviating names. I think we'd usually just call this `qualified_name`.


```suggestion
    let Some(qualified_name) = typing::resolve_assignment(value, checker.semantic()) else {
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:18 on 2025-12-25 18:32_

What do you think about something like this?

```suggestion
/// Airflow 3.0 introduces changes to function signatures. Code that
/// worked in Airflow 2.x will raise a runtime error if not updated to work with Airflow
/// 3.0.
```

It seems like some function signatures have changed since y'all are adding this rule :)

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/airflow/AIR303_args.py`:10 on 2025-12-25 18:34_

This might be quite unusual, but do you want to test something like:

```py
HookLineageCollector().create_asset(positional_arg)
```

I'm not sure if `resolve_assignment` will handle that case automatically.


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:39 on 2025-12-25 18:40_

I don't think this name quite aligns with our [naming conventions](https://docs.astral.sh/ruff/contributing/#rule-naming-convention), specifically

> Ruff's rule names should make grammatical and logical sense when read as "allow ${rule}" or "allow ${rule} items", as in the context of suppression comments

but I'm not coming up with a great alternative either. Maybe something like `Airflow3IncompatibleFunctionSignature` or `Airflow3IncompatibleCall`?

I'm okay with what you have now if it sounds better to you, though.

---

_@ntBre reviewed on 2025-12-25 18:44_

Thank you! This looks great to me overall, I just had a few small suggestions.

---

_@ntBre reviewed on 2025-12-25 18:53_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/airflow/AIR303_args.py`:10 on 2025-12-25 18:53_

One other probably rare case is `hlc.create_asset(*args)`. I don't think the rule would currently flag that because `find_positional` only checks the non-starred arguments.

---

_Comment by @sjyangkevin on 2025-12-27 06:24_

Thanks @ntBre, I've made the following changes based on the feedback.

1. Renamed the test file from `AIR303_args.py` to `AIR303.py`
2. Renamed `airflow_3_function_signature_change_expr` to `airflow_3_function_signature_change`
> nit: I think I would probably just inline these functions:
3. Refactored the implementation to reduce the hierarchy of the function calls
4. Emitted the diagnostic directly to avoid the trail of `else { return; }` arms
5. Moved the enum `FunctionSignatureChangeType` into `function_signature_change_in_3.rs`, as it is less likely to be an enum shared by other modules/implementations. In case in future we need to tailor the message for different types of function signature changes, probably could still keep it.
6. Renamed `qualname` to `qualified_name` to avoid abbreviating names
7. Renamed `Airflow3FunctionSignatureChange` to `Airflow3IncompatibleFunctionSignature` to better align with naming convention
8. Updated the struct document for `Airflow3IncompatibleFunctionSignature` according to the feedback
9. Enhanced the check logic (use this condition `arguments.find_positional(0).is_some() || arguments.args.iter().any(Expr::is_starred_expr);`) to handle starred expressions (i.e., `*args` and `**kwargs`) ; the updated test cases and results are shown in `AIR303.py` and `ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`.
10. Refactored the code to use `if let` instead of `match` as we only have a single pattern matching here. Using `match` raise a clippy warnings. We can refactor this to use `match` when more cases coming in the future.

```rust
if let ["airflow", "lineage", "hook", "HookLineageCollector"] = qualified_name.segments() {
    if attr.as_str() == "create_asset" && has_positional_args {
        ...
```
11. Update the `message` to show better description in the document.
<img width="1036" height="122" alt="Screenshot from 2025-12-27 00-51-14" src="https://github.com/user-attachments/assets/da4339b5-b764-45a3-8286-961231732d63" />

@Lee-W , would you mind have a look into these changes whenever you have time, and let me know if you have further feedback?

Thanks again for both your time in reviewing this!

---

_@sjyangkevin reviewed on 2025-12-27 06:31_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:88 on 2025-12-27 06:31_

Not sure if we should switch the order here, as resolving through assignment might be the majority pattern :thinking: 

---

_Review requested from @ntBre by @sjyangkevin on 2025-12-27 06:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:14 on 2025-12-29 15:41_

nit: Could you put this at the bottom of the file? We like to keep the `Violation` and then its implementation right at the top.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:44 on 2025-12-29 15:41_

Rules have to start out in preview.


```suggestion
#[violation_metadata(preview_since = "0.14.11")]
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:75 on 2025-12-29 15:42_

nit: let's update this to match the new `Violation` name.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:88 on 2025-12-29 15:45_

This looks good to me! Checking that `value` is a `Call` should be quite cheap, and I don't mind having the special case first.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/function_signature_change_in_3.rs`:84 on 2025-12-29 15:46_

Would something like this work, or do you need `call_expr` somewhere else later?

```suggestion
    let Expr::Call(ExprCall { func, arguments, .. }) = expr else {
        return;
    };

    let Expr::Attribute(ExprAttribute { attr, value, .. }) = func else {
```

---

_@ntBre reviewed on 2025-12-29 15:49_

Thank you, this looks great!

I just had a couple more nits after we mark the rule as preview. Otherwise this is good to go if @Lee-W is happy with it too :)

---

_Comment by @Lee-W on 2025-12-29 15:57_

will take another look early tomorrow. Thanks!

---

_Comment by @sjyangkevin on 2025-12-29 18:56_

Thanks @ntBre , I've made the following updates based on your feedback.

1. Put the enum `FunctionSignatureChangeType` to the bottom of the file
2. Updated `stable_since` to `preview_since`
3. Renamed `airflow_3_function_signature_change` to `airflow_3_incompatible_function_signature` to match the new `Violation` name
4. Refactored the logic for resolving the qualified name first resolve by assignment (the common pattern)
5. `call_expr` is only used to access `func`, directly destructure it is a better approach. Thanks!

---

_Review requested from @ntBre by @sjyangkevin on 2025-12-29 18:56_

---

_@Lee-W reviewed on 2025-12-30 03:24_

LGTM! Thanks @sjyangkevin and @ntBre !

---

_@ntBre approved on 2025-12-30 16:30_

Thank you!

---

_Renamed from "[`airflow`] passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed (`AIR303`)" to "[`airflow`] Passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed (`AIR303`)" by @ntBre on 2025-12-30 16:38_

---

_Merged by @ntBre on 2025-12-30 16:39_

---

_Closed by @ntBre on 2025-12-30 16:39_

---
