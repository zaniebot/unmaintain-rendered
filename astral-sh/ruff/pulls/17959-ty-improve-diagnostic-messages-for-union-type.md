```yaml
number: 17959
title: "[ty] improve diagnostic messages for union type function calls"
type: pull_request
state: closed
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
base: main
head: ag/diag-call-union
created_at: 2025-05-08T16:30:57Z
updated_at: 2025-05-09T14:14:26Z
url: https://github.com/astral-sh/ruff/pull/17959
synced_at: 2026-01-12T15:56:09Z
```

# [ty] improve diagnostic messages for union type function calls

---

_@BurntSushi_

This PR goes to some effort to improve diagnostics for invalid function
calls, when the type of the function is a union of function types.
Previously, ty was picking the first error and emitting that only. With
this change, we surface all errors. But we try to go about it with some
smarts.

Specifically, when a function call on a union type fails to type
check, we emit the reason that each variant is invalid (which might
only be one) as sub-diagnostics. Moreover, we change the prose on the
diagnostics to connect the initial error message with the added context
in each sub-diagnostic.

One downside here is that this introduces an alternative code path for
each "invalid function call" diagnostic. I resisted the urge to try and
introduce a heavy abstraction here. Instead, we just pass down some
additional data when we're dealing with a union type, and then do case
analysis on that to determine whether, e.g., `INVALID_ARGUMENT_TYPE`
should be its own top-level diagnostic or if it should be a
sub-diagnostic.

I also chose to structure the code how I did to preserve locality. That
is, instead of having two completely different functions for reporting
top-level diagnostics versus sub-diagnostics, I kept everything close
together. So that if you're changing how the top-level
`INVALID_ARGUMENT_TYPE` diagnostic reads, then it should hopefully be
clearer whether the sub-diagnostic needs changing as well.


---

_Review requested from @carljm by @BurntSushi on 2025-05-08 16:30_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-05-08 16:30_

---

_Review requested from @sharkdp by @BurntSushi on 2025-05-08 16:30_

---

_Review requested from @dcreager by @BurntSushi on 2025-05-08 16:30_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-05-08 16:32_

---

_Label `ty` added by @AlexWaygood on 2025-05-08 16:32_

---

_Comment by @BurntSushi on 2025-05-08 16:34_

Here's an example when multiple union variants are invalid:

![union-multi](https://github.com/user-attachments/assets/00d16f08-0754-49f6-a08d-f0233c18b438)

And here's an example when only one union variant is invalid. In this case, we get the additional context for that function call:

![union-single](https://github.com/user-attachments/assets/cba9e097-d9fb-4d8b-9d9f-80ee6cfe69a2)

I chose to elide that context in the case where multiple variants are invalid because it felt like it would be pretty overwhelming to the end user. This felt like a better place to land in terms of information density from my perspective.

---

_Comment by @MichaReiser on 2025-05-08 16:38_

Can you tell me more about how this works when some specific rule codes are disabled and what severity we pick?

---

_Comment by @BurntSushi on 2025-05-08 16:38_

Also, I decided to just add a new diagnostic called `INVALID_UNION_CALL` for now. We did discuss an alternative here, which was to add a more generic `INVALID_CALL` and possibly collapse all other function calls to that as well. I was somewhat hesitant to do this because 1) I didn't want to tie this diagnostic improvement to that bigger change unnecessarily and 2) I wasn't totally clear on whether merging all of the function call diagnostics into one was the right way to go. I guess the thing that comes to mind is whether users might want to have more granular control over which kinds of function calls are flagged by ty. (I would guess that this doesn't matter too much, but I'm not certain.)

---

_Comment by @BurntSushi on 2025-05-08 16:41_

> Can you tell me more about how this works when some specific rule codes are disabled and what severity we pick?

Anything that was previously flagged as an invalid function call because one or more variants were not callable now falls under a new `INVALID_UNION_CALL` diagnostic. But within that, if you have, e.g., `INVALID_ARGUMENT_TYPE` disabled, then that specific diagnostic could still be surfaced if `INVALID_UNION_CALL` was enabled and one of its union variants was wrong because of an invalid argument type.

EDIT: And same deal for severity. The severity is always tied to `INVALID_UNION_CALL`.

---

_Comment by @MichaReiser on 2025-05-08 16:51_

> Anything that was previously flagged as an invalid function call because one or more variants were not callable now falls under a new INVALID_UNION_CALL diagnostic. But within that, if you have, e.g., INVALID_ARGUMENT_TYPE disabled, then that specific diagnostic could still be surfaced if INVALID_UNION_CALL was enabled and one of its union variants was wrong because of an invalid argument type.

Thanks, I'm not sure if this will be confusing to users. I would be somewhat surprised if a call fails because of an `INVALID_ARGUMENT_TYPE` (each with a different one) and I've this rule disabled.

On the other hand. Only showing union variants of the rules that are enabled can also be confusing:

```py
def foo(a: int, b: str): ...
def bar(a: str): ...

def rand(): bool

if rand():	
	x = foo
else:
	x = bar

x("4", "abcd")
```

Here again, I've disabled `INVALID_ARGUMENT_TYPE`. ty would only show that they try to call `bar` with too many arguments but it's equally likely that the error is the wrong argument type (should be `4`)


I'm not sure what the desired behavior is. I'm interested to hear from others


Edit: Actually, my example doesn't make any sense. It's a union! All variants need to be callable. I think I'd rather we only show variants where the rule isn't disabled. But the re-coding then still feels somewhat odd.



---

_Comment by @MichaReiser on 2025-05-08 17:00_

The *least* confusing in my view would be to have a `POSSIBLY_INVALID_CALL` and `INVALID_CALL` if we want to aggregate the diagnostics like this (I love the improvements!). But I'm not sure if this is too coarse graind for a python type checker

---

_Comment by @BurntSushi on 2025-05-08 17:08_

My thinking here was that folks generally seemed okay with collapsing everything to a more generic `INVALID_CALL` diagnostic. (Not sure if everyone felt that way! I'm still on the fence about it personally.) In that world, you wouldn't be able to have union call diagnostics enabled but invalid argument types disabled. You either get all of them or none of them. I perceive my approach to be "no worse" than that, other than the potential confusion that comes from `INVALID_ARGUMENT_TYPE`.

In order to make this less confusing in terms of rule selection, I think I'd need to tie together the concept of "a call binding error for this variant occurred" and "the specific diagnostic for that error is enabled." Alternatively, if we decide that all function calls should collapse to one diagnostic ID, then I think this entire problem goes away.

Does how we deal with rule selection here need to block this PR? I think this PR is an improvement on the status quo. Namely, the status quo is _also_ confusing, but in a different way. Previously, you just wouldn't even get any diagnostics aside from one variant regardless of what you do with rule selection. And if you ignore one of the rules resulting from an errant union function call, that might suppress the _entire_ diagnostic even if you haven't disabled the error that comes from the other variant. For example:

```
$ cat main.py
def f1() -> int: return 0
def f2(name: str) -> int: return 0

def _(flag: bool):
    if flag:
        f = f1
    else:
        f = f2
    x = f(3)
$ run-ty -q main -- check main.py
error: lint:too-many-positional-arguments: Too many positional arguments to function `f1`: expected 0, got 1
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^
  |
info: `lint:too-many-positional-arguments` is enabled by default

Found 1 diagnostic
$ run-ty -q main -- check --ignore=too-many-positional-arguments main.py
All checks passed!
```

This is despite the fact that there _should_ still be an `invalid-argument-type` lint that is surface here. You didn't explicitly ignore it, but because of the arbitrariness of how union function call errors are handled, you end up with weird behavior. In contrast, with this PR, you get all of the diagnostics. And when you disable the lint that is surfaced in the output, you get the expected behavior IMO:

```
$ run-ty -q pr1 -- check main.py
error: lint:invalid-union-call: Union type `(def f1() -> int) | (def f2(name: str) -> int)` is not callable because of one or more incompatible variants
 --> main.py:9:9
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |         ^^^^
  |
info: Cannot call union variant `def f1() -> int` because there are too many positional arguments to function `f1`: expected 0, got 1
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^
  |
info: Cannot call union variant `def f2(name: str) -> int` because the argument to this function is incorrect
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^ Expected `str`, found `Literal[3]`
  |
info: `lint:invalid-union-call` is enabled by default

Found 1 diagnostic

$ run-ty -q pr1 -- check --ignore=invalid-union-call main.py
All checks passed!
```

The problem is that this still happens:

```
$ run-ty -q pr1 -- check --ignore=too-many-positional-arguments main.py
error: lint:invalid-union-call: Union type `(def f1() -> int) | (def f2(name: str) -> int)` is not callable because of one or more incompatible variants
 --> main.py:9:9
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |         ^^^^
  |
info: Cannot call union variant `def f1() -> int` because there are too many positional arguments to function `f1`: expected 0, got 1
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^
  |
info: Cannot call union variant `def f2(name: str) -> int` because the argument to this function is incorrect
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^ Expected `str`, found `Literal[3]`
  |
info: `lint:invalid-union-call` is enabled by default

Found 1 diagnostic
```

I'd be happy to write this up in an issue, but I would advocate in favor of not considering this a blocking concern.

---

_Comment by @MichaReiser on 2025-05-08 17:16_

> I'd be happy to write this up in an issue, but I would advocate in favor of not considering this a blocking concern.

I do think it's somewhat blocking because it introduces an entire new concept of aggregated diagnostics and the impact on suppression and configuration isn't clear to me. I worry that it makes it overly complicated.

I wasn't aware of the problem the current diagnostic has. This is a massive improvement. 

I think the least confusing and closest to our current configuration model is to only group the diagnostics with the same error code. This has the downside that there could be multiple *invalid call* errors on the same line but I think that would be the same as for non-union calls and I would prefer if the experience between unions and non-unions is consistent ([playground](https://play.ty.dev/441ce50d-638d-4c3c-9246-66497ff0bf1c))

Unless we want to re-think how to handle invalid call diagnostics in general but I think that would likely mean having more coarse-grained diagnostic codes.

I'm sorry to only bringing this up now. It was a bit late yesterday and I wasn't able to fully think this through.

---

_Comment by @BurntSushi on 2025-05-08 17:30_

> I think the least confusing and closest to our current configuration model is to only group the diagnostics with the same error code.

I think I probably strongly disagree with this. I'd be curious to hear from others too. But if we went this direction, then the ability to write a holistic diagnostic message covering the concept of a union becomes effectively impossible I think. The benefit of the approach in this PR is that you can thread a single concept through the prose of the messages. i.e., You're connecting the invalid union call with each of the invalid variants regardless of _how_ each of those variants is invalid. I feel like that's the right conceptual model for good diagnostic messages in a case like this, and I feel like backing this change out in order to only group diagnostics with the same ID is optimizing for the wrong thing.

I fully grant there is some confusion around how to deal with aggregate diagnostics and rule selection, but I think the sort of diagnostic messaging in this PR ought to be our target. And that we should fix/rejigger rule selection in order to ensure this style of messaging is actually possible and encouraged.

> but I think that would be the same as for non-union calls and I would prefer if the experience between unions and non-unions is consistent

So for this case, we do still emit multiple diagnostics for the same line, but they are sub-diagnostics:

```
$ cat main.py
def foo(a: str, b: int): ...
def bar(a: str, b: int): ...

def _(flag: bool):
    if flag:
        f = foo
    else:
        f = bar
    f(1)

$ run-ty -q pr1 -- check main.py
error: lint:invalid-union-call: Union type `(def foo(a: str, b: int) -> Unknown) | (def bar(a: str, b: int) -> Unknown)` is not callable because of one or more incompatible variants
 --> main.py:9:5
  |
7 |     else:
8 |         f = bar
9 |     f(1)
  |     ^^^^
  |
info: Cannot call union variant `def foo(a: str, b: int) -> Unknown` because no argument provided for required parameter `b` of function `foo`
 --> main.py:9:5
  |
7 |     else:
8 |         f = bar
9 |     f(1)
  |     ^^^^
  |
info: Cannot call union variant `def foo(a: str, b: int) -> Unknown` because the argument to this function is incorrect
 --> main.py:9:7
  |
7 |     else:
8 |         f = bar
9 |     f(1)
  |       ^ Expected `str`, found `Literal[1]`
  |
info: Cannot call union variant `def bar(a: str, b: int) -> Unknown` because no argument provided for required parameter `b` of function `bar`
 --> main.py:9:5
  |
7 |     else:
8 |         f = bar
9 |     f(1)
  |     ^^^^
  |
info: Cannot call union variant `def bar(a: str, b: int) -> Unknown` because the argument to this function is incorrect
 --> main.py:9:7
  |
7 |     else:
8 |         f = bar
9 |     f(1)
  |       ^ Expected `str`, found `Literal[1]`
  |
info: `lint:invalid-union-call` is enabled by default

Found 1 diagnostic
```

> I'm sorry to only bringing this up now. It was a bit late yesterday and I wasn't able to fully think this through.

<3

---

_Renamed from "ty_python_semantic: improve diagnostic messages for union type function calls" to "[ty]: improve diagnostic messages for union type function calls" by @MichaReiser on 2025-05-08 17:47_

---

_Renamed from "[ty]: improve diagnostic messages for union type function calls" to "[ty] improve diagnostic messages for union type function calls" by @MichaReiser on 2025-05-08 17:47_

---

_Comment by @BurntSushi on 2025-05-08 19:06_

@MichaReiser and I chatted a bit offline. I think right now we could use feedback from others on the team.

Additionally, I've pushed a commit that _might_ appease some concerns here.

Namely, it modifies how we add sub-diagnostics for the
invalid-union-call case. Specifically, they now respect rule selection.
So if, for example, `--ignore invalid-argument-type` is given, then that
diagnostic won't show up as a sub-diagnostic of `invalid-union-call`.
And if all of the sub-diagnostics of an `invalid-union-call` are
suppressed, then the overall `invalid-union-call` diagnostic is
suppressed too. Moreover, one can suppress the entire thing regardless
of sub-diagnostics via `--ignore invalid-union-call`.

For examples, see the new CLI snapshot tests: https://github.com/astral-sh/ruff/pull/17959/commits/50fe25f583324fc6ae6d007e52c4e637b7ee4156#diff-81dca7a83c246c130b021bcb481291e7359dc5a08e60694597500cf953442010R622

---

_Comment by @carljm on 2025-05-08 20:13_

(Lots of interesting discussion here -- will respond a bit later but want to give it proper consideration, not just an off-the-cuff reply. Very excited about this improvement, regardless.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/constructor.md`:102 on 2025-05-08 23:41_

This looks like it was a pain to update -- I'd also be fine killing the message matchers here and snapshotting these diagnostics instead.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:234 on 2025-05-08 23:47_

In our prior diagnostic messages, we've tried to clearly distinguish between "not callable" (meaning, you've tried to call an object which doesn't support the Python calling protocol) vs "invalid call" or more specific descriptions (meaning, the object is callable but something about the way you tried to call it is wrong.) Along that line I would prefer to word this more like "Invalid call to union type `{}`" (if we keep this generic top-level message).

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1138 on 2025-05-08 23:53_

Unrelated to this PR (except as a natural next follow up), but this diagnostic is still quite bad and hides all the relevant information (why each overload failed to match). This is https://github.com/astral-sh/ty/issues/274

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1759 on 2025-05-09 00:05_

I think this message doesn't quite work grammatically when prepended with "... because ..." -- needs to be "no argument{s} _was_ provided for ..."

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1981 on 2025-05-09 00:07_

This comment looks out of place?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:2029 on 2025-05-09 00:10_

This is cool that we can do this, but from a user perspective I'm not sold on the rationale for it.

If I'm the user getting a report that my union call failed due to multiple variants failing, what am I going to do? I'm going to look at each failed variant in turn and work out what I need to do to make the call succeed. Why would I want less useful context to help me figure that out, just because there were also errors on another variant?

Consider the analogy pushed up one layer. We don't say "well, you had too many diagnostics on your project, so to avoid information overload, we are going to switch to concise output mode". What are you going to do with the many diagnostics? You are going to consider each one in turn and try to resolve it. You still want the context for each one in order to do that successfully. If you _want_ less verbosity for some reason, you'll ask for concise output instead.

In both of these cases, the problem with omitting the extra context automatically is that you then don't have any easy way to _get_ the extra context that we decided not to give you. Changing your code to no longer call a union may be quite a significant change.

---

_@carljm approved on 2025-05-09 00:33_

I think this is a huge improvement on the status quo, and personally I'd be ok with landing it and seeing how it works out in practice. That said, I do think the issues raised here are important ones to consider, thanks @MichaReiser for raising them. Sorry for the novel-length comment; there's a tl/dr at the bottom :)

1) I don't like the separate `invalid-union-call` diagnostic code, because it seems like something I would never want to suppress or enable or change severity of, on its own. If I don't want to see errors due to wrong argument types in calls, then I don't want to see them inside or outside of unions. I can't imagine ever saying "I don't want to see any diagnostics involving calls to unions, but I still want to see all diagnostics for all other calls." Or vice versa. (That said, I saw @JelleZijlstra mentioned a preference for having this code -- I'd love to hear more about why.)

2) Implied by the above, I do think the latest improvement here (to silence union-call sub-diagnostics if their fine-grained rule code is silenced, and potentially silence the entire union-call error if that doesn't leave any remaining errors) is good, if we will keep the `invalid-union-call` diagnostic code. Then at least disabling `invalid-argument-type` actually disables it, inside or outside of unions. We still have the oddity of an `invalid-union-call` diagnostic code that I think is kind of weird and useless, but a useless code is less of a problem than not actually being able to suppress all `invalid-argument-type` errors because some of them are inside a union call.

3) That said, I still think it's weird that you might disable or enable `invalid-argument-type`, and that change alone causes `invalid-union-call` diagnostics to appear or disappear. Or that you suppress `invalid-union-call`, and now you don't see invalid argument type errors, if they happen to be inside a union. That's not the way suppressing a rule code would normally be expected to work. (But I'm not sure in practice if this is really a problem?)

4) I think I'm actually pretty open to Micha's suggestion that we should aggregate union call errors by type. So if some variants have `invalid-call-argument` and others have `call-not-callable`, you'd get two top-level diagnostics, with codes `invalid-call-argument` and `call-not-callable`, each one aggregating the sub-diagnostics for union variants with that issue. This entirely solves the problems with enabling/disabling rule codes. This still allows the union aggregation to provide the context that I find most important, which is just the fact that you tried to call a union, with X Y and Z variants, and these are the ones that had issues (as opposed to just providing the sub-diagnostics directly with zero union context, which I think would be a problem.) What it doesn't allow us to do is _reduce_ the context we present as intelligently, in the (possibly rare?) cases where multiple union elements have errors, and each has a different kind of error. But as mentioned in an inline comment, I'm not a fan of reducing context for the sake of being less verbose in the first place -- I generally want full diagnostics to give me all the context they can.

5) I also feel quite open to consolidating to a single `invalid-call` error code. This also solves all the weirdness around rule codes. But this is mostly because I have a hard time envisioning myself wanting to disable any of the invalid-call diagnostic codes that we have today. I suspect others may have a clearer sense of the real-world use cases for doing that (and I'd love to learn more about them.)

So TL/DR, where do I land? I think my favorite options are (4) or (5), with a very weak preference between them. (That is, if others feel strongly that we should keep fine-grained rule codes for different kinds of call failures, I would go for (4)). But I'm also not sure if the rule-suppression oddities mentioned in 1-3 are actually real problems, so I'm also open to trying out the approach in the current PR.

In any case, thank you @BurntSushi for delving into this complex problem!

---

_Comment by @JelleZijlstra on 2025-05-09 00:47_

> I don't like the separate invalid-union-call diagnostic code, because it seems like something I would never want to suppress or enable or change severity of, on its own. If I don't want to see errors due to wrong argument types in calls, then I don't want to see them inside or outside of unions. I can't imagine ever saying "I don't want to see any diagnostics involving calls to unions, but I still want to see all diagnostics for all other calls." Or vice versa. (That said, I saw @JelleZijlstra mentioned a preference for having this code -- I'd love to hear more about why.)

I think this holds for a lot of error codes that are related to "core" type checking; it doesn't make too much sense either to suppress all errors where you pass an argument of the wrong type, or assign a variable a value of the wrong type. One use case might be if you're gradually introducing typing, and you might want to turn on error codes one at a time.

Ways I think error codes are valuable:

- Keeping suppressions narrow. If I have `f(a, len(b)) # ty: ignore[invalid-union-call]`, I still get errors if e.g. `a` happens to be an undefined name. And if you use a different error code for union calls, you might even still get an error if `b` is not Sized.
- Documenting to readers what you're suppressing. The diagnostic might tell readers there's a union involved.

That said, I think there's several other perfectly good options you could use. (I haven't read the entire discussion in this PR but your comment alludes to a few.) Worth noting that both mypy and pyright actually emit multiple diagnostics for invalid calls to unions, instead of aggregating them in any way.

---

_Comment by @BurntSushi on 2025-05-09 01:05_

Thanks @carljm for the excellent feedback! I want to hone in on where I think our biggest point of disagreement is:

> I think I'm actually pretty open to Micha's suggestion that we should aggregate union call errors by type. So if some variants have invalid-call-argument and others have call-not-callable, you'd get two top-level diagnostics, with codes invalid-call-argument and call-not-callable, each one aggregating the sub-diagnostics for union variants with that issue. This entirely solves the problems with enabling/disabling rule codes. This still allows the union aggregation to provide the context that I find most important, which is just the fact that you tried to call a union, with X Y and Z variants, and these are the ones that had issues (as opposed to just providing the sub-diagnostics directly with zero union context, which I think would be a problem.) What it doesn't allow us to do is reduce the context we present as intelligently. But as mentioned in an inline comment, I'm not a fan of reducing context for the sake of being less verbose in the first place -- I want full diagnostics to give me all the context they can.

I think the thing I am struggling with here is that if you split a union apart into different top-level diagnostics, then it's more difficult to use prose to connect the different union variants when they fail for different reasons. In this PR, you have one top-level diagnostic for an errant function call that talks about a union _cohesively_, and then uses one sub-diagnostic per errant union.

If you instead group the errant union variants by failure type, with each group getting a top-level diagnostic, then each top-level diagnostic has to stand on its own without depending on any others existing. So to be concrete, using one of my examples from one of my initial comments on this PR, we would go from this:

```
error: lint:invalid-union-call: Union type `(def f1() -> int) | (def f2(name: str) -> int)` is not callable because of one or more incompatible variants
 --> main.py:9:9
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |         ^^^^
  |
info: Cannot call union variant `def f1() -> int` because there are too many positional arguments to function `f1`: expected 0, got 1
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^
  |
info: Cannot call union variant `def f2(name: str) -> int` because the argument to this function is incorrect
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^ Expected `str`, found `Literal[3]`
  |
info: `lint:invalid-union-call` is enabled by default
```

to something like this:

```
error: Cannot call union variant `def f1() -> int` because there are too many positional arguments to function `f1`: expected 0, got 1
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^
  |
info: `lint:too-many-positional-arguments` is enabled by default

error: Cannot call union variant `def f2(name: str) -> int` because the argument to this function is incorrect
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^ Expected `str`, found `Literal[3]`
  |
info: `lint:too-many-positional-arguments` is enabled by default
```

I guess to me, this feels overall less clear, because there isn't something giving a cohesive picture that `f(3)` is wrong _because_ the type of `f` is a union of `x | y | ... | z` and "here are the specific problems with it." Instead, it just kind of gets attacked in a piece-meal fashion without prose connecting the ideas.

To me I think the grouping by the same diagnostic lint within the union has a feeling of arbitrariness to me. Like, it feels more like an artifact of _our taxonomy_ than it feels like "this is what a good diagnostic message would look like." (I don't mean to dismiss the benefits of adhering to a taxonomy, but maybe I'm weighting it differently than y'all.)

Maybe there is some worth-smithing to be done that would alleviate my concern. In theory we could also add another top-level diagnostic specifically for the union call as a whole, but I'm not sure if we can easily get an ordering guarantee that it will come first. (Sub-diagnostics are supposed to be what you use when you want to control ordering in the rendered output.) _Or_ we could add a sub-diagnostic to each of the top-level diagnostics like so:

```
error: Cannot call union variant `def f1() -> int` because there are too many positional arguments to function `f1`: expected 0, got 1
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^
  |
info: Union type `(def f1() -> int) | (def f2(name: str) -> int)` is not callable because of one or more incompatible variants
info: `lint:too-many-positional-arguments` is enabled by default

error: Cannot call union variant `def f2(name: str) -> int` because the argument to this function is incorrect
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^ Expected `str`, found `Literal[3]`
  |
info: Union type `(def f1() -> int) | (def f2(name: str) -> int)` is not callable because of one or more incompatible variants
info: `lint:too-many-positional-arguments` is enabled by default
```

> This is cool that we can do this, but from a user perspective I'm not sold on the rationale for it.
> 
> If I'm the user getting a report that my union call failed due to multiple variants failing, what am I going to do? I'm going to look at each failed variant in turn and work out what I need to do to make the call succeed. Why would I want less useful context to help me figure that out, just because there were also errors on another variant?
> 
> Consider the analogy pushed up one layer. We don't say "well, you had too many diagnostics on your project, so to avoid information overload, we are going to switch to concise output mode". What are you going to do with the many diagnostics? You are going to consider each one in turn and try to resolve it. You still want the context for each one in order to do that successfully.
> 
> In both of these cases, the problem with omitting the extra context automatically is that you then don't have any easy way to get the extra context that we decided not to give you. Changing your code to no longer call a union may be quite a significant change.

(This is in regards to emitting extra context for an errant union variant, but only when there is a single errant union variant.)

I think this is a good point. I do somewhat agree with you. I think the problem is that when I didn't have this (and I didn't, initially), the diagnostic felt much noisier and difficult to follow. Instead of "here's an errant union call" and then "here's an errant union variant" and then "here's another errant union variant," you get a bunch of other info messages interspersed between the errant union variants. (Presumably this would get worse as we add more contextual sub-diagnostics to these error cases.)

With that said, I would push back on the analogy here. This isn't just about the _amount_ of information, but how it reads. In the PR as it is now, you get [parallel structure](https://en.wikipedia.org/wiki/Parallelism_(grammar)) within one single top-level diagnostic among its sub-diagnostics. If you add extra sub-diagnostics that don't fit the "errant union variant" pattern, you lose that parallelism. Maybe that parallelism isn't worth it, but it's where my inclination went when I saw the extra sub-diagnostics that didn't follow that pattern. I just overall got disoriented trying to read the information.

I believe rustc also has heuristics for eliding diagnostics when their volume gets too high or too duplicative. But I don't want to lean on that precedent too much here, since I'm not sure that's entirely applicable to this specific case.



---

_Comment by @carljm on 2025-05-09 01:15_

> * Keeping suppressions narrow. If I have `f(a, len(b)) # ty: ignore[invalid-union-call]`, I still get errors if e.g. `a` happens to be an undefined name. And if you use a different error code for union calls, you might even still get an error if `b` is not Sized.

This is a great point. I think I was over-indexing on enabling/disabling as the reason for fine-grained error codes. Ensuring that suppression comments don't accidentally suppress the wrong thing is perhaps even more important, and is a good reason to keep codes more fine-grained.

---

_Comment by @carljm on 2025-05-09 01:27_

@BurntSushi I kind of like the look of your last example, to be honest! Given that mypy and pyright both emit multiple diagnostics and don't even include the union context at all, I wonder if it does make sense to flip our lens upside-down here. The most important thing is why your call was incompatible with a particular union variant -- that is "one problem" to be addressed (whether it is addressed by fixing your call, or by making it so that type is no longer an element of the union). The fact that we are telling you about that union variant because it was a member of a union, which is the type of the thing you tried to call -- that's contextual information to help you understand why you should even care about the type `def f1` in the first place; it's not in itself "the main problem."

I think viewing it this way also makes it pretty natural for each problem to include its own contextual information (like where the target callable is defined), without feeling like you are losing the overall context (since there wouldn't be "overall context" anymore, it would be attached to each diagnostic.)

There is potentially some redundancy here, where for each "problem" we'd repeat the fact that it's part of a union type. But I'm not sure if union calls with many problems are so prevalent that that's a real issue?

---

_Comment by @BurntSushi on 2025-05-09 01:30_

I think I can roll with that. I like that perspective. I'll try flipping this around and see what I come up with. Thanks for pushing back (and @MichaReiser too!). :-)

---

_Comment by @MichaReiser on 2025-05-09 13:10_

I like this a lot. Thanks for exploring more ideas. 

One thing I realised is that this new representation will also work better with editors because editor support for showing sub-diagnostic isn't great. 

---

_Comment by @BurntSushi on 2025-05-09 13:27_

@MichaReiser That's a good point too!

---

_Label `diagnostics` added by @BurntSushi on 2025-05-09 14:13_

---

_Comment by @BurntSushi on 2025-05-09 14:14_

Since doing bottom-up diagnostics was basically a totally different code change, I decided to abandon this PR and open a new one: https://github.com/astral-sh/ruff/pull/17984

---

_Closed by @BurntSushi on 2025-05-09 14:14_

---

_Branch deleted on 2025-05-09 14:14_

---
