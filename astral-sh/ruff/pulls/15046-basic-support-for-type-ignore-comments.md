```yaml
number: 15046
title: "Basic support for `type: ignore` comments"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/type-ignore
created_at: 2024-12-18T16:04:36Z
updated_at: 2024-12-20T18:16:22Z
url: https://github.com/astral-sh/ruff/pull/15046
synced_at: 2026-01-12T15:55:50Z
```

# Basic support for `type: ignore` comments

---

_@MichaReiser_

## Summary

This PR adds initial support for `type: ignore`. It doesn't do anything fancy yet like:

* Detecting invalid type ignore comments
* Detecting type ignore comments that are part of another suppression comment: `# fmt: skip # type: ignore`
* Suppressing specific lints `type: ignore [code]`
* Detecting unsused type ignore comments
* ...

The goal is to add this functionality in separate PRs.

## Test Plan


---

_Label `red-knot` added by @MichaReiser on 2024-12-18 16:04_

---

_Renamed from "Initial type: ignore support" to "Basic support for `type: ignore` comments" by @MichaReiser on 2024-12-18 16:06_

---

_Comment by @github-actions[bot] on 2024-12-18 16:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-12-18 16:16_

---

_Review requested from @carljm by @MichaReiser on 2024-12-18 16:16_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-18 16:16_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-18 16:16_

---

_@MichaReiser reviewed on 2024-12-18 16:22_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/suppression.rs`:9 on 2024-12-18 16:22_

Whoops. I accidentally commited this file when I rebased https://github.com/astral-sh/ruff/pull/14956/files#diff-b1807b646317ac1945d748f7db40451ac62c582b1bbc049b174e8d98f13d3f22 Probably because it didn't get stashed with `git stash`. So consider all code in this file as new

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:21 on 2024-12-18 16:57_

```suggestion
## Before opening parenthesis

A suppression that applies to all errors before the opening parenthesis.
```

("Parentheses" is plural, "parenthesis" is singular; here we are discussing a single opening parenthesis.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:59 on 2024-12-18 16:59_

Nit: when we have a TODO, I prefer to be extra clear about what precisely should change in the test when the TODO is fixed, to reduce ambiguity for future-us.
```suggestion
TODO: We should support this for better interopability with other suppression comments.

```py
# fmt: off
# TODO this error should be suppressed
# error: [unresolved-reference]
a = test \
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:26 on 2024-12-18 17:39_

I'm a bit confused how this test is working. I see you use the end of the diagnostic range for finding applicable suppressions. In this case the diagnostic spans the entire statement, so the end of the diagnostic range would be on the third line. But the suppression range would be just the first line. So why does this suppression apply? (To be clear, I think it _should_ apply, I just don't understand how it works. I could debug and figure it out, but faster to ask you :) )

---

_@carljm approved on 2024-12-18 17:39_

Nice!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/suppression.rs`:34 on 2024-12-18 18:01_

```suggestion
                line_start = token.end();
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/suppression.rs`:29 on 2024-12-18 18:04_

does this mean we ignore `type: ignore` comments that use mypy error codes? That's different to e.g. what pyright does. If pyright sees a `type: ignore[misc]` comment, it just interprets it as a bare `type: ignore` comment that suppresses all typing violations (because the tool doesn't support using `type: ignore` comments with error codes).

This has proved to be very useful in e.g. typeshed, where we often have `type: ignore` comments that use mypy error codes. Pyright just treats them as blanket `type: ignore` comments in our CI at typeshed, but mypy understands them as only applying to specific mypy error codes.

Pytype is the third type checker that we run in CI at typeshed, but I believe they do not support `type: ignore` comments at all, so anything we put in the `type: ignore` comment is sort-of irrelevant from pytype's perspective

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/suppression.rs`:95 on 2024-12-18 18:07_

```suggestion
    /// However, there are few cases where the range gets expanded to
```

---

_@AlexWaygood approved on 2024-12-18 18:08_

Very nice!

---

_@MichaReiser reviewed on 2024-12-18 20:33_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/suppression.rs`:29 on 2024-12-18 20:33_

Oh nice find. Let me add a test for this and also take a look at pyrights regex

It's not entirely clear to me yet whether we should support type: ignore comments with codes or do what pyright does. I can see how ignoring them is useful in a situation like typeshed but I could see why users would want to use them 

---

_@MichaReiser reviewed on 2024-12-18 20:36_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:26 on 2024-12-18 20:36_

This is just a bad example. There's only one error: the unresolved reference to Test. The declared type is unknown, so the assignment itself is fine. 

I think a better example would be to change the right side to Test too and add and acknowledge that error with an error pragma comment. I'll change it tomorrow 

---

_@carljm reviewed on 2024-12-18 21:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:26 on 2024-12-18 21:03_

Oh, of course!

But does that mean that with the current implementation, in a case where the diagnostic range does span multiple lines:

```py
x: str = (
    1 + 3
)  # type: ignore
```

You would be required to suppress this on the last line (as shown), not on the first (after the opening paren)?

I find that a little counter-intuitive; I would expect to be able to suppress a multi-line diagnostic on the line where it starts. (Imagine for example a diagnostic covering an entire `def` or `class` statement: I'd want to suppress that with a `type: ignore` on the `def` or `class` line, not on the final line of the function or class body, which would be pretty weird.) How does this work in pyright and mypy?

---

_@carljm reviewed on 2024-12-18 21:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/suppression.rs`:29 on 2024-12-18 21:05_

I don't think we should support `type: ignore` comments with our own codes. This will cause us a practical problem right out of the gate, because typeshed uses mypy codes in `type: ignore` comments, and our codes don't match mypy codes. If we did this, we'd also have to implement compatibility code aliases for mypy codes right away, which is a complexity we probably want to avoid?

For better or worse, mypy already claimed the "code namespace" for `type: ignore`; given the typeshed situation I think our best option is to do what pyright does: accept and ignore codes with `type: ignore`. And use our own codes with our own form of suppression comment.

---

_@MichaReiser reviewed on 2024-12-18 21:12_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:26 on 2024-12-18 21:12_

I'd have to look but there's the other parenthesis case where mypy doesn't do anything fancy

For classes and other suites. I don't think we should ever use the entire statement range. It leads to awful diagnostic frames. We should instead report the diagnostic on the name, which then also works perfectly with suppression comments 

---

_@MichaReiser reviewed on 2024-12-18 21:15_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/suppression.rs`:29 on 2024-12-18 21:15_

I don't mind deferring the decision but I'm not sure if the decision should only be based on what one project does. 

I don't think supporting codes does imply that we have to support mypy rule names. I consider these two separate decisions

---

_@AlexWaygood reviewed on 2024-12-18 21:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/suppression.rs`:29 on 2024-12-18 21:19_

I agree that in terms of the principle it should not be, and I totally get that it's frustrating to have our design space constrained here. But I agree with Carl that it will probably cause too many compatibility problems if we reject or ignore all `type: ignore` comments that use mypy error codes. That will make it quite painful for users to migrate from mypy to red-knot, or for users to run both tools on one repo. Obviously we hope that most users won't need to run multiple type checkers on any one repo, but some users such as typeshed will *have* to.

---

_@MichaReiser reviewed on 2024-12-18 21:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/suppression.rs`:29 on 2024-12-18 21:23_

That makes sense. I'm also not reasoning about it nor feeling frustrated. I just think that users will ask for it and we may want to add support for it in some way or another. It could just be an option.

Ignoring codes also isn't necessarily enough to run mypy and red knot side by side. At least not if red knot warns about unused suppressions. I don't think we have to solve this now and not supporting codes seems a good start. But we may need a little more but let's learn from users and testing on real projects 

---

_@carljm reviewed on 2024-12-18 21:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:26 on 2024-12-18 21:24_

It looks like mypy and pyright both allow you to put the `type: ignore` on _any_ line within the diagnostic range:

*  https://pyright-play.net/?strict=true&code=IYLgBAzgLgTmC8YAUAoM6wEYwGoxowCYUBKFFAD3GjkSXQGIwoBPABwFNwBLAcwDsA9jA4F02PMTIoW1WAmRisuRs3ZcwfISKVTyLQnNqKMynLtWtOPAcNHSAXkYWpTEsFMvqb2jkA
* https://mypy-play.net/?mypy=latest&python=3.12&gist=2352369ed2274c23a8e54605a1307f16

(The difference between them in this example is that pyright includes the opening and closing parentheses in the diagnostic range; mypy does not.)

I think we will probably want to match this suppression-on-any-line behavior for multi-line diagnostics, otherwise we are just creating an unnecessary barrier for migration as some people's `type: ignore` comments that worked on mypy/pyright will stop working for us.

Either way, it would be good to include a test like this one with multi-line diagnostic range, to show what our behavior is, and maybe include some commentary on the tradeoffs.

(Agreed that it's better to avoid reporting diagnostics on an entire `def` or `class` statement.)

---

_@carljm reviewed on 2024-12-18 21:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/suppression.rs`:29 on 2024-12-18 21:26_

I feel quite strongly that we should avoid creating migration barriers unnecessarily, and mypy is currently the most popular type checker, so I do think we will sometimes have to make decisions based on what it does.

Supporting our own codes in `type: ignore`, without mypy compat aliases, unnecessarily creates a migration barrier from mypy, so I think it would be shooting ourselves in the foot to do this without very strong reasons.

(Edit: wrote this before seeing the previous two comments, github wasn't live updating. Conclusion in the last comment sounds good, agreed that it could be an option.)

---

_@MichaReiser reviewed on 2024-12-19 09:05_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:26 on 2024-12-19 09:05_

> It looks like mypy and pyright both allow you to put the type: ignore on any line within the diagnostic range:

I've to take another look but i'm a bit concerned about using intersection here because it would mean that an ignore comment in the body of a statement would ignore any error in that statement's clause. E.g. 

```
class ThisClassHasAnError:
	def test():
		a + 3  # type: ignore
```

We don't want that the `type: ignore` ignores any errors from the enclosing function or class!

---

_@MichaReiser reviewed on 2024-12-19 09:33_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:26 on 2024-12-19 09:33_

Sooo, this is all slightly more subtle ;)

Your examples don't work with Red Knot because we report a different error range:

* Red Knot: Reports the entire assignment as invalid (the statement!)
* Mypy: Reports the range of the right side (the assigned value)
* Pyright: Reports the range of the right side

So red knot is the weird one here.

To add another data point. Ruff does the opposite of what I have here. It searches by the range's start position https://play.ruff.rs/280de058-f694-4f93-8321-46474a0aa541

Overall, I'm worried about allowing suppression comments anywhere inside an expression. Let's say you have

```py
a = (
	some_call(a, b, c)   # type: ignore 
		+ d
		+ e
		+ b
)
```

The `type: ignore` now disables not just an error related to the call but also any diagnostics related to the binary expression. I'd find this very surprising. It gets worse the more nested your expressions are. 

I could see a middle ground where we allow any suppression comment on the same line as the diagnostic range. 


---

_@MichaReiser reviewed on 2024-12-19 10:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:26 on 2024-12-19 10:09_

My understanding is that Mypy also uses the start and end line numbers. 

Mypy checks if the error is ignored on any `origin.lines`

https://github.com/python/mypy/blob/1f9317f593dc41a2805a3093e2e1890665485e76/mypy/errors.py#L493-L505

And origin is defined as

https://github.com/python/mypy/blob/1f9317f593dc41a2805a3093e2e1890665485e76/mypy/errors.py#L98-L101

So I think we should do the same for now

---

_Converted to draft by @MichaReiser on 2024-12-19 11:06_

---

_Comment by @MichaReiser on 2024-12-19 11:37_

I adjusted the implementation to accept `type: ignore` comments on the start and end lines of the diagnostic range. I understand that this matches mypy's behavior https://github.com/astral-sh/ruff/pull/15046#discussion_r1891545731. 

However, this in itself won't be sufficient for mypy compatibility. We'd also have to match mypy's diagnostic range which we currently don't for invalid assignments (https://github.com/astral-sh/ruff/pull/15046#discussion_r1891439644). But that's a problem out of this PR's scope



---

_Marked ready for review by @MichaReiser on 2024-12-19 11:37_

---

_Review requested from @carljm by @MichaReiser on 2024-12-19 11:37_

---

_@carljm reviewed on 2024-12-19 15:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:26 on 2024-12-19 15:43_

> We don't want that the `type: ignore` ignores any errors from the enclosing function or class!

Well, we agreed that we shouldn't have diagnostics spanning an entire function or class anyway. But yes, the point about full intersection potentially leading to too much suppression makes sense. It is interesting that this still seems to be what pyright does (at least that's how I interpret https://pyright-play.net/?strict=true&code=IYLgBAzgLgTmC8YAUAoM6wEYwGoxowCYUBKFFAD3GjkSXQGIwoBPABwFNwBLAcwDsA9jA4F02HGLDEyKFtVgJkUiY2bsuYPkJFSZ5FoQW1lGLLj1rWnHgOGjZAL2NLUZiXtJWNtnRyA ). Though I'm having trouble constructing any case where it's ambiguous -- pyright seems to mostly avoid "nested" diagnostics by using narrow enough ranges, and having erroring operations return Unknown which then causes the outer operation to no longer be an error.

EDIT: oops, should have read your updated tests first, I see you explored this already! The ambiguous case can be constructed in pyright by using `typing.cast`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:48 on 2024-12-19 15:46_

Could we use `/ 0` here and then the test would function without a TODO?

Or maybe that muddies the comparison with pyright, which doesn't error on division by zero

---

_@carljm approved on 2024-12-19 15:48_

Looks good, thank you for digging into the multi-line behavior here!

---

_Comment by @MichaReiser on 2024-12-19 16:45_

Something for tomorrow. I want to take another look why we need to test both the start and the end. Ruff only tests the start. Testing both might be a problem for a future linter if we have

```py
if True:
	print("test")
else: 
	pass  # knot: ignore
```

because the ignore on the `pass` would now ignore any error that applies to the entire else-clause.

---

_Comment by @MichaReiser on 2024-12-20 09:29_

Not supporting end positions does seem unintuitive. For example, the following wouldn't work

```py
y = (
    4 /
    0  # type: ignore
)
```

We should push users towards using specific error codes to mitigate the issue that I pointed out in https://github.com/astral-sh/ruff/pull/15046#issuecomment-2555032954

---

_@MichaReiser reviewed on 2024-12-20 09:35_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type-ignore.md`:48 on 2024-12-20 09:35_

It doesn't work with `/ 0` because Red Knot doesn't support `cast` yet (and without the cast it's `Unknown / 0` which we seem to accept)

---

_Merged by @MichaReiser on 2024-12-20 09:35_

---

_Closed by @MichaReiser on 2024-12-20 09:35_

---

_Branch deleted on 2024-12-20 09:35_

---

_Comment by @carljm on 2024-12-20 18:16_

> We should push users towards using specific error codes

And try to avoid overly-large diagnostic ranges. In particular, I suspect we should try to avoid diagnostic ranges on statements, because I think those are the cases where the end is least intuitive.

---
