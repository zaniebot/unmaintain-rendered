```yaml
number: 17281
title: "Add Rule to check for singular `yield` in `@contextlib.{async}contextmanager` decorated functions (RUF062)"
type: pull_request
state: open
author: maxmynter
labels:
  - rule
assignees: []
base: main
head: ctxmg-yield-once
created_at: 2025-04-07T16:43:43Z
updated_at: 2025-10-19T18:55:39Z
url: https://github.com/astral-sh/ruff/pull/17281
synced_at: 2026-01-12T15:56:01Z
```

# Add Rule to check for singular `yield` in `@contextlib.{async}contextmanager` decorated functions (RUF062)

---

_@maxmynter_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Closes #16949 

## Summary
Add a rule that checks that only a single `yield` statement is in `@contextlib.contextmanager` or `@contextlib.asynccontextmanager` decorated functions. 

Later `yield` statements are executed in the cleanup stage of the contextmanager and result in a `RuntimeError`.

See [docs](https://docs.python.org/3/library/contextlib.html#contextlib.contextmanager)

### Notes
- I reused the `function_def_visit_preorder_except_body` from the `unused_async.rs` rule and moved it to `helpers.rs` alongside `class_def_visit_preorder_except_body`. While I don't use the latter, I moved both to maintain code locality.



<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Added a testing fixture and `cargo insta` snapshot.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-04-07 16:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @maxmynter on 2025-04-07 17:15_

I've looked through these ecosystem changes.
They all come from branches in the control flow like this one:

```python
@contextmanager
def context():
   print("setup")
   if condition:
       print("condition")
       yield
   else:
       print("not condition")
       yield
    print("teardown")
```

The current implementation does not check for this; it should. I'm working on it. 



---

_Converted to draft by @maxmynter on 2025-04-07 17:15_

---

_Comment by @maxmynter on 2025-04-09 22:04_

Still work in progress. Currently handling of `try-except-else` blocks is wrong and I also want to extend the test for deeper nested cases.

---

_Comment by @maxmynter on 2025-04-10 16:00_

RUF060 traverses source-ordered AST nodes while maintaining a stack tracking yield counts across execution paths:
- Overrides visit_stmt to handle optional control flow blocks (if/elif/else, try/except/else/finally, for/while/else)
- For branching structures: takes maximum yield count across exclusive branches to proceed
- For try/except/else/finally the except(s)/else are mutually exclusive
- For loops: flags any yield in loop body as problematic (likely multiple yields, we only know at runtime iirc), optional else clause
- Match/Case does not need special handling since >= one case block are mandatory

Using two testing fixtures; one for failing and one for passing cases.

---

_Marked ready for review by @maxmynter on 2025-04-10 16:00_

---

_Comment by @maxmynter on 2025-04-14 13:11_

Can we run the ecosystem tests again? 

I try running them locally but get the same issues as #10436 (`error: invalid value 'RUF9' for '--ignore <RULE_CODE>'` in all repo's). It doesn't seem related to my changes and I also get the failure on the commit that passed in CI.
If you have any pointers how to fix that, I would greatly appreciate.

Otherwise this is ready for review.

cc: @ntBre (since you reacted to my previous comment above). 

---

_Comment by @ntBre on 2025-04-14 15:15_

The `RUF9` rules are only available in test builds, so you have to build in the test or maybe dev profile. I've run into this a bit myself and don't remember exactly the command I ran to avoid it :sweat_smile: 

The ecosystem checks should rerun automatically and update the comment. Are you seeing stale results? You could try pushing an empty commit to retrigger it, but CI might also check if relevant files changed, I'd have to double check.

I'll try to give this a review in the next day or two!

---

_Assigned to @ntBre by @ntBre on 2025-04-14 15:15_

---

_Comment by @ntBre on 2025-04-14 19:39_

I think you've picked a really tough rule to implement because it requires such a careful analysis of control flow. I added a minimized version of the first ecosystem hit as a test case:

```python
@contextlib.contextmanager
def foo():
    if False:
        yield 1
        return
    yield 2
```

and it is a real false positive.

I think this rule's implementation might be simplified by the control flow graph work being tracked in #17065, so I'd probably recommend holding off on this for now.

Thanks for your work here and apologies for not noticing this or commenting on the issue earlier!

For now, the main other thing you could do is just go through the ecosystem results and see if there are any other interesting test cases. The two I randomly picked also had this early return form, so they may make up a majority of false positives.

---

_Comment by @maxmynter on 2025-04-16 08:57_

I went through the ecosystem changes. Most are the return case you describe. Sometimes a little more complex: 

```python
try:
    yield
finally:
    return
```
So an implementation that tracks returns over control flow branches would also need to consider cases like this one where the return does not follow in the same block.


There are [two](https://github.com/ibis-project/ibis/blob/21e47ac0b39dc07122b67a95f0e92abd162a746f/ibis/backends/athena/__init__.py#L286) [other](https://github.com/zulip/zulip/blob/440864e7c92511f1181899133d5cc87b4988dfdc/zerver/lib/context_managers.py#L36) `try`-`except` blocks like this:

```python
try:
    yield 1
except:
    yield 2
```

which in principle is a true positive. If the `try` fails after the `yield`, there would be a second one in the `except` block.

However, I think it's a reasonable assumption that in the context of an `@contextmanager` the error prone code is prior to the `yield`. 
Then we can assume that the control flow either passes the `yield`s in `try`-`else`-`finally` or `try`-`except`-`finally`.




-----
As for the ecosystem changes. No they are not stale for me. I think i got sidetracked by this `RUF9` error and not being able to execute locally.

---

_Comment by @ntBre on 2025-04-16 13:08_

Thanks again for working on this! I just want to make sure you saw this part of my comment above since I saw some new commits:

> I think this rule's implementation might be simplified by the control flow graph work being tracked in https://github.com/astral-sh/ruff/issues/17065, so **I'd probably recommend holding off on this for now**.

You're certainly welcome to keep working on it, but I think it will be easier to implement and to review once we have more general control-flow graph support.

cc @dylwil3 to make sure I'm not totally off-base with what the CFG work will give us

---

_Comment by @dylwil3 on 2025-04-16 13:20_

Yep that's the hope! However the initial version will probably have simplified handling for `try` statements (since those are quite the quagmire); I also haven't actually thought much about `yield` statements so it's possible I haven't modeled them correctly 😅 

---

_Comment by @ntBre on 2025-04-16 13:35_

Thanks Dylan!

I'd still lean towards waiting on the separate CFG work, and then the rule will grow capabilities as the CFG infrastructure improves, but I'm open to other thoughts too. The extra unit tests from the ecosystem check will still be very helpful for knowing where we stand throughout the process.

---

_Comment by @maxmynter on 2025-04-16 14:15_

Thanks @ntBre, I saw your note but i've learned quite a bit in implementing this. That's why I went with [bias toward action](https://astral-sh.notion.site/Astral-s-Values-0ed6a642bcc84e91af6836b2373572f5) knowing I'm happier with a working implementation that's idle for roadmap reasons, than to abandon this half finished.
I also think it would be cool to have a substantial rule contributed to ruff  🤩 (i made `RUF102` last week, but it's rather simple).

In the latest commit's i've changed to the assumption that any failures in a `try` block happen before a `yield`. 
This is also my recommentation. I think it's a sensible hypothesis in the context of `@contextmanager` and otherwise we would either just accumulate over branches (with many false positives) or have special cases for pre/post `yield` errors which let's complexity of the implementation explode.

It seems that this adresses the ecosystem change issues.

So if you are up for it I would love to try to get this over the finish line. Of course, I understand if you have too much on your plate or just want to wait. That's a risk I took willingly.

In that case, @dylwil3, What is the timeline for #17065? 
I'm full time on OSS contributions for another three weeks and if it's done in time, I would love another jab at this one using the CFG. Also let me know if I can help with this or in any other capacity!

---

_Comment by @dylwil3 on 2025-04-16 16:06_

The goal is to have the stacked PRs ready to review by Monday - but it's hard to predict with confidence the timeline after that since it depends on the results of code review. Optimistically, a reasonable implementation will be merged in by the following Monday which you can then play with here.

> Also let me know if I can help with this or in any other capacity!

Absolutely! At the moment there's a few parts of the design that I'd like to stabilize, but then I'd be happy to help divvy up the work - thanks for offering!

---

_Comment by @maxmynter on 2025-04-16 21:21_

Awesome! Looking forward! 

I'll mark this as draft until then and this probably doesn't need a review before the CFG is ready (unless of course someone is bored; in that case i'd appreciate any pointers to improve my Rust).

---

_Converted to draft by @maxmynter on 2025-04-16 21:22_

---

_Comment by @maxmynter on 2025-05-10 04:29_

Note: The merge conflicts are with #16480 which by now has been merged as `RUF060`.
A renaming here should do. I'm Happy to take care once we decide to move forward here 

I would love to get this one over the finish line sometime and given extensive test cases and the ecosystem checks, I'm also pretty confident. The actual implementation is only 350ish lines. Does it still make sense to wait on the CFG work, @ntBre?

---

_Comment by @dylwil3 on 2025-05-11 16:22_

@maxmynter as these things go sometimes, CFG work has been slowed in favor of some higher priority tasks. My apologies! I'm personally okay with moving forward with the rule as it currently exists as long as we feel confident about getting no/rare false positives (this is preferred even if it means having more false negatives). 

---

_Label `rule` added by @ntBre on 2025-05-12 12:56_

---

_Comment by @ntBre on 2025-05-12 12:58_

Sure, I can try to review it at some point if you fix the conflicts.

---

_Comment by @ntBre on 2025-05-12 18:39_

Just as a note on the rule code, you may want to jump to RUF062. https://github.com/astral-sh/ruff/pull/17368 is currently using RUF061 too.

---

_Comment by @maxmynter on 2025-05-13 03:58_

Awesome, I will tidy up here over the next few days and reopen and request a review once I'm there. 

@dylwil3 I hope I didn't come off as pushy and if so, apologies. I guess, I'm just excited about this contribution and definitely understand that you all have other priorities as weil!  

---

_Marked ready for review by @maxmynter on 2025-05-13 22:25_

---

_Review requested from @ntBre by @MichaReiser on 2025-05-26 09:35_

---

_Review requested from @dylwil3 by @MichaReiser on 2025-05-26 09:35_

---

_Comment by @ntBre on 2025-06-06 13:04_

I did a quick merge with some changes I made recently, hope you don't mind. I'm preparing to review this today!

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF062.py`:93 on 2025-06-06 13:29_

Is this the same as case 3 above?

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF062.py`:138 on 2025-06-06 13:32_

It looks like this one might be missing an `else` based on the name and comment

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF062.py`:154 on 2025-06-06 13:33_

Ah maybe the body of this one is swapped with `while_loop_with_else`?

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF062.py`:184 on 2025-06-06 13:46_

This part kind of confuses me. We're not inspecting the actual body of `is_true` right? It's just a placeholder for a boolean function? I might just delete the inner function here if that's the case, it's fine for it to be undefined.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF062_1.py`:46 on 2025-06-06 13:51_

The one case where this might be valid is if the `yield` is followed by an unconditional `break` or `return`. Does the rule handle that case? `return` seemed well handled in the valid cases, but I don't remember seeing `break`.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF062.py`:319 on 2025-06-06 13:58_

So are these cases *not* valid anymore? I came back up here to ask after reading the invalid cases below, which look very similar. And now I'm seeing the `# RUF060` comments here which indicate to me that these cause errors.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/mod.rs`:103 on 2025-06-06 14:00_

nit: if the second test file is _1 I'd usually make the first one _0.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:214 on 2025-06-06 14:07_

very minor nit: I was curious to see what `visit_preorder` looked like, but I think it doesn't exist anymore. I know you just carried this comment over from the other file, but if you're curious, it would be interesting to me to know what `visit_preorder` is called now (and if we could possibly use it now).

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:37 on 2025-06-06 14:19_

Is it worth tracking an enum or bool saying whether the context manager was async? I think this message will be slightly incorrect for `asynccontextmanager`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:54 on 2025-06-06 14:29_

Is this condition ever transiently `true` while traversing and then reset back to `false`? I'm wondering because an alternative approach here would be to store a `&Checker` in the `YieldPathTracker` and call `report_diagnostic` as soon as we see an extra yield. I think this would have two main benefits:

1. We can emit multiple diagnostics if there are more than two `yield`s
2. We can capture a more helpful diagnostic range from the problematic `yield` statement

And a third point closely related to these is that we're currently refactoring our diagnostic infrastructure in a way that should allow us to attach multiple spans/pieces of info to a diagnostic soon. We could potentially emit a multi-part diagnostic here with pieces like:

- This `yield` in
- This context manager is an error because of
- This earlier `yield` in the same path

each with their own range/code snippet, which would be pretty cool. We can't do that now, but I feel like the `&Checker` approach would make that change easy in the future.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:72 on 2025-06-06 15:38_

nit: it seems like this is a visitor, so I'd probably name it `YieldPathVisitor`.

I also think this type could use some docs explaining the meaning of the stacks and how they are modified while traversing the AST. I'm writing this before reading much of the code below, but I'm also wondering if these stacks are manipulated independently or if it might make more sense to have a single stack of something like `Scope`s, where each scope has its own `yields` and `returns`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:80 on 2025-06-06 15:40_

Do we need to `pop` here or  could you just inspect the value with `last`? That doesn't seem quite right for the `None` case where you push 0, but that's also slightly surprising to me.

---

_@ntBre reviewed on 2025-06-06 15:51_

Thanks. I haven't really wrapped my head fully around the stack stuff, but I have a few suggestions that might make things more clear.

---

_@maxmynter reviewed on 2025-06-12 07:52_

---

_Review comment by @maxmynter on `crates/ruff_linter/resources/test/fixtures/ruff/RUF062.py`:93 on 2025-06-12 07:52_

Yes, thanks for catching this.

---

_@maxmynter reviewed on 2025-06-12 07:56_

---

_Review comment by @maxmynter on `crates/ruff_linter/resources/test/fixtures/ruff/RUF062.py`:184 on 2025-06-12 07:56_

Agree. Back when I was writing these, I wasn't too deep in Ruff's testing setup and got intimidated when the linting of the fixture began screaming at me! Removed these

---

_@maxmynter reviewed on 2025-06-12 07:59_

---

_Review comment by @maxmynter on `crates/ruff_linter/resources/test/fixtures/ruff/RUF062_1.py`:46 on 2025-06-12 07:59_

It's not yet. I will add handling and tests for `break`.

---

_Review comment by @maxmynter on `crates/ruff_linter/resources/test/fixtures/ruff/RUF062.py`:319 on 2025-06-12 08:12_

The heuristic of this rule assumes that an error in a `try/except` occurs before the yield statement. Thus it's assumes the data flow and thus yield accumulation to either follow `try->else->finally` or `except->finally`.

Therefore this is valid but those two aren't: 

```python
# (15) Invalid: Unguarded yield in finally
@contextlib.contextmanager
def yield_in_finally():
    try:
        yield
    except:
        pass
    else:
        return
    finally:
        yield

# (16) Invalid: No returning except accumulates yields
@contextlib.contextmanager
def yield_in_no_return_except():
    try:
        pass
    except:
        yield
    else:
        yield
        return
    yield
``` 

I agree about the comments though. They are stale. An earlier version of the rule was also accumulating `yields` in the full `try->except->else->finally` path. But this had changes in the ecosystem tests for cases like these: 
```python
try:
     some_flaky_setup()
     yield "success"
except CommonFlakinessError:
    yield "alternative"
```
Therefore I moved this from invalid to valid cases. The same is true for the 3 succeeding test cases. I removed all the stale `#RUF060` comments there too.

I think in the context of a contextmanager it makes sense, that the error prone code preceds the `yield` and we only count `yield`s in the abovementioned path.



---

_@maxmynter reviewed on 2025-06-12 08:12_

---

_Comment by @maxmynter on 2025-06-12 08:53_

Test todos:
- [x] Review Comments on tests
- [x] Rename fixture `RUF062.py` -> `RUF062_0.py` 
- [x] Update Snapshots

Rule Logic Todos
- [x] Add tests for break
- [ ] Implement handling for `break`
- [x] Accumulate violating expressions for individual diagnostic
- [x] Refactor `.pop()` use
- [x] Rename `YieldPathTracker`
- [x] Move stacks into `struct Scope` 
- [x] Fixup `merge_continuing_branch` borrowing workaround (line 108)


# Questions
## Ok to only report along max yield path?
Currently, we accumulate `yields` greedily. Therefore we can only report diagnostics for yields along this path. Eg. here 
```python
# (12) Invalid: Multiple yields in complex try/except/else/finally
@contextlib.contextmanager
def complex_try_except_else_finally(condition):
    try:
        if condition:
            yield "in try if"
        else:
            yield "in try else"
    except ValueError:
        yield "in ValueError"  # Unreported RUF062; only report on max yield path
    except TypeError:
        yield "in TypeError"  # Unreported RUF062; only report on max yield path
    else:
        yield "in else"  # RUF062
    finally:
        yield "in finally"  # RUF062
```
we only report along the try-else-finally pathway. Is that fine or should we accumulate all? 
Computationally this may become more complex though as we multiply the number of scopes at every branch instead of only taking the maximum path.

In the above example we would need to track along the `try -> ValueError -> finally`, `try -> TypeError -> finally`, and `try -> else -> finally` pathways. Without it, not all offending violations are marked. However, fixing iteratively will eventually adress all violations.

## Don't report on yield followed by break in loop?
Implementing this is quite involved because we would need to recursively check the body of the loop and the contained expressions while keeping track of their order. This breaks the symmetry on yield check. Thus,  we  would need to check for being nested in a loop everywhere and . if we are, check for a succeeding `break`. This also needs us to track the order in which we encounter the `yield` / `break`s.

Given we found no loops in the ecosystem checks (and `yield` out of a loop in a `contextmanager` decorated function is unorthodox, they are only allowed to yield once), I would suggest one of the following:
- Keep the heuristic and report yields in loops, users can `# noqa` if they need to,
- Drop the heuristic and ignore the fact that loops are repeatedly executed. We would just check the enclosed control flow like we do with e.g. `if`/`else` bodies.

I would lean toward two because false positives are annoying. But i'm hesitant for the complexity added by adressing yield given that we did not see it occur anywhere.

If we really want to include the loop check, I would like that in a separate PR because this one is big already.

Let me know what you think.

---

_Renamed from "Add Rule to check for singular `yield` in `@contextlib.{async}contextmanager` decorated functions (RUF060)" to "Add Rule to check for singular `yield` in `@contextlib.{async}contextmanager` decorated functions (RUF062)" by @maxmynter on 2025-06-12 08:55_

---

_@maxmynter reviewed on 2025-06-12 09:09_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:214 on 2025-06-12 09:09_

It was changed here 

```bash
commit 050f33277163db62449d11e9cc84137be26661d4 refs/remotes/upstream/dhruv/callable-disjoint
Author: Micha Reiser <micha@reiser.io>
Date:   Fri Mar 28 20:40:26 2025 +0100

    Rename `visit_preorder` to `visit_source_order` (#17046)

    ## Summary

    We renamed the `PreorderVisitor` to `SourceOrderVisitor` a long time ago
    but it seems that we missed to rename the `visit_preorder` functions to
    `visit_source_order`.
    This PR renames `visit_preorder` to `visit_source_order`
```
with this being the (generated) code: 
https://github.com/astral-sh/ruff/blob/e6fe2af292e7be87c2917863464a5455c5b22c38/crates/ruff_python_ast/src/generated.rs#L7288-L7321

I updated the name in [42828fb](https://github.com/astral-sh/ruff/pull/17281/commits/42828fb5ff48b1014ed27fef06442aab046c151b).

---

_@maxmynter reviewed on 2025-06-12 10:18_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:37 on 2025-06-12 10:18_

Adressed in [7ae6e79](https://github.com/astral-sh/ruff/pull/17281/commits/7ae6e798247d070f8513878e84fb51c35b2467f8)

---

_@maxmynter reviewed on 2025-06-12 10:22_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:54 on 2025-06-12 10:22_

No. This is never transiently true, so this is a great idea!

---

_@maxmynter reviewed on 2025-06-12 15:24_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:54 on 2025-06-12 15:24_

Adressed in [5451c18](https://github.com/astral-sh/ruff/pull/17281/commits/5451c18fb937365b9295ecc522042bf2109b0b4c)

We need a HashMap to track offending expressions as every expression can be part of multiple paths which may or may not be violating the rule. Thus they may be added repeatedly.

I now use the expressions to add the diagnostic at the specific `yield` instead of the full function.

---

_@maxmynter reviewed on 2025-06-13 14:03_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:72 on 2025-06-13 14:03_

Good idea -- makes for a natural abstraction and the code easier to understand!

---

_@maxmynter reviewed on 2025-06-13 14:30_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:80 on 2025-06-13 14:30_

`last` would have been enough but it's irrelevant now with the refactor.

I agree the debug assert and pushing is awkward. I added it to avoid a runtime `panic!` but if we attempt to pop from an empty stack, there is always a logical error. With an extensive test suite I think `.expect()` is nicer.

---

_Review requested from @ntBre by @maxmynter on 2025-06-13 17:39_

---

_Comment by @ntBre on 2025-07-10 13:15_

(I'm finally taking a look at this today, thanks again for your work here!)

To your questions, I think it _would_ be nice to report all of the violations, if it's not too much more programming work. I think we generally like to avoid situations where fixing one issue "creates" a second one that we didn't report initially. I don't think this is a blocker, though.

And I think I agree with you about not worrying about `break`. I had just reviewed a rule where `break` and `return` were also being used and the test cases here brought it to mind, but that would seem like a strange pattern in a context manager, as you said.

I'll take another look at the rest of the code now!

---

_@ntBre reviewed on 2025-07-10 13:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:214 on 2025-07-10 13:17_

Looks like everything is updated except the comment 😆 Thanks for finding the commit!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:14 on 2025-07-10 13:22_

Should we mention `asynccontextmanager` here?


```suggestion
/// Checks that a function decorated with `contextlib.contextmanager` or 
/// `contextlib.asynccontextmanager` yields at most once.
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:21 on 2025-07-10 13:23_

Hmm, we usually have a `use instead:` part of the example, but I guess it's not really clear what the user meant. I guess we could just drop the second yield? Maybe this is fine as-is.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:80 on 2025-07-10 13:26_

Oh, when I saw `String` in the `Violation` struct, I assumed you were slicing out the actual contents from the file. For this case, we could just use a `&'static str` and avoid allocating. (Or you could try the slicing approach, so if a user imports the decorator directly the error message would reflect that)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:64 on 2025-07-10 13:56_

I tried this patch locally to embed the `Checker` in the visitor to emit the errors as we encounter them. I'm not sure it's really much better because we do still need a hash set to avoid duplicate diagnostics. Just an idea if it appeals to you. Possibly it would help with emitting all diagnostics in a branch?

<details><summary>Patch</summary>

```diff
diff --git a/crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs b/crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs
index fc2dfe6667..ae44981249 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs
@@ -4,7 +4,7 @@ use ruff_python_ast::AnyNodeRef;
 use ruff_python_ast::helpers::map_callable;
 use ruff_python_ast::{self as ast, visitor::source_order};
 use ruff_text_size::{Ranged, TextRange};
-use rustc_hash::FxHashMap;
+use rustc_hash::FxHashSet;
 
 use crate::checkers::ast::Checker;
 use crate::rules::ruff::rules::helpers::function_def_visit_sourceorder_except_body;
@@ -54,14 +54,8 @@ impl Violation for MultipleYieldsInContextManager {
 /// RUF062
 pub(crate) fn multiple_yields_in_contextmanager(checker: &Checker, function_def: &StmtFunctionDef) {
     if let Some(context_manager_name) = get_contextmanager_decorator(function_def, checker) {
-        let mut path_tracker = YieldPathVisitor::new();
+        let mut path_tracker = YieldPathVisitor::new(checker, context_manager_name);
         source_order::walk_body(&mut path_tracker, &function_def.body);
-        for expr in path_tracker.into_violations() {
-            checker.report_diagnostic(
-                MultipleYieldsInContextManager::new(context_manager_name.clone()),
-                expr.range(),
-            );
-        }
     }
 }
 
@@ -93,16 +87,20 @@ fn get_contextmanager_decorator(
 // Within a scope we evaluate all control flow paths and propagate the yields along the
 // maximum path to the outer scope.
 // Return exits the contextmanager decorated function and we stop accumulating yields along that path.
-struct YieldPathVisitor<'a> {
+struct YieldPathVisitor<'a, 'b> {
     scopes: Vec<Scope<'a>>,
-    violations: FxHashMap<TextRange, &'a Expr>,
+    checker: &'a Checker<'b>,
+    name: String,
+    seen: FxHashSet<TextRange>,
 }
 
-impl<'a> YieldPathVisitor<'a> {
-    fn new() -> Self {
+impl<'a, 'b> YieldPathVisitor<'a, 'b> {
+    fn new(checker: &'a Checker<'b>, name: String) -> Self {
         Self {
             scopes: vec![Scope::new()],
-            violations: FxHashMap::default(),
+            checker,
+            name,
+            seen: FxHashSet::default(),
         }
     }
 
@@ -146,7 +144,7 @@ impl<'a> YieldPathVisitor<'a> {
     fn report_multiple_yield_violations(&mut self, yields: &[&'a Expr]) {
         // Only report the second to last violations
         for &yield_expr in yields.iter().skip(1) {
-            self.violations.insert(yield_expr.range(), yield_expr);
+            self.report_single_yield_violation(yield_expr);
         }
     }
 
@@ -159,11 +157,13 @@ impl<'a> YieldPathVisitor<'a> {
     }
 
     fn report_single_yield_violation(&mut self, yield_expr: &'a Expr) {
-        self.violations.insert(yield_expr.range(), yield_expr);
-    }
-
-    fn into_violations(self) -> impl Iterator<Item = &'a Expr> {
-        self.violations.into_values()
+        let range = yield_expr.range();
+        if self.seen.insert(range) {
+            self.checker.report_diagnostic(
+                MultipleYieldsInContextManager::new(self.name.clone()),
+                range,
+            );
+        }
     }
 
     // For exclusive branches (if/elif/else, match cases, ...) - propagate the maximum
@@ -226,7 +226,7 @@ impl<'a> Scope<'a> {
     }
 }
 
-impl<'a> source_order::SourceOrderVisitor<'a> for YieldPathVisitor<'a> {
+impl<'a> source_order::SourceOrderVisitor<'a> for YieldPathVisitor<'a, '_> {
     fn enter_node(&mut self, node: AnyNodeRef<'a>) -> source_order::TraversalSignal {
         match node {
             AnyNodeRef::StmtFor(_)
@@ -405,7 +405,7 @@ impl<'a> source_order::SourceOrderVisitor<'a> for YieldPathVisitor<'a> {
                 if let Some(scope) = self.scopes.last_mut() {
                     scope.add_yield(expr);
                     if scope.does_yield_more_than_once() {
-                        self.violations.insert(expr.range(), expr);
+                        self.report_single_yield_violation(expr);
                     }
                 }
             }
```

</details>

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:112 on 2025-07-10 14:09_

This check pops up in a lot of places, which doesn't feel quite right to me. I think a nicer API, if it's possible, would be to have `add_yield` perform this check. That way you don't have to worry about this in so many places, you just call `add_yield` whenever you encounter one, and a diagnostic is emitted if appropriate.

---

_@ntBre reviewed on 2025-07-10 14:17_

I think the tests look good, but I'm hoping we can still simplify the code a bit more.

---

_Comment by @maxmynter on 2025-07-14 15:39_

Thanks @ntBre for your review. I'm somewhat occupied the next two weeks but will continue here afterwards!

Edit (Aug 11th): Will get back to this -- sorry, lots of life stuff right now. 

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:14 on 2025-08-21 11:44_

Adressed in [7781d14](https://github.com/astral-sh/ruff/pull/17281/commits/7781d140633b62716cd06cc5795e1287b74bd7c1)

---

_@maxmynter reviewed on 2025-08-21 11:44_

---

_@maxmynter reviewed on 2025-08-21 11:45_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:21 on 2025-08-21 11:45_

I made it more explicit in [7781d14](https://github.com/astral-sh/ruff/pull/17281/commits/7781d140633b62716cd06cc5795e1287b74bd7c1). 

I feel it may be a bit redundant, but since it doesn't add a lot of text, I feel its better to be really explicit and have the working example, too.

---

_@maxmynter reviewed on 2025-08-21 11:55_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:80 on 2025-08-21 11:55_

Adressed in [e95fd25](https://github.com/astral-sh/ruff/pull/17281/commits/e95fd25ab6b558f1112d8968e4db05e7faad95ff)

---

_@maxmynter reviewed on 2025-08-21 15:46_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:64 on 2025-08-21 15:46_

Thanks for your suggestion. I've incorporated parts of it in [395e8ad](https://github.com/astral-sh/ruff/pull/17281/commits/395e8ad8139113570a893447761eb31d042d0d14) but tried to keep the concerns of the visitor and caller separate. 

---

_@maxmynter reviewed on 2025-08-21 15:51_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:112 on 2025-08-21 15:51_

Adressed in [395e8ad](https://github.com/astral-sh/ruff/pull/17281/commits/395e8ad8139113570a893447761eb31d042d0d14) and [21a92aa](https://github.com/astral-sh/ruff/pull/17281/commits/21a92aacda77d096e8cd928e38d11abf8eb695a7)

---

_Comment by @maxmynter on 2025-08-21 19:02_

Finally got around to do some more work here! Thanks (again) for your review @ntBre. 

I've 
- Resolved merge conflicts. Just renaming of methods.
- Broken up the Try-except-else-finally logic into smaller functions to make it easier to reason about
- use a collection-emission pattern for the yield counting.
- Improved naming quite a bit
- Added a couple more brief comments
- Removed a couple unnecessary allocations
- Extracted a couple code duplications

Let me know what you think. 

---

_Review requested from @ntBre by @maxmynter on 2025-08-21 19:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:184 on 2025-08-26 19:50_

I deleted the implementation of this method, and all of the tests still pass. Is it possible that this is unnecessary, or just none of the tests exercise it? Intuitively, it seems to me if we're leaving the scope, we don't really need to clear it out, but I might not be following the flow here correctly.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:111 on 2025-08-26 20:06_

I'm not really buying the separation of concerns thing here. To me, the whole concern is to emit diagnostics, so I think it makes sense to have the checker here and emit diagnostics as we find them. I tried this again locally, and it's both less code and avoids allocating an intermediate `Vec` if we just embed the `Checker`, which I think makes it a clear win.

---

_@ntBre reviewed on 2025-08-26 21:31_

Thanks for following up on this and continuing to handle my pedantic feedback :)

All of my minor concerns are resolved, but I'm still thinking the implementation is more complicated than it needs to be. This is kind of a generalization of my comment below, but I just started deleting chunks of code and all of our tests still pass. Here's the patch I was working on locally:

<details><summary>Patch</summary>

```diff
diff --git a/crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs b/crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs
index 305b21d3ed..fc73075430 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs
@@ -66,18 +66,8 @@ impl Violation for MultipleYieldsInContextManager {
 /// RUF062
 pub(crate) fn multiple_yields_in_contextmanager(checker: &Checker, function_def: &StmtFunctionDef) {
     if let Some(context_manager_name) = get_contextmanager_decorator(function_def, checker) {
-        let mut violations = Vec::new();
-        {
-            let mut yield_tracker = YieldTracker::new(&mut violations);
-            source_order::walk_body(&mut yield_tracker, &function_def.body);
-        }
-
-        for range in violations {
-            checker.report_diagnostic(
-                MultipleYieldsInContextManager::new(context_manager_name),
-                range,
-            );
-        }
+        let mut yield_tracker = YieldTracker::new(checker, context_manager_name);
+        source_order::walk_body(&mut yield_tracker, &function_def.body);
     }
 }
 
@@ -107,16 +97,18 @@ fn get_contextmanager_decorator(
 // Within a scope we evaluate all control flow paths and propagate the yields along the
 // maximum path to the outer scope.
 // Return exits the contextmanager decorated function and we stop accumulating yields along that path.
-struct YieldTracker<'a> {
-    violations: &'a mut Vec<TextRange>,
+struct YieldTracker<'a, 'b> {
+    checker: &'a Checker<'b>,
+    name: &'static str,
     scopes: Vec<YieldScope<'a>>,
     reported_ranges: FxHashSet<TextRange>,
 }
 
-impl<'a> YieldTracker<'a> {
-    fn new(violations: &'a mut Vec<TextRange>) -> Self {
+impl<'a, 'b> YieldTracker<'a, 'b> {
+    fn new(checker: &'a Checker<'b>, name: &'static str) -> Self {
         Self {
-            violations,
+            checker,
+            name,
             scopes: vec![YieldScope::new()],
             reported_ranges: FxHashSet::default(),
         }
@@ -124,51 +116,36 @@ impl<'a> YieldTracker<'a> {
 
     fn add_yield(&mut self, expr: &'a Expr) {
         if let Some(scope) = self.scopes.last_mut() {
-            scope.add_yield(expr);
+            scope.yield_expressions.push(expr);
             if scope.yields_excessively() {
                 self.emit_violation(expr);
             }
         }
     }
 
-    fn emit_violation(&mut self, expr: &'a Expr) {
+    fn emit_violation(&mut self, expr: &Expr) {
         let range = expr.range();
         if self.reported_ranges.insert(range) {
-            self.violations.push(range);
+            self.checker
+                .report_diagnostic(MultipleYieldsInContextManager::new(self.name), range);
         }
     }
 
-    fn emit_multiple_violations(&mut self, yields: &[&'a Expr]) {
+    fn emit_multiple_violations<'e>(&mut self, yields: impl IntoIterator<Item = &'e &'e Expr>) {
         // The first yield conforms to the protocol
-        for &yield_expr in yields.iter().skip(1) {
+        for yield_expr in yields.into_iter().skip(1) {
             self.emit_violation(yield_expr);
         }
     }
 
-    fn report_excess(&mut self, yields: &[&'a Expr]) {
-        if yields.len() > 1 {
-            self.emit_multiple_violations(yields);
-        }
-    }
-
     fn propagate_yields(&mut self, yields: &[&'a Expr]) {
-        self.report_excess(yields);
-        let scope = self
-            .scopes
-            .last_mut()
-            .expect("Missing current scope for yield propagation");
         for &yield_expr in yields {
-            scope.add_yield(yield_expr);
-        }
-        let yields_excessive = scope.yields_excessively();
-        let yield_exprs_clone = scope.yield_expressions.clone();
-        if yields_excessive {
-            self.emit_multiple_violations(&yield_exprs_clone);
+            self.add_yield(yield_expr);
         }
     }
 
-    fn push_scope(&mut self, scope: YieldScope<'a>) {
-        self.scopes.push(scope);
+    fn push_scope(&mut self) {
+        self.scopes.push(YieldScope::new());
     }
 
     fn pop_scope(&mut self) -> Option<(Vec<&'a Expr>, bool)> {
@@ -177,18 +154,11 @@ impl<'a> YieldTracker<'a> {
             .map(|scope| (scope.yield_expressions, scope.does_return))
     }
 
-    fn clear_scope_yields(&mut self) {
-        self.scopes
-            .last_mut()
-            .expect("Missing current scope for clearing yields")
-            .clear();
-    }
-
-    fn max_yields(branches: &[Vec<&'a Expr>]) -> Vec<&'a Expr> {
+    fn max_yields<'v>(branches: &'v [Vec<&'a Expr>]) -> &'v [&'a Expr] {
         branches
             .iter()
             .max_by_key(|branch| branch.len())
-            .cloned()
+            .map(Vec::as_slice)
             .unwrap_or_default()
     }
 
@@ -200,7 +170,7 @@ impl<'a> YieldTracker<'a> {
 
     fn handle_loop(&mut self, body: &'a [ast::Stmt], orelse: &'a [ast::Stmt]) {
         self.visit_body(body);
-        self.push_scope(YieldScope::new());
+        self.push_scope();
         self.visit_body(orelse);
     }
 
@@ -220,7 +190,7 @@ impl<'a> YieldTracker<'a> {
             let (except_yields, except_returns) = self
                 .pop_scope()
                 .expect("Missing except handler scope in try-statement");
-            self.report_excess(&except_yields);
+            self.emit_multiple_violations(&except_yields);
 
             if except_returns {
                 returning_except_branches.push(except_yields);
@@ -233,10 +203,6 @@ impl<'a> YieldTracker<'a> {
             .pop_scope()
             .expect("Missing try block scope in try-statement");
 
-        self.report_excess(&try_yields);
-        self.report_excess(&else_yields);
-        self.report_excess(&finally_yields);
-
         let path = TryExceptPath {
             try_yields,
             try_returns,
@@ -248,27 +214,14 @@ impl<'a> YieldTracker<'a> {
         };
 
         if finally_returns {
-            self.handle_terminating_paths(&path);
         } else {
             self.handle_continuing_paths(&path);
         }
     }
 
-    // Finally returns - execution stops, report worst-case path
-    fn handle_terminating_paths(&mut self, path: &TryExceptPath<'a>) {
-        let except_yields = Self::get_max_except_path(path);
-        let base_path = Self::build_pre_finally_path(path, &except_yields);
-        let max_path = Self::append_finally(&base_path, &path.finally_yields);
-
-        self.report_excess(&max_path);
-        self.clear_scope_yields();
-    }
-
     // Finally doesn't return - execution continues, handle all paths
     fn handle_continuing_paths(&mut self, path: &TryExceptPath<'a>) {
-        let (exception_return, exception_no_return) = Self::build_except_paths(path);
-
-        self.report_excess(&exception_return);
+        let (_exception_return, exception_no_return) = Self::build_except_paths(path);
 
         let normal_path = Self::build_try_else_path(path);
 
@@ -276,60 +229,23 @@ impl<'a> YieldTracker<'a> {
     }
 
     fn handle_exclusive_branches(&mut self, branch_count: usize) {
-        let mut returning_branches = Vec::new();
         let mut continuing_branches = Vec::new();
 
         for _ in 0..branch_count {
             let (branch_yields, branch_returns) = self
                 .pop_scope()
                 .expect("Missing branch scope in if/match statement");
-            self.report_excess(&branch_yields);
 
-            if branch_returns {
-                returning_branches.push(branch_yields);
-            } else {
+            if !branch_returns {
                 continuing_branches.push(branch_yields);
             }
         }
 
-        let max_returning = Self::max_yields(&returning_branches);
         let max_continuing = Self::max_yields(&continuing_branches);
 
-        self.report_excess(&max_returning);
         self.propagate_yields(&max_continuing);
     }
 
-    // Path building methods
-    // Find except handler with most yields
-    fn get_max_except_path(path: &TryExceptPath<'a>) -> Vec<&'a Expr> {
-        let max_returning_except = Self::max_yields(&path.returning_except_branches);
-        let max_continuing_except = Self::max_yields(&path.continuing_except_branches);
-
-        if max_returning_except.len() > max_continuing_except.len() {
-            max_returning_except
-        } else {
-            max_continuing_except
-        }
-    }
-
-    // Build path before finally: try + (else or except)
-    fn build_pre_finally_path(
-        path: &TryExceptPath<'a>,
-        max_except_yields: &[&'a Expr],
-    ) -> Vec<&'a Expr> {
-        let mut common_path = path.try_yields.clone();
-
-        if !path.try_returns {
-            common_path.extend(if path.else_yields.len() > max_except_yields.len() {
-                &path.else_yields
-            } else {
-                max_except_yields
-            });
-        }
-
-        common_path
-    }
-
     // Build exception paths with finally appended
     fn build_except_paths(path: &TryExceptPath<'a>) -> (Vec<&'a Expr>, Vec<&'a Expr>) {
         let max_returning_except = Self::max_yields(&path.returning_except_branches);
@@ -357,17 +273,15 @@ impl<'a> YieldTracker<'a> {
         exception_no_return: &[&'a Expr],
     ) {
         if path.try_returns {
-            let try_path = Self::append_finally(&path.try_yields, &path.finally_yields);
-            self.report_excess(&try_path);
             self.propagate_yields(exception_no_return);
         } else if path.else_returns {
-            self.report_excess(normal_path);
+            self.emit_multiple_violations(normal_path);
             self.propagate_yields(exception_no_return);
         } else {
             let max_yield_path = if normal_path.len() > exception_no_return.len() {
-                normal_path.to_vec()
+                normal_path
             } else {
-                exception_no_return.to_vec()
+                exception_no_return
             };
             self.propagate_yields(&max_yield_path);
         }
@@ -397,24 +311,12 @@ impl<'a> YieldScope<'a> {
         }
     }
 
-    fn clear(&mut self) {
-        self.yield_expressions.clear();
-    }
-
     fn yields_excessively(&self) -> bool {
         self.yield_expressions.len() > 1
     }
-
-    fn add_yield(&mut self, expr: &'a Expr) {
-        self.yield_expressions.push(expr);
-    }
-
-    fn set_does_return(&mut self, value: bool) {
-        self.does_return = value;
-    }
 }
 
-impl<'a> source_order::SourceOrderVisitor<'a> for YieldTracker<'a> {
+impl<'a, 'b> source_order::SourceOrderVisitor<'a> for YieldTracker<'a, 'b> {
     fn enter_node(&mut self, node: AnyNodeRef<'a>) -> source_order::TraversalSignal {
         match node {
             AnyNodeRef::StmtFor(_)
@@ -426,7 +328,7 @@ impl<'a> source_order::SourceOrderVisitor<'a> for YieldTracker<'a> {
                 // Track for primary control flow structures
                 // Optional branches like else/finally clauses are handled in leave_node
                 // Except is handled in leave node to maintain logical locality
-                self.push_scope(YieldScope::new());
+                self.push_scope();
             }
             _ => {}
         }
@@ -447,8 +349,7 @@ impl<'a> source_order::SourceOrderVisitor<'a> for YieldTracker<'a> {
                 self.handle_exclusive_branches(branch_count);
             }
             AnyNodeRef::StmtFor(_) | AnyNodeRef::StmtWhile(_) => {
-                let (else_yields, else_returns) =
-                    self.pop_scope().expect("Missing loop else scope");
+                let (else_yields, _) = self.pop_scope().expect("Missing loop else scope");
                 let (body_yields, _body_returns) =
                     self.pop_scope().expect("Missing loop body scope");
 
@@ -460,10 +361,6 @@ impl<'a> source_order::SourceOrderVisitor<'a> for YieldTracker<'a> {
                     }
                 }
                 self.propagate_yields(&else_yields);
-                if else_returns {
-                    // If else returns, don't propagate yield count
-                    self.clear_scope_yields();
-                }
             }
             _ => {}
         }
@@ -482,7 +379,7 @@ impl<'a> source_order::SourceOrderVisitor<'a> for YieldTracker<'a> {
         match stmt {
             ast::Stmt::Return(_) => {
                 if let Some(scope) = self.scopes.last_mut() {
-                    scope.set_does_return(true);
+                    scope.does_return = true;
                 }
             }
             ast::Stmt::FunctionDef(nested) => {
@@ -513,7 +410,7 @@ impl<'a> source_order::SourceOrderVisitor<'a> for YieldTracker<'a> {
                 if self.enter_node(node).is_traverse() {
                     self.visit_body(body);
                     for clause in elif_else_clauses {
-                        self.push_scope(YieldScope::new());
+                        self.push_scope();
                         self.visit_elif_else_clause(clause);
                     }
                     self.leave_node(node);
@@ -532,13 +429,13 @@ impl<'a> source_order::SourceOrderVisitor<'a> for YieldTracker<'a> {
                 if self.enter_node(node).is_traverse() {
                     self.visit_body(body);
                     for handler in handlers {
-                        self.push_scope(YieldScope::new());
+                        self.push_scope();
                         self.visit_except_handler(handler);
                     }
 
-                    self.push_scope(YieldScope::new());
+                    self.push_scope();
                     self.visit_body(orelse);
-                    self.push_scope(YieldScope::new());
+                    self.push_scope();
                     self.visit_body(finalbody);
                     self.leave_node(node);
                 }
```

</details>

My main thinking boils down to this: we're already using the `Scope` pushing and popping to manage scopes, and we're calling `add_yield` every time we encounter a `yield` expression, so it seems to me that we shouldn't have to do any post-processing on the nodes, we just push a `yield` every time we encounter it and emit a diagnostic if we've already seen too many in this `Scope`. You can see this in my `add_yield` implementation in the patch, which I think you already had mostly the same version of, I just tried to use it a bit more aggressively, I think:

```rust
    fn add_yield(&mut self, expr: &'a Expr) {
        if let Some(scope) = self.scopes.last_mut() {
            scope.yield_expressions.push(expr);
            if scope.yields_excessively() {
                self.emit_violation(expr);
            }
        }
    }
```

If we call this every time we encounter a `yield` expression, and we've set up our `Scope` stack correctly, won't we automatically emit a violation in each of the correct places?

I could be wrong and just confused about all of the various `emit_*` and `handle_*` and `propagate` functions, but intuitively this is what I was expecting.

In any case, the code I deleted is not being tested, so we may need more extensive test coverage to make sure it's actually needed.

I'm sorry to keep dragging this out, I was hoping to approve and merge this today. I'm happy to keep playing with the code until I convince myself it's correct if I'm off base here.

---

_@maxmynter reviewed on 2025-08-28 12:48_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:184 on 2025-08-28 12:48_

There are some edge cases where we may need the scope clearing. For example 

```python
@contextmanager
def test():
    for item in something():
        if condition(item):
            break
    else:
        yield 
        return
    yield
```

If we `break` then the `else` doesn't execute and we need to clear the `yield` from the else branch to not further accumulate them. Otherwise we would get a false positive on the next one.

We did say earlier that we don't want logic handling yields in loop bodies and heuristically assume them to happen multiple times. But I would argue that this here is a different case. 

Applications for this pattern could be if we [search](https://book.pythontips.com/en/latest/for_-_else.html#else-clause) for a specific item in `something()`.

We should definitely add a test case for this.

Note: we can get rid of the scope clearing after the `finally` block because that always executes. 

---

_@maxmynter reviewed on 2025-08-28 12:56_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/multiple_yields_contextmanager.rs`:184 on 2025-08-28 12:56_

Test added in [e0ba97b](https://github.com/astral-sh/ruff/pull/17281/commits/e0ba97b9f26f7f44573361e10a4c137e746502ef)

---

_Comment by @maxmynter on 2025-09-05 21:58_

Thank you, @ntBre , for being pedantic. I'm learning a lot here. Plus, in this case it wasn't, I found edge cases this didn't handle before. And it should definitely not be possible to delete core logic without tests breaking. 

So what changed? 
- **Removed Tracker / Reporter Seperation**: Embedded `Checker` directly and removed intermediate `violations` storing
- **Simplified scope management**: Switched from tracking expression references to simple yield counts 
- **Reworked visitor logic**: Now only relies on `visit_stmt` and `visit_expr`, removed complex path-building methods and ~170 lines while keeping parity in logic
- **Added 18 more test cases** including the loop-else-break scenario
- **Fixed false positives**: Now correctly ignores `yield`s in `lambda` and generators

I think the complexity around `try-except` is warranted. Mainly due to the fact that `finally` executes even when previous `try` or `except` branches `return`. We need to keep track of this state to know if we should continue to accumulate `yields` after the finally or if the branch is guarded by a `return`.

---

_Review requested from @ntBre by @maxmynter on 2025-09-05 21:59_

---

_Comment by @maxmynter on 2025-10-19 18:55_

Ping @ntBre (No rush, just that this doesn't get lost :) )

---
