```yaml
number: 16321
title: "[red-knot] Rewrite `Type::try_iterate()` to improve type inference and diagnostic messages"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/bad-iter-msg-2
created_at: 2025-02-22T22:05:51Z
updated_at: 2025-07-25T18:32:11Z
url: https://github.com/astral-sh/ruff/pull/16321
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Rewrite `Type::try_iterate()` to improve type inference and diagnostic messages

---

_@AlexWaygood_

## Summary

This PR rewrites `Type::try_iterate()` to greatly improve the quality of our type inference for edge cases involving iterables, and greatly improve the quality of our diagnostics. `IterateError` is renamed to `IterationError`, and ~~is expanded to have 10(!) inner variants~~. (Following code review, I've managed to reduce this to 4 inner variants.)

Fixes https://github.com/astral-sh/ruff/issues/16272. Fixes https://github.com/astral-sh/ruff/issues/16123. Helps a lot with astral-sh/ruff#13989.

Note: some of the diagnostic messages are now somewhat verbose. In the long run, I think we could probably change a lot of them to be diagnostics with concise messages but with several notes attached to them.

## Test Plan

Several new mdtests, and lots of diagnostic snapshots

---

_Label `red-knot` added by @AlexWaygood on 2025-02-22 22:05_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-22 22:05_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-22 22:05_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-22 22:05_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2364 on 2025-02-23 09:07_

I'd prefer to keep the error type named `IterateError` because I then don't have to guess what the error type is named: `try_iterate` -> `IterateError`, `try_call` -> `CallError`. 



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2964 on 2025-02-23 09:15_

I'm not sure if retaining all those different errors is the right approach. This is what I called out in my `CallOutcome` refactor that we need to find the right balance between retaining enough detail for diagnostics without making error handling awkward in other cases. 

I think my preference here would be that the diagnostic handler recomputes some of that information rather than that we funnel it all the way through. The argument here isn't necessarily performance but the complexity it introduces when handling an iterate error. You can see this with `CallError` today: Each call site that handles `CallError` needs to match on all variants and define how a call error in that situation should be matched. The more variants, the more complicated these error handlers become. Obviously, we have to retain enough information to distinguish the cases that are relevant for error propagation but we should aim to keep the enum minimal. 

I also expect that we'll need more variants long term and may even need to retain more information per variant to highlight the relevant parts. That's why I'm somewhat leaning towards simply re-doing some of the `iterate` resolution `IterateError`'s diagnostic logic. E.g. what about unions? 

---

_@MichaReiser reviewed on 2025-02-23 09:20_

The error messages look great. Although they're all rather long now. It makes me wonder if now is the right time to improve them or if we should just wait for when we have `notes` (or another mechanism to explain why a certain diagnostic was created) to make the improvement. 

Either way. I'm concerned about having 10 `IterateError` variants. It doesn't seem to scale well and I'm somewhat convinced that we'll have to retain even more information for diagnostics in the future (e.g. the range where the `__getitem__` and `__iterate__` method are defined). 

This makes me believe that we should not distinguish between all those variants in `IterateError` but instead redo some of the `iterate` logic in the `IterateError::report_diagnostic`. 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2964 on 2025-02-23 14:10_

Thank you! I looked at it again and I managed to reduce it from 10 to 4 variants without duplicating any logic anywhere, without sacrificing the precision of type inference, and without sacrificing the precision of any diagnostic messages. This was also a net reduction in code, and simplified `Type::try_iterate()` quite a bit.

---

_@AlexWaygood reviewed on 2025-02-23 14:10_

---

_@AlexWaygood reviewed on 2025-02-23 14:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2364 on 2025-02-23 14:16_

blegh, I much prefer `IterationError` 😄 because "iterate" just can't be used as a [noun adjunct](https://en.wikipedia.org/wiki/Noun_adjunct) in the same way as "call" or "bool" (because it's a verb rather than a noun -- "call" can be both a verb or a noun depending on context, which is why that feels OK to me, but that's not true for "iterate")

I don't feel too strongly though. I can change this back if you strongly dislike the new name, or if others agree with you :-)

---

_Comment by @AlexWaygood on 2025-02-23 14:23_

> The error messages look great.

Thanks!

> Although they're all rather long now. It makes me wonder if now is the right time to improve them or if we should just wait for when we have `notes` (or another mechanism to explain why a certain diagnostic was created) to make the improvement.

Yeah. The main reasons why I wanted to do something now were:
- We have some error messages on `main` that are just flat-out incorrect, and are pretty confusing (https://github.com/astral-sh/ruff/issues/16272)
- We finally _can_ have both type inference that is more precise and diagnostics that are more informative, following your great work this week refacting `Type::call()` and `Type::iterate()`. I wanted to try it out 😄
- I think we will _want_ all this information in our diagnostics. I agree that we probably won't want it _presented_ all in one long sentence in the final version of our diagnostics, but it seems to me that this work will make it easier to migrate to those kinds of diagnostics-with-notes when the time comes -- because the changes I'm making here mean that when the time comes, all the information will be available to us to produce those kinds of diagnostics.

> Either way. I'm concerned about having 10 `IterateError` variants.

I looked again following your comments, and managed to reduce it to 4 variants without sacrificing the precision of our type inference or our diagnostic messages.

---

_@AlexWaygood reviewed on 2025-02-23 14:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2964 on 2025-02-23 14:28_

> Each call site that handles `CallError` needs to match on all variants and define how a call error in that situation should be matched. The more variants, the more complicated these error handlers become.

FWIW: while I absolutely agree that this is a concern, I think it is _less_ of a concern for iteration than it is for calling. The main reason why we need to match on `CallError` everywhere is because so many high-level operations in Python desugar to lots of call operations. Iteration _does_ happen implicitly in lots of places in Python, but the only place I know of where there's a higher-level operation that uses iteration under the hood is the `in` and `not in` operators: if a container doesn't have a `__contains__` method, Python will attempt to iterate over the container to see if the object is (or isn't) in the container.

The TL;DR here is that I think there will overall be many fewer places that we will need to match over the variants here and propagate them into a higher-level diagnostic; it's not nearly as significant a problem as it is for `CallError`.

---

_@MichaReiser reviewed on 2025-02-23 15:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2964 on 2025-02-23 15:18_

> Thank you! I looked at it again and I managed to reduce it from 10 to 4 variants without dup

Nice, I'll have another look and this addresses my concerns :)

I also think that I probably haven't done a good job at this myself with `BoolError`. It's more fine-grained than it probably ought to be. 

> The TL;DR here is that I think there will overall be many fewer places that we will need to match over the variants here and propagate them into a higher-level diagnostic; it's not nearly as significant a problem as it is for CallError.

I totally agree with this. I believe there's only one case inside `member` where we have to deal with `iterate` errors. My motivation for course correcting here is mainly to test and establish certain patterns and it seems you've been very successful at it!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2364 on 2025-02-23 15:20_

It's too much a detail to feel strongly about it but I'm interested in other opinions as well

---

_@MichaReiser reviewed on 2025-02-23 15:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unpacking.md_-_Unpacking_-_Right_hand_side_not_iterable.snap`:25 on 2025-02-23 15:46_

I think I prefer the old message because it's more on the point 
(Overall, messages should be as concise as possible). 

I do think that we want to add a help text saying *because `Literal[1]` doesn't have an `__iter__` method* and *because `Literal[1]` doesn't have a `__getitem__` method* once our diagnostic system supports it.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:108 on 2025-02-23 15:54_

I'm curious to hear your take on this and how you decided to use snapshoting because it's not clear to me when we want to use snapshot tests. I've found updating the messages very painful when working on `unsupported-bool-conversion` but having the assertions (and messages) inline is significantely more readable and avoids bugs slipping through by accepting the snapshot tests. 

I think it's a failure if we start using snapshot tests everywhere (or for a large majority of tests) because the experience is than very close to what we have from Ruff. Instead, we should use them mainly to validate that the diagnostic ranges are correct. 





---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2385 on 2025-02-23 15:58_

do we need the `DunderIter` part or can we omit it similar to `PossiblyUnbound`? Trying to think of ways on how to make this error code shorter.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2964 on 2025-02-23 16:08_

Thanks for iterating on this. I still think that we might be doing too much work in `Type::iterate` that's wasted if we don't end up emitting the diagnostic but I'd have to play with the error variants myself to fully understand if that's the case and you can probably answer this best. I also don't think that this should block merging this PR anymore. 

Ideally, we'd have exactly one error variant that `Type::iterate` returns. That would be the most efficient and easiest to handle by upstream code. It also allows to defer some of the error-probing calls to the diagnostic code path only (and skip them entirely if the diagnostic is turned off). There are two reasons why we might not want to do that:

* We might want to infer a more precise type for the element type even in the error case
* We might have upstream code that calls `iterate` with deviating behavior based on *some condition*




---

_@MichaReiser approved on 2025-02-23 16:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2964 on 2025-02-23 18:19_

> I still think that we might be doing too much work in `Type::iterate` that's wasted if we don't end up emitting the diagnostic but I'd have to play with the error variants myself to fully understand if that's the case and you can probably answer this best.

I don't _think_ that's the case. But if you think you can make all my tests pass while doing less work in `Type::iterate()`, I'd love to see it ;-)

For example, here's one edge case that we don't handle correctly on `main` (it's covered by a test in this PR):

```py
from typing import reveal_type

class Iterator:
    def __next__(self) -> str:
        return "foo"

def test(flag: bool):
    class Foo:
        if flag:
            def __iter__(self) -> Iterator:
                return Iterator()

        def __getitem__(self, key: int) -> bytes:
            return b"42"

    for x in Foo():
        reveal_type(x)
        yield x
```

On `main` we say this:

```
error: lint:not-iterable
  --> /Users/alexw/dev/experiment/foo.py:16:14
   |
14 |             return b"42"
15 |
16 |     for x in Foo():
   |              ^^^^^ Object of type `Foo` is not iterable because its `__iter__` method is possibly unbound
17 |         reveal_type(x)
18 |         yield x
   |

info: revealed-type
  --> /Users/alexw/dev/experiment/foo.py:17:9
   |
16 |     for x in Foo():
17 |         reveal_type(x)
   |         -------------- info: Revealed type is `str`
18 |         yield x
   |
```

But on my branch we now say this:

```
info: revealed-type
  --> /Users/alexw/dev/experiment/foo.py:17:9
   |
16 |     for x in Foo():
17 |         reveal_type(x)
   |         -------------- info: Revealed type is `str | bytes`
18 |         yield x
   |
```

which is much more correct, in my view. Since the `__iter__` method is possibly unbound, we union together the return type of the iterator's `__next__` method and the return type of the `__getitem__` method to infer the type for `x` (rather than just using the return type of the `__next__` method. And we no longer report a diagnostic, which reflects the behaviour you get at runtime, where the `Foo` class is always iterable whether or not `flag` is `True` or `False`:

```pycon
% python -i foo.py                                                                                     
>>> next(test(flag=True))
Runtime type is 'str'
'foo'
>>> next(test(flag=False))
Runtime type is 'bytes'
b'42'
```

But in order to achieve this we do need to continue calling dunders even after we hit the first error (which would be that the `__iter__` method is possibly unbound). We need to check that the `__iter__` method returns a valid iterator class in the event that it is bound, and we need to check that `__getitem__` provides a valid fallback in the event that `__iter__` is unbound. Only then do we have enough information to return `Ok()` from `Type::try_iterate()` rather than `Err()`.

---

_@AlexWaygood reviewed on 2025-02-23 18:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2385 on 2025-02-23 18:26_

yeah, it's too long... I'll shorten it to `PossiblyUnboundIterAndGetitemError`. Still pretty long but it's... better

---

_@AlexWaygood reviewed on 2025-02-23 18:26_

---

_@MichaReiser reviewed on 2025-02-23 18:41_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2964 on 2025-02-23 18:41_

> I don't think that's the case. But if you think you can make all my tests pass while doing less work in Type::iterate(), I'd love to see it ;-)

Hmm, I rather don't. I don't consider it a priority right now and I also think that you should know best. I was mainly trying to explain when I think it's appropriate to add new variants and it's perfect if you've now narrowed it down to exactly those cases

---

_@AlexWaygood reviewed on 2025-02-23 18:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unpacking.md_-_Unpacking_-_Right_hand_side_not_iterable.snap`:25 on 2025-02-23 18:59_

I agree with all that. I actually think for all of these diagnostics, the top-line error message should probably just be either "Object of type <whatever> is not iterable" or "Object of type <whatever> may not be iterable".

Again, though, this PR isn't really about trying to get to _perfect_ diagnostics right now -- its aim is really to distinguish between the different error cases and make sure that we're propagating the necessary information into `IterationError`` so that we can easily turn these into beautifully presented diagnostics when our infrastructure's there.

Overall, I think I'd prefer to leave this as-is for now. Otherwise it's the only `IterationError` diagnostic message that doesn't given an explanation, which seems inconsistent 😄 and I think long-term, the explanation probably shouldn't be part of the top-line message for any of these diagnostics

---

_@AlexWaygood reviewed on 2025-02-23 19:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unpacking.md_-_Unpacking_-_Right_hand_side_not_iterable.snap`:25 on 2025-02-23 19:19_

I added a TODO comment in https://github.com/astral-sh/ruff/pull/16321/commits/df75629989c3d947c5cba5e107d35ec7e78f780d saying that we probably don't want all the information in one long sentence. Though I think the TODO comment also applies to lots of our other diagnostics right now!

---

_@AlexWaygood reviewed on 2025-02-23 21:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:108 on 2025-02-23 21:12_

It's a great question.

Overall I feel like mdtest (as it is currently -- obviously we can keep improving it!) is fantastic in a lot of ways:
- It allows us to write tests very concisely and declaratively
- It's very easy to make lots of assertions on which types are revealed in which cases
- It's very easy to make lots of assertions that "this Python code leads us to emit an error on that line with error code X".

What I think mdtest isn't very well suited to right now is making assertions on the full error message:
- It's very tedious to have to go through all locations and update the asserted error message if you make a change to the error message
- Even if you do make an assertion on the full error message, you don't see the full details of how the diagnostic will be rendered to the user. And as we've been discussing, that seems pretty relevant here, since cramming all the information into one sentence obviously isn't the ideal way of presenting these diagnostics.
- [This one is very fixable] I was pretty surprised that my changes didn't initially lead to any tests failing, even though I had changed a bunch of error messages. That's because currently if the error message is "Object X is not iterable because foo bar baz" and the assertion is `error: [not-iterable] "Object X is not iterable"`, the assertion will pass, since mdtest just checks whether the actual error message _contains_ the asserted errror message (it doesn't check that the asserted error message is a fullmatch).

So using snapshots felt like a good fit for a lot of these tests, where what we really want to test is how the information is reported to the user, and where we expect that the presentation of the diagnostics will continue to change in the future. However, I do agree that it's not ideal:
- It would be so much nicer with inline snapshots; having them far away from the Python snippets makes it much harder to tell whether the snapshot is correct or incorrect
- It's much easier to just blindly accept the snapshot without checking that it's correct
- Our current snapshotting setup for mdtest isn't ideal. `cargo test -p red_knot_python_semantic` stops testing a markdown file as soon as it finds a failing snapshot in that markdown file. That meant that I had to run `cargo test -p red_knot_python_semantic; cargo insta review` several times before all snapshots for `for.md` were updated, which was pretty frustrating.

All told, I'm also not sure that using this much snapshotting is a great idea right now. I'm happy to switch it to standard mdtest assertions on full diagnostic messages if you'd prefer it :-)

---

_@MichaReiser reviewed on 2025-02-24 07:22_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:108 on 2025-02-24 07:22_

Thanks for the detailed explanation. I'd probably create one snapshot test for each unique diagnostic rendering variant. All tests that only enforce type inference feature should not use snaphshot testing. I leave it up to you to assess whether that's the case.

I suspect that @carljm has opinions on this as well ;)



---

_@AlexWaygood reviewed on 2025-02-24 14:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:108 on 2025-02-24 14:24_

> I'd probably create one snapshot test for each unique diagnostic rendering variant.

That makes sense to me. I did a quickish audit and removed a few hundred lines of snapshots. We're pretty much there on this PR now, I think.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:108 on 2025-02-24 18:36_

I don't think I have a lot to add here. As far as the current state, I think I basically agree with what Micha suggests above.

This thread did prompt me to add a few thoughts over on https://github.com/astral-sh/ruff/issues/16255

Regarding messages in `# error:` comments being a "contains" check rather than a "full match", that was intentional, the idea being to reduce update pain by only asserting the critical information that a certain test wants to ensure is present in the message. I'm not sure this works out in practice, though; we almost always use the full message, and it reads pretty oddly if you don't. So I'd be OK changing this to full match.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:444 on 2025-02-24 18:43_

This test does not snapshot diagnostics but also doesn't include a message here; is that intentional?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/snapshots/for.md_-_For_loops_-_Possibly_unbound_`__iter__`_and_bad_`__getitem__`_method.snap`:44 on 2025-02-24 19:02_

Haven't gotten to the implementation yet, but just from reading the tests it's not clear to me why here we get "does not support the old-style iteration protocol" while in another test I saw an error message that said something like "expects a `__getitem__` signature at least as permissive as ...".

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2364 on 2025-02-24 19:07_

`IterateError` sounds wrong to me also, for the grammatical reason @AlexWaygood describes. I don't think I would be likely to make the assumption that the error type would be named precisely the same as the method that can return it. But I also don't feel strongly about it either way, `IterateError` really wouldn't bother me.

---

_@carljm approved on 2025-02-24 19:22_

This looks great to me, thank you! I think the number of variants of `IterationErrorKind` is reasonable, and the current PR does a good job of deferring any extra work that is solely for diagnostic purposes into the path where there definitely is a diagnostic to report (mostly by wrapping the underlying call error in the iteration error, which seems like a good pattern to me.)

---

_@AlexWaygood reviewed on 2025-02-25 05:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:444 on 2025-02-25 05:54_

IIRC the specific reason I added this test was to check that we still inferred `int` (rather than `Unknown`) for `x`, despite the fact that `__iter__` is possibly unbound and `__getitem__` is set to `None`. I think in an early version of this PR, that wasn't true.

This at least warrants me adding some prose to describe the test, though; I agree that this isn't obvious! And it's probably best to include the full error message here anyway, just for consistency with the other tests in this file.

Thanks!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:108 on 2025-02-25 11:39_

> Regarding messages in `# error:` comments being a "contains" check rather than a "full match", that was intentional, the idea being to reduce update pain by only asserting the critical information that a certain test wants to ensure is present in the message. I'm not sure this works out in practice, though; we almost always use the full message, and it reads pretty oddly if you don't. So I'd be OK changing this to full match.

Yeah, I remember the reasons for the decision! But I also agree that it hasn't proven useful in practice. And as I say, it actually _confused_ me briefly while working on this PR -- so I'd also favour changing it now to require a full match.

---

_@AlexWaygood reviewed on 2025-02-25 11:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:444 on 2025-02-25 11:41_

Yes, for reference, I added it as a regression test for the change in this commit: https://github.com/AlexWaygood/ruff/commit/243164c1f8e3fc681cb93fd3c1f507d41d08c8d0

---

_@AlexWaygood reviewed on 2025-02-25 11:41_

---

_@AlexWaygood reviewed on 2025-02-25 13:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/for.md_-_For_loops_-_Possibly_unbound_`__iter__`_and_bad_`__getitem__`_method.snap`:44 on 2025-02-25 13:57_

It's because I was being lazy about `CallError::Union(_)` variants. Fixed it now.

---

_Merged by @AlexWaygood on 2025-02-25 14:02_

---

_Closed by @AlexWaygood on 2025-02-25 14:02_

---

_@AlexWaygood reviewed on 2025-02-28 11:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/loops/for.md`:108 on 2025-02-28 11:40_

Hmm, I tried out making this change and a surprising number of tests failed. I guess we use this feature more than I thought.

It's probably worth revisiting this once we have support for subdiagnostics/notes. Once those are implemented, we'll generally have shorter summary lines in our diagnostics, so it'll be less cumbersome to enforce a fullmatch on the summary line as part of mdtest.

---

_Branch deleted on 2025-07-25 18:32_

---
