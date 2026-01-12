```yaml
number: 20010
title: "[`flake8-pyi`] Avoid syntax error from conflict with `PIE790` (`PYI021`)"
type: pull_request
state: merged
author: second-ed
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/PYI021_isolation_level
created_at: 2025-08-20T20:17:27Z
updated_at: 2025-09-24T21:52:07Z
url: https://github.com/astral-sh/ruff/pull/20010
synced_at: 2026-01-12T15:56:52Z
```

# [`flake8-pyi`] Avoid syntax error from conflict with `PIE790` (`PYI021`)

---

_@second-ed_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
First contribution so please let me know if I've made a mistake anywhere. This was aimed to fix #19982, it adds the isolation level to PYI021 to in the same style as the PIE790 rule.

fixes: #19982

## Test Plan

<!-- How was it tested? -->
I added a case to the PYI021.pyi file where the two rules are present as there wasn't a case with them both interacting, using the minimal reproducible example that @ntBre created on the issue (I think I got the `# ERROR` markings wrong, so please let me know how to fix that if I did).


---

_Review requested from @AlexWaygood by @second-ed on 2025-08-20 20:17_

---

_Comment by @github-actions[bot] on 2025-08-20 20:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-08-20 21:21_

Awesome, thanks for working on this! One note, I believe the `# ERROR` comments in that file are just to make reviewing snapshots a little easier and don't actually activate the rules. The file `PYI021.pyi` is used as one of the `test_case`s in this test:

https://github.com/astral-sh/ruff/blob/39f7a876d6aceedc3db4e3d63250d78bbb830fc2/crates/ruff_linter/src/rules/flake8_pyi/mod.rs#L130-L139

so only PYI021 is active still:

https://github.com/astral-sh/ruff/blob/39f7a876d6aceedc3db4e3d63250d78bbb830fc2/crates/ruff_linter/src/rules/flake8_pyi/mod.rs#L39

I think you'll probably want to create a copy of this test function and enable both the rules. Something like this:

```rust
    #[test_case(Path::new("PYI021.pyi"))]
    fn pyi021_pie790_empty_function(path: &Path) -> Result<()> {
        let diagnostics = test_path(
            Path::new("flake8_pyi").join(path).as_path(),
            &settings::LinterSettings::for_rules([Rule::DocstringInStub, Rule::UnnecessaryPlaceholder]),
        )?;
        assert_diagnostics!(diagnostics);
        Ok(())
    }
```

Or you could just hard-code the path in this case since I doubt we'll need multiple `test_case`s :) It might also make sense to use a separate `pyi` file from the main `PYI021.pyi` case, but I don't think that's a big deal either way, up to you.

I think your fix is likely still correct, but this will make sure we're testing the right thing!

---

_Label `bug` added by @ntBre on 2025-08-20 21:22_

---

_Comment by @second-ed on 2025-08-20 21:45_

Perfect, thanks for the suggestions. I'll make those changes tomorrow!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-21 21:07_

---

_Comment by @second-ed on 2025-08-21 21:07_

Hey, I added the changes you suggested but I wasn't getting a pass on the test locally (as it was panicking after applying both fixes), I think there's something wrong with my isolation level check in the actual implementation. I've pushed up the code so you can see, but will have a play with it after work tomorrow and try to work out the issue

---

_Comment by @ntBre on 2025-08-21 21:20_

I'll take a look. `Isolation` may not be enough. We may need to check explicitly (in one or both rules) that we're not leaving an empty function body. That might be a good separate test to try, or check if we already have some. Do the rules individually suggest deleting the last statement in a function?

---

_Comment by @ntBre on 2025-08-21 22:36_

Ah sorry, this might actually be trickier than I expected. PYI021 only looks at the deferred definitions after the AST has been traversed, so `checker.semantic().current_statement_id()` is returning `None`. That explains why `Isolation` isn't working. But both rules try to check that they aren't deleting the last statement in a definition, so `Isolation` also still seems like the right fix. I'm just not seeing a way to get a real `NodeId` to pass to `Checker::isolation` from a `Definition` or docstring literal.

We may have to move PYI021's check into the same phase of analysis as PIE790, which would be a bit more involved.

https://github.com/astral-sh/ruff/blob/a50b389d626528d55c22979dac50c48387a5e1e0/crates/ruff_linter/src/checkers/ast/analyze/suite.rs#L8-L12

---

_Converted to draft by @second-ed on 2025-08-22 18:28_

---

_Comment by @second-ed on 2025-08-22 18:29_

Hey, having a look through now, will give my best guess but I'm sure you'll have a better idea! I'll commit as far as I get

---

_Comment by @second-ed on 2025-08-22 19:16_

My initial plan is:
add both rules to a `IsolationLevel::Group` then look instead of getting the `current_statement_id` get something like the `current_scope` or something like that, does that sound reasonable?

---

_Marked ready for review by @second-ed on 2025-08-23 08:39_

---

_Renamed from "[ruff] #19982 add isolation rule for PYI021" to "[`flake8-pyi`] add isolation level for interaction between rules #19982 (PYI021)" by @second-ed on 2025-08-24 12:14_

---

_Comment by @ntBre on 2025-08-26 19:16_

Sorry, I don't mean to leave you hanging on this, I'm just not quite sure the best approach yet either. I don't think I mentioned this above, but I even tried something like this method added to our `SemanticModel`:

```rust
    pub fn statement_id(&self, stmt: &ast::Stmt) -> Option<NodeId> {
        self.nodes.iter().find(|node| self.statement(*node) == stmt)
    }
```

without much luck. Basically your idea sounds right to me, and is what I expected the fix to be too, but I'm not sure if it works with the way we're traversing the AST or calling this rule.

Don't feel any pressure to keep working on this, I'll try to take another look at some point too.


---

_Comment by @second-ed on 2025-08-26 20:12_

Hey no worries - I can see you're pretty busy on a load of other PRs! 

I'm happy to keep tweaking and chipping away at ideas, it's a good opportunity for me to get familiar with more of the codebase.

Out of interest what `Stmt` are you passing into this method?
```rust
    pub fn statement_id(&self, stmt: &ast::Stmt) -> Option<NodeId> {
        self.nodes.iter().find(|node| self.statement(*node) == stmt)
    }
```

Maybe passing the statement that holds the ellipsis expression could work? 

---

_Comment by @second-ed on 2025-09-03 20:16_

I've had a play, I added the method you shared. After running `dbg!` on all of the statements in the `docstring_in_stubs`, they all seem to be returning None. 

The fact that the `self.node_id` is None makes me agree with you that we should move the rule itself to the same point where the other rule is triggered, but that causes issues in that the function signature will need to change because the function that calls `unnecessary_placeholder` uses `suite: &[Stmt]` whereas the other one uses `definition: &Definition`

I think that the self.node_id is always None because it's being called after the ast has finished traversing?

```rust
[crates/ruff_python_semantic/src/model.rs:1414:9] &self.node_id = None
[crates/ruff_linter/src/rules/flake8_pyi/rules/docstring_in_stubs.rs:65:5] checker.semantic().statement_id(&statements[0]) = None
[crates/ruff_python_semantic/src/model.rs:1414:9] &self.node_id = None
[crates/ruff_linter/src/rules/flake8_pyi/rules/docstring_in_stubs.rs:66:5] checker.semantic().statement_id(&statements[1]) = None
[crates/ruff_python_semantic/src/model.rs:1414:9] &self.node_id = None
```

---

_Comment by @ntBre on 2025-09-03 20:49_

That all sounds right to me. I think the best course of action is to move it to the earlier analysis phase, while we're still traversing the AST, like `PIE790`. I was quite hesitant to do that earlier, but I think it won't be too bad after looking again today. We should still be able to call `docstring_from` on just a `&[Stmt]`, which we have access to in that phase:

https://github.com/astral-sh/ruff/blob/bbfcf6e111648052783bdb68ec71706fc69b714f/crates/ruff_linter/src/docstrings/extraction.rs#L6-L8

and the only reason we pass a `Definition` to `PYI021` is also to extract its `&[Stmt]`.

So this rule actually seems to fit nicely into `analyze::suite` next to `PIE790`:

https://github.com/astral-sh/ruff/blob/bbfcf6e111648052783bdb68ec71706fc69b714f/crates/ruff_linter/src/checkers/ast/analyze/suite.rs#L9-L12

unless there's some additional subtlety I'm missing.

---

_Comment by @second-ed on 2025-09-03 21:14_

Awesome sounds good, cheers for the hint I'll give it a go!

---

_Comment by @second-ed on 2025-09-04 17:42_

I think I've got it there now, all tests pass without any hardcoding.

I moved `in_protocol_or_abstract_method` to the semantic model itself because I thought I might need to check it in PYI021 but didn't need it in the end so can move it back if you want although I think it makes more sense as a method than a free function

I know you're busy so no rush to review, but let me know what you think

---

_Comment by @second-ed on 2025-09-08 17:39_

Hey @ntBre just realised I forgot to tag you in my last message, just to let you know I think this one is ready for review whenever you get a chance, cheers!

---

_Comment by @second-ed on 2025-09-08 19:01_

Sorry! forgot to update the snapshot tests, will do now

---

_Comment by @second-ed on 2025-09-08 19:57_

Hmmm I'm not getting the same fail when I run the tests locally I've run the following commands:

```shell
cargo insta test --review
```

and this which I copied from the git action
```shell
cargo insta test --all-features --unreferenced reject --test-runner nextest
```

the tests all pass when I run the code locally, not sure if I've missed something 🤔 

no rush at all @ntBre , but please let me know if theres something I've overlooked! cheers

---

_Comment by @ntBre on 2025-09-08 20:10_

You may need to rebase/merge main and then accept the snapshots again. It looks like an artifact from our recent diagnostic refactor, which changed the output format a bit.

(Sorry for the delayed review. The code changes look good from a quick skim, just trying to finish the 0.13 release before doing a proper review)

---

_Review requested from @carljm by @second-ed on 2025-09-09 16:45_

---

_Review requested from @sharkdp by @second-ed on 2025-09-09 16:45_

---

_Review requested from @dcreager by @second-ed on 2025-09-09 16:45_

---

_Review requested from @MichaReiser by @second-ed on 2025-09-09 16:45_

---

_Review requested from @dhruvmanila by @second-ed on 2025-09-09 16:45_

---

_Comment by @second-ed on 2025-09-09 17:39_

Might be a good candidate to squash when merging 😅 sorry!

---

_Comment by @ntBre on 2025-09-09 17:49_

No worries, we always squash merge! It does look like something went slightly awry with the merge. At least on GitHub, it's showing 201 commits. I think it should still be okay, though. Only the relevant changes are showing up.

---

_Review request for @carljm removed by @ntBre on 2025-09-09 17:49_

---

_Review request for @sharkdp removed by @ntBre on 2025-09-09 17:49_

---

_Review request for @dcreager removed by @ntBre on 2025-09-09 17:49_

---

_Review request for @MichaReiser removed by @ntBre on 2025-09-09 17:49_

---

_Review request for @dhruvmanila removed by @ntBre on 2025-09-09 17:49_

---

_Review requested from @ntBre by @ntBre on 2025-09-09 17:49_

---

_Comment by @second-ed on 2025-09-09 18:06_

Yeah sorry, I think I botched the rebase (thats what I get for rebasing when I'm firmly in camp merge!). I had a look at the changes too and they only are the ones I introduced so should be ok 🤞 

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/model.rs`:1411 on 2025-09-23 21:25_

Is this called anywhere now? I think we can revert this.

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/model.rs`:1671 on 2025-09-23 21:26_

I'd probably lean toward leaving this in the same file as before, unless there's a specific reason to move it out.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI021_1.pyi`:2 on 2025-09-23 21:30_

This docstring seems slightly misleading since the snapshot shows it trying to fix both diagnostics 😄 

In testing this locally in the CLI, the net effect is that we still emit both diagnostics when checking, but the isolation level means only the first diagnostic actually gets fixed. Since we sort by diagnostic range, that means PYI021 gets fixed and PIE790 does not. You don't need to write all of that in the docstring, to be clear, just recording my observations!

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/suite.rs`:13 on 2025-09-23 22:00_

One thing I didn't realize about moving this here is that it now gets called for non-function (or -class or -module) suites, such as context managers or `if` statements:

```py
with foo():
    """docstring"""
    pass

if True:
    """docstring"""
    pass
```

I tested these locally, and we flag both of these. Fortunately, it looks like we already track a [`docstring_state`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/checkers/ast/mod.rs#L235) field on `Checker`. We'd have to add a `pub(crate)` getter function for this and make the `DocstringState` and `ExpectedDocstringKind` enums `pub(crate)` too, but I think we can use that to check that we're in an actual docstring.

There's an existing [`SemanticModel::in_pep_257_docstring`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_semantic/src/model.rs#L1997) method, but unfortunately that flag only gets set on the `SemanticModel` once we're actually visiting the docstring.

---

_@ntBre reviewed on 2025-09-23 22:06_

Thank you! This looks great overall, I just had a couple of nits and one slightly larger issue with the `suite` handling. But even the fix for that shouldn't be too bad.

---

_@second-ed reviewed on 2025-09-24 20:16_

---

_Review comment by @second-ed on `crates/ruff_linter/src/checkers/ast/analyze/suite.rs`:13 on 2025-09-24 20:16_

Hey thanks for this, I think I've addressed all of the comments from the review, please let me know if I've missed anything!

---

_@ntBre approved on 2025-09-24 21:22_

Thank you, this is great!

I pushed one small commit fixing a few nits:
* revert changes to `unnecessary_placeholder.rs`
* `current_docstring_state` -> `docstring_state`
* exclude `ExpectedDocstringKind::Attribute`

The last one was probably the biggest, but I also think it wasn't possible to reach based on how we're calling `analyze::suite`. It just seemed better to be explicit in case that changed in the future.

---

_Renamed from "[`flake8-pyi`] add isolation level for interaction between rules #19982 (PYI021)" to "[`flake8-pyi`] Avoid syntax error from conflict with `PIE790` (`PYI021`)" by @ntBre on 2025-09-24 21:24_

---

_Merged by @ntBre on 2025-09-24 21:27_

---

_Closed by @ntBre on 2025-09-24 21:27_

---

_Comment by @second-ed on 2025-09-24 21:52_

Awesome, nice one - I didn't realise that we didn't have to worry about docstring_state clashing being both an attribute and a method of the struct that's good to know. Yeah checking explicitly for the expected docstring kind makes sense too, I'll have a closer read of the changes you made to see if there are any tricks to steal! 

Cheers for your help and guidance on this PR btw, I'll keep an eye out for any others that I think are my level of difficulty!

---
