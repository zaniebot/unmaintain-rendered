```yaml
number: 20733
title: "[`pylint`] Implement `stop-iteration-return` (`PLR1708`)"
type: pull_request
state: merged
author: fatelei
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: stop_iteration_return
created_at: 2025-10-07T04:42:47Z
updated_at: 2025-10-23T22:02:42Z
url: https://github.com/astral-sh/ruff/pull/20733
synced_at: 2026-01-10T17:34:34Z
```

# [`pylint`] Implement `stop-iteration-return` (`PLR1708`)

---

_Pull request opened by @fatelei on 2025-10-07 04:42_

## Summary

implement pylint rule stop-iteration-return / R1708

## Test Plan

<!-- How was it tested? -->


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:15 on 2025-10-15 19:31_

I tried this out on Python 3.7, which is the earliest version we support, and it raises a runtime error even then, so I think we should try to emphasize that a  bit more than just saying it was deprecated and will be removed. 

I also don't think it matters if the exception has a value, at least on 3.13:

```pycon
Python 3.13.7 (main, Aug 15 2025, 12:34:02) [GCC 15.2.1 20250813] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> def my_generator():
...     yield 1
...     yield 2
...     raise StopIteration
...
>>> for _ in my_generator(): ...
...
Ellipsis
Ellipsis
Traceback (most recent call last):
  File "<python-input-0>", line 4, in my_generator
    raise StopIteration
StopIteration

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    for _ in my_generator(): ...
             ~~~~~~~~~~~~^^
RuntimeError: generator raised StopIteration
>>>
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:49 on 2025-10-15 19:33_

The `use return instead` part might be nice as the `fix_title`. I believe it will display even if the rule doesn't actually have a fix.


```suggestion
    fn message(&self) -> String {
        "Explicit `raise StopIteration` in generator".to_string()
    }
    
    fn fix_title(&self) -> Option<String> {
        "Use `return` instead".to_string()
    }
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:287 on 2025-10-15 19:35_

New rules have to be added in `RuleGroup::Preview`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:68 on 2025-10-15 19:36_

I could be wrong, but I'm pretty sure this scope kind is for when you're inside of a generator expression, not inside the definition of a generator function. I'm not seeing any new snapshot files to confirm that, though. You'll need to run the tests locally and accept any new snapshots for the new tests you added to work.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:78 on 2025-10-15 19:39_

I think we'll need to use [`has_builtin_binding`](https://github.com/astral-sh/ruff/blob/d2e8d765705956694ac7bb23f4de179352b3c3ed/crates/ruff_python_semantic/src/model.rs#L273) here. That will make sure `StopIteration` hasn't been shadowed or anything like that.

---

_@ntBre requested changes on 2025-10-15 19:45_

Thank you! This looks good to me overall, I just had a few fairly small suggestions besides needing to add the test snapshots. Let me know if you need  help with that. It might also be good for @amyreese to take a quick look.

I think this is a good rule to add, especially since this pattern will always (?) cause a runtime error on the versions of Python we support.

---

_Label `rule` added by @ntBre on 2025-10-15 19:45_

---

_Label `preview` added by @ntBre on 2025-10-15 19:45_

---

_Review requested from @carljm by @fatelei on 2025-10-16 02:10_

---

_Review requested from @MichaReiser by @fatelei on 2025-10-16 02:10_

---

_Review requested from @sharkdp by @fatelei on 2025-10-16 02:10_

---

_Review requested from @dcreager by @fatelei on 2025-10-16 02:10_

---

_Comment by @github-actions[bot] on 2025-10-16 02:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:91 on 2025-10-20 22:26_

Since this is likely the more "expensive" portion of the check (iterating over every statement in the parent function's body), I'd suggest extracting this logic into a function, and call that function from inside the matching branches below, so that we only check for a generator function context if the statement actually matches `raise StopIteration`.

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:122 on 2025-10-20 22:40_

I think this nested if block could actually be replaced with:

```rust
if checker.semantic().match_builtin_expr(&func, "StopIteration")
```

See the `blocking-input` rule for an example: https://github.com/astral-sh/ruff/blob/511710e1ef930050d6a58d73251ddf85558ecc93/crates/ruff_linter/src/rules/flake8_async/rules/blocking_input.rs#L46

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:133 on 2025-10-20 22:40_

Same for `match_builtin_expr`

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:108 on 2025-10-20 23:00_

Since the two arms of this if-else share their internal logic, it might be possible to combine them into a match statement to reduce duplication and nesting. Something like:

```rust
match &raise_stmt.exc.as_ref() {
  Some(ast::Expr::Call(...) | ast::Expr::Name(...)) => {
    ...
  },
  _ => {}
}
```

---

_@amyreese requested changes on 2025-10-20 23:01_

---

_Review comment by @ntBre on `crates/ty/docs/environment.md`:45 on 2025-10-22 21:23_

I think we should revert changes to this file, unless this is just a GitHub rendering bug.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:16 on 2025-10-22 21:28_

The first part here feels a bit redundant, and then I think the fact that it raises a runtime error is a bit more severe than breaking an abstraction. I think we could just say something like:


```suggestion
/// Raising `StopIteration` in a generator function causes a `RuntimeError`
/// when the generator is iterated over.
```

---

_@ntBre reviewed on 2025-10-22 21:31_

Thanks, this is looking good! I noticed that we have a related rule in [return-in-generator (B901)](https://docs.astral.sh/ruff/rules/return-in-generator/#return-in-generator-b901), and instead of triggering on `return` statements, it actually runs on function definition statements. Then its visitor looks for both yields and `return` statements. I think it might make sense to mirror that structure here so that we don't have to traverse multiple scopes looking for an enclosing function. B901 can even avoid visiting nested functions since those will be traversed later by the `Checker`:

https://github.com/astral-sh/ruff/blob/766ed5b5f394b3dae19dba4859aac797b48fbd00/crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs#L123-L125

---

_@amyreese reviewed on 2025-10-22 22:02_

---

_Review comment by @amyreese on `crates/ty/docs/environment.md`:45 on 2025-10-22 22:02_

I think this is github due to the branch being based on a two-week old rev. When I did a local rebase, it didn't show in the results.

---

_@amyreese reviewed on 2025-10-22 22:03_

---

_Review comment by @amyreese on `crates/ruff_linter/resources/test/fixtures/pylint/stop_iteration_return.py`:71 on 2025-10-22 22:03_

There's also some extra indentation and whitespace in here. I suggest running `ruff format` on this fixture and then re-running the tests and accept the snapshot changes.

---

_@ntBre approved on 2025-10-23 21:34_

Thank you! This looks good to me now if @amyreese is happy with it.

I pushed two commits fixing the merge conflicts from one of my PRs earlier and fixing a small nit about the function order.

---

_Renamed from "feat: implement pylint rule stop-iteration-return / R1708" to "[`pylint`] Implement `stop-iteration-return` (`PLR1708`)" by @ntBre on 2025-10-23 21:35_

---

_@amyreese approved on 2025-10-23 22:02_

---

_Merged by @amyreese on 2025-10-23 22:02_

---

_Closed by @amyreese on 2025-10-23 22:02_

---
