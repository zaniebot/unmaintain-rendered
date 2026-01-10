```yaml
number: 13633
title: "[red-knot] Add control flow for `try`/`except` blocks (v2)"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: except-handler-2
created_at: 2024-10-04T18:19:02Z
updated_at: 2024-10-14T18:28:52Z
url: https://github.com/astral-sh/ruff/pull/13633
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] Add control flow for `try`/`except` blocks (v2)

---

_Pull request opened by @AlexWaygood on 2024-10-04 18:19_

## Summary

This PR adds control flow for `try`/`except`/`else`/`finally` blocks to red-knot. It's a replacement PR for astral-sh/ruff#13338, which had some fundamental issues in its approach, in particular with regards to `finally` blocks.

The semantics of `try`/`except` blocks are very complicated! I've written up a long document outlining all the various jumps control flow could take, which can be found [here](https://astral-sh.notion.site/Exception-handler-control-flow-11348797e1ca80bb8ce1e9aedbbe439d). I won't try to summarise that document in this PR description. But I will give a brief description of some of the ways I've attempted to model these semantics in this PR:

Abstractions for handling `try`/`except` blocks have been added to a new `builder` submodule, `builder/exception_handlers.rs`:
- `TryNodeContext` keeps track of state for a single `try`/`except`/`else`/`finally` block. Exactly what state we need to keep track of varies according to whether the node has a `finally` branch, and according to which branch of the `StmtTry` node we're currently visiting.
- `TryNodeContextStack` is a stack of `TryNodeContext` instances. For any given scope, `try` blocks can be arbitrarily nested; this means that we must keep a stack of `TryNodeContext`s for each scope we visit.
- `TryNodeContextStackManager` is a stack of `TryNodeContextStack`s. Whenever we enter a nested scope, a new `TryNodeContextStack` is initialised by the `TryNodeContextStackManager` and appended to the stack of stacks. Whenever we exit that scope, the `TryNodeContextStack` is popped off the stack of stacks.

The diff for this PR is quite large, but this is mostly tests. There aren't actually _that_ many tests, but they unfortunately need to be quite verbose. This is because we may add a more sophisticated understanding of exception handlers in the future (where we would understand that e.g. `x = 1` can never raise an exception), and I wanted the tests to be robust to this so that they wouldn't have to be rewritten when that happens. (This also helps readability of the tests, since we obviously know that `x = 1` can never raise exceptions.) To address this, I made sure to use assignments to function calls for testing places where a raised exception could cause a jump in control flow. This will be robust to future improvements, since it will always be the case that we will consider a function call capable of raising arbitrary exceptions.

## Test Plan

All tests have been added to `infer.rs`. They all use `reveal_type` to assert that the type of a variable changes as we move through the various `try`/`except`/`else`/`finally` branches.

---

_Label `red-knot` added by @AlexWaygood on 2024-10-04 18:19_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-04 18:19_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-04 18:19_

---

_Comment by @AlexWaygood on 2024-10-04 18:33_

Codspeed reports a 2% regression in the `red_knot[cold]` benchmark. Unless there's either something I'm doing that's completely the wrong approach performance-wise _or_ there are some easy wins we can see that aren't too complicated, I'd prefer not to worry about that too much and try to optimize it in followup PRs. Getting the semantics correct was hard enough 😅

---

_Comment by @github-actions[bot] on 2024-10-04 18:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@T-256 reviewed on 2024-10-04 22:48_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/src/types/infer.rs`:5686 on 2024-10-04 22:48_

I think it should be always Unbound ( or Unknown?) in outer context. here is my testing:
```py
>>> try:
	1/0
except Exception as e:
	pass

>>> e
Traceback (most recent call last):
  File "<pyshell#8>", line 1, in <module>
    e
NameError: name 'e' is not defined
>>> try:
	1/0
except Exception as e:
	e=e

	
>>> e
Traceback (most recent call last):
  File "<pyshell#11>", line 1, in <module>
    e
NameError: name 'e' is not defined
>>> try:
	1/0
except Exception as e:
	ee=e

	
>>> ee
ZeroDivisionError('division by zero')
>>> 
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5686 on 2024-10-04 22:54_

That's correct, but it's actually a separate thing to anything that this PR is trying to fix. (Though we might not have an issue to track this detail, and we probably should!)

try/except blocks don't create new scopes in Python; most variables defined in exception handlers will persist until after the block has finished. Bindings created using `as` in the `except` itself are however special-cased by the interpreter — the interpreter literally inserts an implicit `del` statement for that specific variable prior to the block ending. This is to reduce the chance of creating reference cycles, which would otherwise be very common for this kind of pattern and would create a lot of unnecessary work for the garbage collector.

---

_@AlexWaygood reviewed on 2024-10-04 22:54_

---

_@AlexWaygood reviewed on 2024-10-04 22:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5686 on 2024-10-04 22:57_

I discussed this issue in more depth in the PR description to one of my previous PRs, and there's some discussion in that PR thread of how we plan to model this: https://github.com/astral-sh/ruff/pull/13267#issue-2510259051

---

_@carljm reviewed on 2024-10-05 14:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5686 on 2024-10-05 14:41_

Created https://github.com/astral-sh/ruff/issues/13641 to track this.

---

_@carljm reviewed on 2024-10-05 14:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5686 on 2024-10-05 14:43_

Should we add TODO comments for test assertions that we know should change in the future?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:895 on 2024-10-05 14:59_

Ah, this is tricky indeed. I hadn't fully understood the awkward consequences of the way finally blocks work for our CFG. Thanks for taking the time to think this through!

Unfortunately I don't think this approach (of storing and then re-applying Definitions in the finally block) is going to give us the right results. Consider a case like this:

```
x = 1
try:
    x = could_raise_returns_str()
finally:
    y = x
reveal_type(y)
```

The correct revealed type for `y` is `str`, because in any case where code flow continues after the `finally`, that means the `try` block actually completed without an exception. But this PR currently gives the revealed type as `Literal[1] | str`. By storing and reapplying the `Definition` for `y`, we get the type of the RHS from the scenario where we might have an exception.

I think the only way to handle this correctly is to, in some form, duplicate or double visit the `finally` block. We effectively need to type it twice, once under the assumption that any code it protects might have raised, and again under the assumption that it didn't.

This will be a significant bit of work, as it troubles some core assumptions we have about visiting every expression exactly once. I don't think we should do it in this PR.

But I also don't think we should do this store-and-reapply-definitions thing, either, for two reasons. One is that I think it's just generally important for correctness that we maintain the control-flow-graph abstraction and don't work around it with tricks like this. The other is just about the tradeoff in semantics for Python code. Until/unless we get to a correct double-visit fix, I think the best tradeoff is to accept some false negatives while checking the `finally` block itself, but ensure we get the types correct after the `finally` block. In other words, for now I think we should just visit the finally block under the no-exceptions assumption.

What do you think?

---

_@carljm reviewed on 2024-10-05 15:00_

Haven't fully reviewed yet, just one kind of fundamental thing that jumped out at me on first look, would like to get your thoughts on that.

---

_@AlexWaygood reviewed on 2024-10-05 15:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5686 on 2024-10-05 15:08_

> Should we add TODO comments for test assertions that we know should change in the future?

Now that we have `reveal_type` support, I think I can actually just fix them so that they don't need to change in this PR at all. I'll split that out into a separate PR.

---

_Comment by @MichaReiser on 2024-10-05 15:09_

Would it be possible and would you feel comfortable to make the internal document public and mention it in the pr summary?

I hope I get to review this on Monday or no later than Tuesday 

---

_Comment by @AlexWaygood on 2024-10-05 15:18_

> Would it be possible and would you feel comfortable to make the internal document public and mention it in the pr summary?

Done!

---

_@AlexWaygood reviewed on 2024-10-05 15:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:895 on 2024-10-05 15:38_

Thanks for the great example that shows the flaws in this approach! Ugh, I really thought I'd covered everything this time 🫠 This was, as I'm sure you guessed, the bit of this PR that I was least sure about.

> But I also don't think we should do this store-and-reapply-definitions thing, either, for two reasons. One is that I think it's just generally important for correctness that we maintain the control-flow-graph abstraction and don't work around it with tricks like this. The other is just about the tradeoff in semantics for Python code. Until/unless we get to a correct double-visit fix, I think the best tradeoff is to accept some false negatives while checking the `finally` block itself, but ensure we get the types correct after the `finally` block. In other words, for now I think we should just visit the finally block under the no-exceptions assumption.
> 
> What do you think?

I think this makes me a little sad after I spent so much time thinking about `finally` blocks 😆

I think it is pretty important that we fix this eventually. In the long run, this will lead to false positives as well as false negatives. For example, when we start emitting diagnostics for unreachable code, we will emit spurious errors on the `if` branch inside the `finally` block in this snippet, as we will incorrectly infer it as being unreachable:

```py
x = 42

try:
    x = could_raise_returns_int()
except:
    could_raise()
    x = "foo"
else:
    could_raise()
    x = "foo"
finally:
    if isinstance(x, int):
        ...  # we'd probably detect this as unreachable
             # unless we consider the fact that we might have jumped to the `finally`
             # branch from halfway through an `except` or `else` branch
    else:
        ...
```

Another way I thought of trying to fix this "awkwardness" was to utilise the fact that we know that `try`/`except` blocks with `finally` branches desugar to nested `try`/`except` blocks. We could attempt to "synthesize" a nested `StmtTry` node if we see that a `StmtTry` node has a non-empty `finally` suite. (Not actually _create_ a synthetic `StmtTry` node, but visit the `StmtTry` node exactly as if it were a nested `StmtTry` inside another `StmtTry`.) I actually started off trying to do that, but quickly stopped as this PR's current approach seemed like a simpler solution. (And I was also not sure how this would work with the assertions we have that you mentioned above, about only ever visiting every expression once.) Given the issue you just pointed out in your example, it seems like that probably is the only good way of doing it, though; there doesn't seem to be any way of taking shortcuts while respecting Python's semantics properly.

---

_@carljm reviewed on 2024-10-05 16:12_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:895 on 2024-10-05 16:12_

Yeah, I think you're right that we will want to fix this.

---

_@AlexWaygood reviewed on 2024-10-05 16:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5686 on 2024-10-05 16:57_

https://github.com/astral-sh/ruff/pull/13643

---

_@AlexWaygood reviewed on 2024-10-05 17:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:895 on 2024-10-05 17:42_

I've written up your edge case in my document describing control-flow semantics for exception handlers. It's _very_ specific! I believe it only applies to `StmtTry` nodes that:

- have `finally` blocks, and either:
  - do not have any `except` branches, or
  - all the `except` branches of the `StmtTry` node lead to immediate termination of the scope following the `finally` block, through either a `raise`, `return`or similar.

The specificity of the edge case doesn't mean that it's unimportant to consider, however.

---

_@carljm reviewed on 2024-10-05 17:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:895 on 2024-10-05 17:47_

I think it also applies to try blocks with except handlers, it's just that the issue shifts to considering the possibility of an exception in the exception handler, rather than an exception in the try block?

And try/finally without except handlers is not an uncommon case. 

---

_@AlexWaygood reviewed on 2024-10-05 17:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:895 on 2024-10-05 17:50_

> I think it also applies to try blocks with except handlers, it's just that the issue shifts to considering the possibility of an exception in the exception handler, rather than an exception in the try block?

Ah, great point.

> And try/finally without except handlers is not an uncommon case.

I said _specific_, not uncommon! I agree that `try`/`finally` without `except` is pretty common, so I definitely agree this is an important case to consider.

---

_Comment by @AlexWaygood on 2024-10-13 14:46_

(I still have yet to address @carljm's feedback, which is going to require a somewhat significant rewrite. I just pushed some changes to rewrite the tests using `mdtest`.)

---

_Comment by @AlexWaygood on 2024-10-13 15:55_

We may or may not want to add a lot of the `finally`-related infrastructure back when we add accurate inference for `finally` blocks in the future, so rather than pushing to this PR branch I've created yet another new PR here: https://github.com/astral-sh/ruff/pull/13729. That's so it'll be easy to see exactly what the `finally`-related infrastructure looked like before we decided for now to assume that there are no currently-suspended exceptions in the `finally` block.

TL;DR: I'm closing this for now; astral-sh/ruff#13729 is the new version that addresses @carljm's comments!

---

_Closed by @AlexWaygood on 2024-10-13 15:55_

---

_Branch deleted on 2024-10-13 15:55_

---

_Comment by @carljm on 2024-10-14 18:28_

Another thing I realized that we'll need to take into account when we do smarter handling for `finally` blocks is that if a `finally` block contains a `continue` or `break` statement, that can result in further code in the same scope seeing the results of executing a `finally` block when it was entered via exception.

---
