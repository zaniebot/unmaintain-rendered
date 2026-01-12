```yaml
number: 18234
title: "Add a `ViolationMetadata::rule` method"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/violation-rules
created_at: 2025-05-20T22:36:38Z
updated_at: 2025-05-28T13:27:10Z
url: https://github.com/astral-sh/ruff/pull/18234
synced_at: 2026-01-12T15:56:15Z
```

# Add a `ViolationMetadata::rule` method

---

_@ntBre_

Summary
--

This PR adds a macro-generated method to retrieve the `Rule` associated with a given `Violation` struct, which makes it substantially cheaper than parsing from the rule name. The rule is then converted to a `NoqaCode` for storage on the `Message` (and eventually on the new diagnostic type). The `ViolationMetadata::rule_name` method was now unused, so the `rule` method replaces it.

Several types had to be moved from the `ruff_diagnostics` crate to the `ruff_linter` crate to make this work, namely the `Violation` traits and the old `Diagnostic` type, which had a constructor generic over a `Violation`.

It's actually a fairly small PR, minus the hundreds of import changes. The main changes are in these files:

- [crates/ruff_linter/src/message/mod.rs](https://github.com/astral-sh/ruff/pull/18234/files#diff-139754ea310d75f28307008d21c771a190038bd106efe3b9267cc2d6c0fa0921)
- [crates/ruff_diagnostics/src/lib.rs](https://github.com/astral-sh/ruff/pull/18234/files#diff-8e8ea5c586935bf21ea439f24253fcfd5955d2cb130f5377c2fa7bfee3ea3a81)
- [crates/ruff_linter/src/diagnostic.rs](https://github.com/astral-sh/ruff/pull/18234/files#diff-1d0c9aad90d8f9446079c5be5f284150d97797158715bd9729e6f1f70246297a)
- [crates/ruff_linter/src/lib.rs](https://github.com/astral-sh/ruff/pull/18234/files#diff-eb93ef7e78a612f5fa9145412c75cf6b1a5cefba1c2233e4a11a880a1ce1fbcc)

Test Plan
--

Existing tests

---

_Comment by @codspeed-hq[bot] on 2025-05-20 22:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fviolation-rules)

### Merging #18234 will **improve performances by 21.11%**

<sub>Comparing <code>brent/violation-rules</code> (daa0d26) with <code>main</code> (a3ee6bb)</sub>



### Summary

`⚡ 8` improvements  
`✅ 26` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` linter/all-rules[numpy/ctypeslib.py] `` | 4.2 ms | 4 ms | +5.03% |
| ⚡ | `` linter/all-rules[numpy/globals.py] `` | 738.4 µs | 684.4 µs | +7.9% |
| ⚡ | `` linter/all-rules[pydantic/types.py] `` | 8.8 ms | 8.2 ms | +7.04% |
| ⚡ | `` linter/all-rules[unicode/pypinyin.py] `` | 2.2 ms | 1.8 ms | +21.11% |
| ⚡ | `` linter/all-with-preview-rules[numpy/ctypeslib.py] `` | 5.1 ms | 4.8 ms | +6.13% |
| ⚡ | `` linter/all-with-preview-rules[numpy/globals.py] `` | 842.5 µs | 794 µs | +6.11% |
| ⚡ | `` linter/all-with-preview-rules[pydantic/types.py] `` | 10.5 ms | 9.9 ms | +6.49% |
| ⚡ | `` linter/all-with-preview-rules[unicode/pypinyin.py] `` | 2.5 ms | 2.1 ms | +20.21% |


---

_Comment by @github-actions[bot] on 2025-05-20 22:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @ntBre on 2025-05-20 22:45_

Those benchmarks are pretty promising, maybe this is worth pursuing.

---

_Comment by @MichaReiser on 2025-05-21 06:11_

The speedups look promising. However, I don't think we want or even can store a `Rule` on the new `ruff_db::Diagnostic`. How would this work in that new model?

I think my overall preference would be to reduce the need to access `Rule` after `report_diagnostic` has been called (and instead, use `NoqaCode` directly)

---

_Comment by @github-actions[bot] on 2025-05-27 00:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @ntBre on 2025-05-27 00:36_

I think this is ready for review. I tried to regroup all of the imports I added to keep the `crate` blocks separate from the others.

I also changed `Message` to store a `NoqaCode` instead of a `Rule` directly, fortunately without losing much, if any, of the performance improvement. Some of the smaller benchmarks actually sped up a little, I think.

I also moved the `RULE` constant to a `ViolationMetadata::rule` method, replacing `ViolationMetadata::rule_name`, which was now unused.

---

_Label `diagnostics` added by @ntBre on 2025-05-27 00:36_

---

_Label `internal` added by @ntBre on 2025-05-27 00:36_

---

_Renamed from "[test] Try adding a `Violation::RULE` constant" to "Add a `ViolationMetadata::rule` method" by @ntBre on 2025-05-27 00:37_

---

_@ntBre reviewed on 2025-05-27 00:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/lib.rs`:18 on 2025-05-27 00:48_

This is not strictly necessary, it just made rewriting some of the imports automatically a little easier.

---

_Marked ready for review by @ntBre on 2025-05-27 00:55_

---

_Review requested from @AlexWaygood by @ntBre on 2025-05-27 00:55_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-05-27 00:55_

---

_Review requested from @MichaReiser by @ntBre on 2025-05-27 00:55_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/diagnostic.rs`:28 on 2025-05-27 05:50_

Do we allow `needless_pass_by_value` because passing by reference would require changing all call sites? It might be worth adding a note why we allow this.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/diagnostic.rs`:20 on 2025-05-27 05:51_

Our plan is to remove this `Diagnostic` type with `ruff_db::Diagnostic`. What's your plan on how to remove `rule` from here because we can't add it to `ruff_db::Diagnostic`.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/lib.rs`:18 on 2025-05-27 05:52_

Do we need all the re-exports. E.g. `SourceMap` is probably less common

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:478 on 2025-05-27 05:54_

Can we move this impl block out of the macro code? It doesn't seem to depend on any placeholders (same for the `AsRule` implementation)

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:467 on 2025-05-27 05:55_

I think one of the next important refactors is to remove `rule` (and `AsRule`) from `Diagnostic` because that won't work long-term

---

_@MichaReiser approved on 2025-05-27 05:56_

Nice! 

The only thing that's unclear to me and I would like to get a short summary on what the next steps are is how you plan on removing `rule` (and `AsRule`) from `Diagnostic` because we won't be able to add it to `ruff_db::Diagnostic`. IMO, I think that's the next important refactor we shoul do

---

_Comment by @ntBre on 2025-05-27 13:43_

> The only thing that's unclear to me and I would like to get a short summary on what the next steps are is how you plan on removing `rule` (and `AsRule`) from `Diagnostic` because we won't be able to add it to `ruff_db::Diagnostic`. IMO, I think that's the next important refactor we shoul do

My plan was to get rid of this old `Diagnostic` kind entirely soon. I was picturing `Message` as its successor, which is why I only updated it to use the `NoqaCode` instead of also updating `Diagnostic`. I was thinking that would take care of most uses, but the `rule_enabled` checks might be slightly trickier. We could still implement `AsRule` via `FromStr` as long as we have the rule name stored, or we can possibly move that enabled check into `Checker::report_diagnostic` like we discussed a bit on #18232.

In short, I think most of the need for this field goes away when we combine the diagnostic types, and we should be able to work around the rest if needed. Does that sound right to you?

---

_@ntBre reviewed on 2025-05-27 13:47_

---

_Review comment by @ntBre on `crates/ruff_macros/src/map_codes.rs`:478 on 2025-05-27 13:47_

Oh yes, good catch!

---

_@ntBre reviewed on 2025-05-27 13:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/diagnostic.rs`:28 on 2025-05-27 13:50_

Yes that's right. I was a bit confused why it wasn't already needless and thought further refactoring (like combining this with `Checker::report_diagnostic` in most cases) might make it not needless again. I'll add a note!

---

_@ntBre reviewed on 2025-05-27 13:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/lib.rs`:18 on 2025-05-27 13:55_

Definitely not, I just got a bit tired/lazy updating imports and wanted to replace all `ruff_diagnostics::*` imports with `crate::*` in this crate. It should be easy enough to remove the less common ones like `IsolationLevel` and `SourceMap`, though.

---

_Comment by @MichaReiser on 2025-05-27 13:55_

>  I was thinking that would take care of most uses, but the rule_enabled checks might be slightly trickier. 

Do you have a link to this check. I think that should be fine as long as you have a `ViolationMetadata`.

> In short, I think most of the need for this field goes away when we combine the diagnostic types, and we should be able to work around the rest if needed. Does that sound right to you?

It does sound right in that this is what I would hope to be possible. I don't have a clear enough picture to say whether it will be possible.

---

_Comment by @ntBre on 2025-05-27 14:09_

> Do you have a link to this check. I think that should be fine as long as you have a `ViolationMetadata`.

Oops, I meant `Checker::enabled`:

https://github.com/astral-sh/ruff/blob/8d5655a7baa1ecc84a906cbfc0e2294cf173dee7/crates/ruff_linter/src/checkers/ast/mod.rs#L454-L457

I think we can work around the uses that don't pass a `Rule` directly easily enough with a `ViolationMetadata`, like you said.

> It does sound right in that this is what I would hope to be possible. I don't have a clear enough picture to say whether it will be possible.

Sounds good. It's not entirely clear to me either, but it feels like it should work!


---

_Comment by @MichaReiser on 2025-05-27 14:15_

Why would we have to change `checker.enabled`? Most call sites should have access to the rule or are there call sites where we check if the diagnostic's rule is enabled?

---

_Comment by @ntBre on 2025-05-27 14:21_

Yes, there are call sites where `diagnostic.rule()` is passed. Some of them are already replaced in #18232, but there are still a few left.

---

_Comment by @ntBre on 2025-05-27 14:43_

Did I really break the mypy primer? I didn't think I touched any ty files, but everything should be up to date and it looks okay on main. I also tried rerunning once. It looks like this is the root cause:

```
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/function/execute.rs:209:25 when checking `/tmp/mypy_primer/projects/arviz/arviz/data/inference_data.py`: `infer_definition_types(Id(c2a15)): execute: too many cycle iterations`
```

---

_Comment by @MichaReiser on 2025-05-27 15:32_

> Did I really break the mypy primer? I didn't think I touched any ty files, but everything should be up to date and it looks okay on main. I also tried rerunning once. It looks like this is the root cause:

seems unlikely. 

---

_Comment by @AlexWaygood on 2025-05-27 15:46_

I think it's possible that a recent commit on arviz (https://github.com/arviz-devs/arviz/commit/3205b82bb4d6097c31f7334d7ac51a6de37002d0? it landed a few hours ago) means that ty now crashes on that project when we didn't previously. There's the same crash being reported on https://github.com/astral-sh/ruff/pull/18333

---

_Comment by @AlexWaygood on 2025-05-27 15:47_

if so we should just move the project from `good.txt` to `bad.txt` (these files [here
(https://github.com/astral-sh/ruff/tree/main/crates/ty_python_semantic/resources/primer)) but would be good to confirm that that's the cause first

---

_Comment by @AlexWaygood on 2025-05-27 16:38_

Confirmed locally that ty @ Ruff's `main` branch crashes on arviz @ https://github.com/arviz-devs/arviz/commit/3205b82bb4d6097c31f7334d7ac51a6de37002d0 but not on the prior commit

---

_Comment by @AlexWaygood on 2025-05-27 16:43_

filed https://github.com/astral-sh/ruff/pull/18336 to fix

---

_Comment by @AlexWaygood on 2025-05-27 16:52_

> filed #18336 to fix

Which has now landed -- the primer panic should go away if you rebase!

---

_Comment by @ntBre on 2025-05-27 18:49_

Amazing, thank you @AlexWaygood!

---

_Comment by @ntBre on 2025-05-27 18:59_

Hmm, I got a new cascade of errors on this one.

```
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/views.rs:114:9 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/build.py`: `No downcaster registered for type `dyn ty_python_semantic::db::Db` in `Views``
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/__main__.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/parser.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/grammar_parser.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/grammar.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/sccutils.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/first_sets.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/__init__.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/tokenizer.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/utils.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/python_generator.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/parser_generator.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/grammar_visualizer.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/web.py`: `index `23` is uninitialized`
fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/[481](https://github.com/astral-sh/ruff/actions/runs/15283313638/job/42987393320?pr=18234#step:6:482)8b15/src/zalsa.rs:210:32 when checking `/tmp/mypy_primer/projects/pegen/src/pegen/validator.py`: `index `23` is uninitialized
```

I'll try rerunning once to see if that helps.

---

_Merged by @ntBre on 2025-05-28 13:27_

---

_Closed by @ntBre on 2025-05-28 13:27_

---

_Branch deleted on 2025-05-28 13:27_

---
