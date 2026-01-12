```yaml
number: 18391
title: "Unify `OldDiagnostic` and `Message`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/combine-message-diagnostic
created_at: 2025-05-30T19:27:53Z
updated_at: 2025-06-20T20:09:39Z
url: https://github.com/astral-sh/ruff/pull/18391
synced_at: 2026-01-12T15:56:17Z
```

# Unify `OldDiagnostic` and `Message`

---

_@ntBre_

Summary
--

This PR unifies the remaining differences between `OldDiagnostic` and
`Message` (`OldDiagnostic` was only missing an optional `noqa_offset` field) and
replaces `Message` with `OldDiagnostic`.

The biggest functional difference is that the combined `OldDiagnostic` kind no
longer implements `AsRule` for an infallible conversion to `Rule`. This was
pretty easy to work around with `is_some_and` and `is_none_or` in the few places
it was needed. In `LintContext::report_diagnostic_if_enabled` we can just use
the new `Violation::rule` method, which takes care of most cases.

Most of the interesting changes are in [this range](https://github.com/astral-sh/ruff/pull/18391/files/8156992540f4e81f914a9534e99241b1e14382a1) before I started renaming.

Test Plan
--

Existing tests

Future Work
--

I think it's time to start shifting some of these fields to the new `Diagnostic`
kind. I believe we want `Fix` for sure, but I'm less sure about the others. We
may want to keep a thin wrapper type here anyway to implement a `rule` method,
so we could leave some of these fields on that too.

---

_Label `internal` added by @ntBre on 2025-05-30 19:28_

---

_Label `diagnostics` added by @ntBre on 2025-05-30 19:28_

---

_@ntBre reviewed on 2025-05-30 19:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/mod.rs`:176 on 2025-05-30 19:31_

It might be time to update the call sites.

With the exception of this method itself, I think the rest of this hunk was moved from `diagnostic.rs` with no changes.

---

_Comment by @codspeed-hq[bot] on 2025-05-30 19:33_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fcombine-message-diagnostic?runnerMode=Instrumentation)

### Merging #18391 will **not alter performance**

<sub>Comparing <code>brent/combine-message-diagnostic</code> (52a54a9) with <code>main</code> (4e83db4)</sub>



### Summary

`✅ 37` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-05-30 19:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/tokens.rs`:178 on 2025-05-30 19:34_

Looking at this again, maybe these should all be `is_some_and`. I don't think it actually matters because they should all be lint diagnostics, not syntax errors, at this point, but `is_none_or` would retain syntax errors if they were ever present at this point.

---

_@ntBre reviewed on 2025-05-30 19:34_

---

_Comment by @ntBre on 2025-05-30 19:45_

Well the benchmark results are disappointing, but we're still ahead of where we were before #18234:

|    | Benchmark                                                | Change  | #18234  |
|----|----------------------------------------------------------|---------|---------|
| ❌ | `` linter/all-rules[numpy/globals.py] ``                 | -6.81%  | +7.9%   |
| ❌ | `` linter/all-rules[pydantic/types.py] ``                | -5.4%   | +7.04%  |
| ❌ | `` linter/all-rules[unicode/pypinyin.py] ``              | -13.82% | +21.11% |
| ❌ | `` linter/all-with-preview-rules[numpy/ctypeslib.py] ``  | -4.2%   | +6.13%  |
| ❌ | `` linter/all-with-preview-rules[numpy/globals.py] ``    | -5.47%  | +6.11%  |
| ❌ | `` linter/all-with-preview-rules[pydantic/types.py] ``   | -4.74%  | +6.49%  |
| ❌ | `` linter/all-with-preview-rules[unicode/pypinyin.py] `` | -12.77% | +20.21% |
| ❌ | `` linter/all-rules[numpy/ctypeslib.py] ``               | -3%     | +5.03%  |

I think I already went through all of the `to_rule` calls, but I'll look at the flamegraphs and see if I can find any improvements.

---

_Comment by @MichaReiser on 2025-05-30 21:12_

I haven't reviewed this but I think we should remove the rule method if possible (not as part of this pr)

---

_Comment by @ntBre on 2025-06-02 20:09_

Alright, we're finally back under the codspeed threshold, but the macro changes in the last commit feel potentially hacky to me. There are also still some ~2% perf regressions, but I think I handled all of the hot spots. The remaining new code in the flamegraphs looks pretty broadly distributed, which I think means it's just from the new `Diagnostic` construction. 

One big thing I noticed is that all of the rules in `check_tokens` unconditionally push diagnostics and then a `retain` cleans them up at the end. Changing all of the `report_diagnostic` calls in that call graph to `report_diagnostic_if_enabled` should be a big help because both `Diagnostic` construction _and_ the `to_rule` call in the final `retain` are expensive. But I think that's a better fit for a follow-up.

I'll leave this in draft because I'm a bit uncertain about the proc macro changes, especially, but I think it's ready for at least a quick review.

For a little more context on the proc macro changes, my goal was to make the conversion of `NoqaCode` to `Rule` less expensive by avoiding calling `Rule::from_code`:

https://github.com/astral-sh/ruff/blob/87a15f7e4cf328e6485cee98960d19f51a955ef4/crates/ruff_linter/src/registry.rs#L18-L26

which both requires formatting a `NoqaCode` into a `String` to call and then reparses the `NoqaCode` into its parts. I moved `Linter::common_prefix` out of the `RuleNamespace` trait so that it could become `const`, which enabled me to pass its output to the `match` in `generate_rule_to_code`.

`Rule::from_code` was already faster than parsing a rule from a kebab-case rule name, which I saw in the performance report after https://github.com/astral-sh/ruff/pull/18391/commits/bba7f6df7112a1fbfca53b72a73b21ab3d27edb6. The macro changes drop the worst-case performance regression from -4.8% to -2%. That difference might be small enough not to mess with the macros anyway.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/tokens.rs`:178 on 2025-06-03 06:43_

I didn't realize this before but it's pretty scary that `check_tokens` could remove diagnostics that were added outside `check_tokens` (we aren't in control of where the context comes from). I think we should refactor this and avoid pushing diagnostics that aren't enabled instead.

---

_Comment by @MichaReiser on 2025-06-03 06:52_

I skimmed through the changes and had two thoughts:

* Would it have been easier to replace the usages of `Message::to_rule` instead of trying to optimize its performance, given that this is probably something we want to do anyway (because we can't add `Rule` to `Diagnostic` or have it anywhere in our shared rendering logic). I skimmed through the usages and I think most places can easily be replaced by testing against the noqa code.
* For the `noqa` code. One thing I mentioned in one of your previous PRs is that we could change the `NoqaCode` representation from `String, String` to `&'static str, `usize` there the `usize is the split point between prefix and suffix. Using a pre-computed offset allows both `prefix` and `suffix` to return a `&str` instead of a `String`. I'm not sure if this would have been helpful here.

---

_Comment by @ntBre on 2025-06-03 12:20_

Thanks for having a look!

> * Would it have been easier to replace the usages of `Message::to_rule` instead of trying to optimize its performance, given that this is probably something we want to do anyway

I tried that first, but my attempts turned out to be much larger refactors than expected or didn't help the performance like I hoped. For example, [this commit](https://github.com/astral-sh/ruff/pull/18391/commits/d04d40dfc3dd2966ac00e3ae3992ec4fe6560635) was a slight regression from the commit before it and also made `cmp_fix` very awkward. This could still be the right approach since we'll need it later, though, like you said.

> * For the `noqa` code. One thing I mentioned in one of your previous PRs is that we could change the `NoqaCode` representation from `String, String` to `&'static str, `usize`there the`usize is the split point between prefix and suffix. Using a pre-computed offset allows both `prefix` and `suffix` to return a `&str` instead of a `String`. I'm not sure if this would have been helpful here.

Ah I did forget about that one. I tried using `NoqaCode(Linter, &'static str)` instead of the current `NoqaCode(&'static str, &'static str)`, but not `(&str, usize)`.

I can give both of these another try, but I'll probably start in a separate PR since I think they can be handled separately.

---

_Comment by @ntBre on 2025-06-05 19:54_

I spent a while today looking into refactoring `NoqaCode` to be a `(&'static str, usize)`, and I think it's basically impossible. The `Linter::common_prefix` code is generated in the `RuleNamespace` derive macro, while the rest of the rule code is generated in the `map_codes` macro, so there's nowhere that has access to both the prefix and suffix strings to generate a single `&'static str` combining them.

Even with a const `common_prefix` method like I added here, we can't actually call that const function in the macro, so we still can't get access to it at macro-expansion time. We can get two separate const `&'static str`s, but I don't believe there's any way to combine them, at least without some wild transmutes through byte arrays, which is what the crates I found mentioning this did.

I think I need to do one more pass here to check the sites of `to_rule` calls[^1] that were added as part of the `OldDiagnostic` and `Message` merger to see if it's worth keeping the macro changes that added `NoqaCode::rule`, but otherwise this is up to date and the perf looks even better than before (worst case is -1%, down from -2%).

[^1]: To clarify, `to_rule` is still removed, they were added here before I rebased on the PR removing them, and I replaced them with `noqa_code().and_then(|code| code.rule())`.

---

_Comment by @ntBre on 2025-06-17 20:05_

I rebased on main and reverted the proc macro changes to see if they really help the benchmarks now or not.

Edit: then I dropped the revert again since reverting caused an 8.7% perf regression.

---

_Comment by @ntBre on 2025-06-18 12:46_

With #18736 on hold for now, I think this is ready for review.

---

_Marked ready for review by @ntBre on 2025-06-18 12:46_

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-18 12:46_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-06-18 12:47_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-18 12:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:65 on 2025-06-18 13:06_

One challenge with the approach you've taken here is that I don't think we can store a `NoqaCode` on `ruff_db::Diagnostic`. Ideally, the type on the `ruff_db::Diagnostic` would just be a `String` or an otherwise mostly agnostic type. 

I don't know how much that changes to your refactor. I guess we could call `Rule::from_code` in the places where we need to go back from code to `Rule`. I sort of would prefer this but I suspect that this would be slower because that requires actual parsing (and not just matching). 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:396 on 2025-06-18 13:35_

Could we account for `per_file_ignores` inside `report_diagnostic` instead of doing this post-filtering here?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:433 on 2025-06-18 13:37_

This would be a larger refactor, I think. But it would be somewhat nice if `diagnostic.build_fix` would return an `Option<FixBuilder>` that's only `Some` if we should generate a fix. But that's probably a larger refactor too. This would have the advantage that we still have a reference to `Rule` inside of `DiagnosticBuilder`. But I fear this would be another large and very painful refactor and I'm not sure if its' worth it. 

Actually. I don't think that would work because we always want to generate a fix to show it to users even when it shouldn't get applied.... Although, looking at the code here I'm not sure if that's how Ruff currently works. Either way, I think this coudl simplify things actually... 

Instead of what I wrote above. `DiagnosticBuilder::with_fix` or `set_fix` could still check if the fix should be applied and then wrap the `Fix` in a `FixWithFixability` which is an enum with two variants: `Fix(Fix)`, `DisplayOnly(Fix)` where `DisplayOnly` is a fix that we only want to show but not count in statistics or apply when applying code fixes. 

Either way. I think that would be a separate refactor

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:442 on 2025-06-18 13:42_

I guess, we could also eagerly resolve the applibabily during construction instead of doing it here. 

Should this use `noqa_code` instead of `name().parse()`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/test.rs`:330 on 2025-06-18 13:44_

This is so confusing ... looking forward to have a single `print_diagnostics` function that prints all diagnostics.

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:354 on 2025-06-18 13:45_

By how much does this blow up ruff's code size?

---

_@MichaReiser approved on 2025-06-18 13:47_

Thank you. 

I think this is a step in the right direction. What's unclear to me is how we can avoid the `noqa_code.rule()` calls entirely (or in most cases). I left a few suggestions but that might require some more larger refactors (or maybe not?). 

The reason why I think we should avoid `NoqaCode::rule` is because: a) it's slow and b) I don't think we should store a `NoqaCode` on `ruff_db::Diagnostic` (we can, but I would prefer not to)

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/tokens.rs`:178 on 2025-06-18 15:52_

Yeah I looked into this a bit originally, and it's just a big refactor. Every rule reachable from this function needs to be updated to check if its rule is enabled before emitting a diagnostic. But it will make things simpler and more performant too. (And less scary, I didn't even consider dropping unrelated diagnostics before!)

I'll tackle that separately!

---

_@ntBre reviewed on 2025-06-18 15:53_

---

_@ntBre reviewed on 2025-06-18 15:59_

---

_Review comment by @ntBre on `crates/ruff_macros/src/map_codes.rs`:354 on 2025-06-18 15:59_

I guess if there's one `NoqaCode` per rule and ~900 rules, this adds ~900 constants (which might get inlined?) and ~900 `match` arms. Is that a lot? I don't really have a good intuition for the effects of these kinds of changes on code size.

Anyway, I'm still hoping we can revert this part of the PR if we can recover the perf hits in other ways, hopefully from your other suggestions.

---

_@MichaReiser reviewed on 2025-06-18 16:02_

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:354 on 2025-06-18 16:02_

I suggest building ruff and comparing the binary sizes. You can also use https://github.com/RazrFalcon/cargo-bloat to get an understanding of the compiled code

---

_@ntBre reviewed on 2025-06-18 16:23_

---

_Review comment by @ntBre on `crates/ruff_macros/src/map_codes.rs`:354 on 2025-06-18 16:23_

I got these results from `du -b target/release/ruff`:

```
41,559,344 bytes for main
41,669,840 bytes for this branch
```

so I guess this one is 110 kilobytes bigger. `NoqaCode::rule` does make an appearance in cargo-bloat too. `Rule::from_str` is present in both, so it would be nice if we ever get to remove that too.

<details><summary>cargo-bloat output</summary>

this branch:
```
 File  .text    Size               Crate Name
 0.2%   0.3% 76.0KiB         ruff_linter ruff_linter::checkers::ast::analyze::expression::expression
 0.1%   0.3% 72.1KiB                ruff <ruff::args::CheckCommand as clap_builder::derive::Args>::augment_args
 0.1%   0.2% 51.4KiB     ruff_workspace? <ruff_workspace::options::_::<impl serde::de::Deserialize for ruff_workspace::options::LintOptionsWire>::deserialize::__Visitor as serde::de::Visitor>::visit_map
 0.1%   0.2% 51.4KiB     ruff_workspace? <ruff_workspace::options::_::<impl serde::de::Deserialize for ruff_workspace::options::LintOptionsWire>::deserialize::__Visitor as serde::de::Visitor>::visit_map
 0.1%   0.2% 51.4KiB     ruff_workspace? <ruff_workspace::options::_::<impl serde::de::Deserialize for ruff_workspace::options::LintOptionsWire>::deserialize::__Visitor as serde::de::Visitor>::visit_map
 0.1%   0.2% 43.8KiB        ruff_linter? <ruff_linter::codes::Rule as core::str::traits::FromStr>::from_str
 0.1%   0.2% 39.8KiB         ruff_linter ruff_linter::checkers::ast::analyze::statement::statement
 0.1%   0.2% 35.7KiB     ruff_workspace? <ruff_workspace::options::_::<impl serde::de::Deserialize for ruff_workspace::options::LintOptionsWire>::deserialize::__Visitor as serde::de::Visitor>::visit_map
 0.1%   0.2% 35.7KiB     ruff_workspace? <ruff_workspace::options::_::<impl serde::de::Deserialize for ruff_workspace::options::LintOptionsWire>::deserialize::__Visitor as serde::de::Visitor>::visit_map
 0.1%   0.2% 35.3KiB         ruff_linter <alloc::vec::Vec<T,A> as core::clone::Clone>::clone
 0.1%   0.2% 35.1KiB ruff_python_codegen ruff_python_codegen::generator::Generator::unparse_stmt
 0.1%   0.1% 32.4KiB      ruff_workspace ruff_workspace::configuration::LintConfiguration::as_rule_table
 0.1%   0.1% 32.4KiB      ruff_workspace ruff_workspace::configuration::Configuration::into_settings
 0.1%   0.1% 31.3KiB         ruff_linter ruff_linter::checkers::ast::analyze::definitions::definitions
 0.1%   0.1% 31.3KiB         ruff_linter ruff_linter::checkers::ast::analyze::deferred_scopes::deferred_scopes
 0.1%   0.1% 31.2KiB         ruff_linter ruff_linter::codes::NoqaCode::rule
 0.1%   0.1% 30.8KiB      ruff_workspace ruff_workspace::configuration::Configuration::from_options
 0.1%   0.1% 29.8KiB       libcst_native libcst_native::parser::grammar::python::__parse_compound_stmt
 0.1%   0.1% 29.5KiB         ruff_server <toml::value::Value as serde::de::Deserializer>::deserialize_any
 0.1%   0.1% 29.5KiB                ruff <toml::value::Value as serde::de::Deserializer>::deserialize_any
44.5%  95.4% 21.3MiB                     And 33811 smaller methods. Use -n N to show more.
46.7% 100.0% 22.4MiB                     .text section size, the file size is 47.9MiB
```

main:
```
 File  .text    Size               Crate Name
 0.1%   0.3% 72.1KiB                ruff <ruff::args::CheckCommand as clap_builder::derive::Args>::augment_args
 0.1%   0.2% 51.4KiB     ruff_workspace? <ruff_workspace::options::_::<impl serde::de::Deserialize for ruff_workspace::options::LintOptionsWire>::deserialize::__Visitor as serde::de::Visitor>::visit_map
 0.1%   0.2% 51.4KiB     ruff_workspace? <ruff_workspace::options::_::<impl serde::de::Deserialize for ruff_workspace::options::LintOptionsWire>::deserialize::__Visitor as serde::de::Visitor>::visit_map
 0.1%   0.2% 51.4KiB     ruff_workspace? <ruff_workspace::options::_::<impl serde::de::Deserialize for ruff_workspace::options::LintOptionsWire>::deserialize::__Visitor as serde::de::Visitor>::visit_map
 0.1%   0.2% 46.2KiB         ruff_linter ruff_linter::checkers::ast::analyze::statement::statement
 0.1%   0.2% 43.8KiB        ruff_linter? <ruff_linter::codes::Rule as core::str::traits::FromStr>::from_str
 0.1%   0.2% 37.8KiB         ruff_linter ruff_linter::checkers::ast::analyze::expression::expression
 0.1%   0.2% 35.7KiB     ruff_workspace? <ruff_workspace::options::_::<impl serde::de::Deserialize for ruff_workspace::options::LintOptionsWire>::deserialize::__Visitor as serde::de::Visitor>::visit_map
 0.1%   0.2% 35.7KiB     ruff_workspace? <ruff_workspace::options::_::<impl serde::de::Deserialize for ruff_workspace::options::LintOptionsWire>::deserialize::__Visitor as serde::de::Visitor>::visit_map
 0.1%   0.2% 35.3KiB         ruff_linter <alloc::vec::Vec<T,A> as core::clone::Clone>::clone
 0.1%   0.2% 35.1KiB ruff_python_codegen ruff_python_codegen::generator::Generator::unparse_stmt
 0.1%   0.1% 32.4KiB      ruff_workspace ruff_workspace::configuration::LintConfiguration::as_rule_table
 0.1%   0.1% 32.4KiB      ruff_workspace ruff_workspace::configuration::Configuration::into_settings
 0.1%   0.1% 30.8KiB      ruff_workspace ruff_workspace::configuration::Configuration::from_options
 0.1%   0.1% 29.8KiB       libcst_native libcst_native::parser::grammar::python::__parse_compound_stmt
 0.1%   0.1% 29.5KiB         ruff_server <toml::value::Value as serde::de::Deserializer>::deserialize_any
 0.1%   0.1% 29.5KiB                ruff <toml::value::Value as serde::de::Deserializer>::deserialize_any
 0.1%   0.1% 28.7KiB      libcst_native? <libcst_native::nodes::expression::Expression as core::clone::Clone>::clone
 0.1%   0.1% 28.7KiB         ruff_linter ruff_linter::checkers::ast::analyze::deferred_scopes::deferred_scopes
 0.1%   0.1% 27.9KiB                ruff <ruff::args::FormatCommand as clap_builder::derive::Args>::augment_args
44.7%  95.5% 21.3MiB                     And 33578 smaller methods. Use -n N to show more.
46.7% 100.0% 22.3MiB                     .text section size, the file size is 47.8MiB
```

</details>

---

_@ntBre reviewed on 2025-06-18 21:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/tokens.rs`:178 on 2025-06-18 21:11_

Resolved by https://github.com/astral-sh/ruff/pull/18769

---

_@ntBre reviewed on 2025-06-19 00:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:396 on 2025-06-19 00:44_

Yeah, I guess we could check it in `report_diagnostic`, but that would be another case that would require returning an `Option<DiagnosticGuard>` right?

It looks like we could also just apply the per-file ignores to the `LinterSettings` at the top of `check_path` (or possibly even earlier). This changes one RUF100 snapshot from

```
        0       │-RUF100_2.py:1:19: RUF100 [*] Unused `noqa` directive (unused: `F401`)
              0 │+RUF100_2.py:1:19: RUF100 [*] Unused `noqa` directive (non-enabled: `F401`)
```

but I think `non-enabled` is actually more correct. All of the other tests pass.

---

_@MichaReiser reviewed on 2025-06-19 09:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:396 on 2025-06-19 09:00_

Ah right, we don't return `Option` today. But yes, I think we could just apply it to the `LinterSettings`!

---

_Comment by @MichaReiser on 2025-06-19 09:00_

By the way. I don't think any of those other refactors should block this PR. We can merge it and then try to reduce the places where we use `noqa_code().rule()` (Ideally, remove them all)

---

_Closed by @ntBre on 2025-06-19 12:55_

---

_Reopened by @ntBre on 2025-06-19 12:55_

---

_Merged by @ntBre on 2025-06-19 13:37_

---

_Closed by @ntBre on 2025-06-19 13:37_

---

_Branch deleted on 2025-06-19 13:38_

---

_@MichaReiser reviewed on 2025-06-19 13:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:396 on 2025-06-19 13:54_

Not only would this be more correct, it should also be faster because we can skip the rules entirely. I'm not sure if we want to change how we construct `LinterSettings` (we probably shouldn't if it requires a deep clone). But checking the rule set as resolved for this file inside `is_enabled` definetely makes sense

---

_@ntBre reviewed on 2025-06-19 14:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:396 on 2025-06-19 14:11_

Yeah I'm playing with this a bit today. It's easy enough to apply the per-file ignores at the top of `check_path`, but it does look like it needs a deep clone. I tried just cloning the rules table, but some of the nested calls expect the value on `settings` to be updated too.

Maybe we could have a wrapper around `LinterSettings` that is bound to a single path, or something like that. 

---

_@MichaReiser reviewed on 2025-06-19 14:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:396 on 2025-06-19 14:18_

> I tried just cloning the rules table, but some of the nested calls expect the value on settings to be updated too.

Uhm, that's annoying. could we refactor those calls or are there simply too many/are hard to find?

---

_@ntBre reviewed on 2025-06-19 14:41_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:396 on 2025-06-19 14:41_

"Some" might have been a bit of an understatement. I think every, or nearly every, place we currently check if rules are enabled uses `settings.rules.enabled`, so it would be a pretty big refactor (making `LinterSettings::rules` private "only" causes 88 compilation errors, so maybe it's not _every_ rule). I guess I'd end up touching all of those call sites with a wrapper too, though. I was picturing something like this:

```rust
struct ResolvedLinterSettings<'a> {
    settings: &'a LinterSettings,
    per_file_ignores: RuleSet,
}

impl<'a> ResolvedLinterSettings<'a> {
    fn new(settings: &'a LinterSettings, path: &Path) -> Self {
        Self { settings, per_file_ignores: fs::ignores_from_path(path, &settings.per_file_ignores) }
    }

    fn enabled(&self, rule: Rule) -> bool {
        self.settings.rules.enabled(rule) && !self.per_file_ignores.contains(rule)
    }
}
```

or I guess cloning `self.settings.rules.enabled` and combining it with `per_file_ignores` more eagerly.

Do you like that idea, or would you prefer trying to pass down the updated rules table?

---

_@MichaReiser reviewed on 2025-06-19 14:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:396 on 2025-06-19 14:44_

Oh you're right. I thought we added a `checker` (or `context`) `is_rule_enabled` method but it seems everyone just calls `settings.rule.enabled` directly :( But given that there are only 74 references, this might not be too bad to refactor (and we now have the right place to add the methnod -> LintContext)

---

_@ntBre reviewed on 2025-06-19 14:47_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:396 on 2025-06-19 14:47_

Oh that's true. I was worried about passing a `Path` everywhere, but if we can encapsulate it in the checker or context, that should work well. Thanks!

---

_@ntBre reviewed on 2025-06-20 20:09_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:433 on 2025-06-20 20:09_

I could certainly be wrong, but my reading of this section of code is that it's removing fixes that the user explicitly marked as unfixable. Should we still generate a fix for display in those cases?

If not, I think I came up with a very simple solution, just applying both the applicability from your other comment and this `should_fix` check in `DiagnosticGuard::set_fix`. I'll open a PR soon to see what you think.

---
