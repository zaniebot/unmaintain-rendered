```yaml
number: 17586
title: "[red-knot] improve unsupported bool conversion diagnostics"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/more-diagnostic-improvements
created_at: 2025-04-23T16:36:26Z
updated_at: 2025-05-07T15:21:49Z
url: https://github.com/astral-sh/ruff/pull/17586
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] improve unsupported bool conversion diagnostics

---

_@BurntSushi_

This mostly only improves things for incorrect arguments and for an
incorrect return type on a `__bool__` method. It doesn't do much
to improve the case when `__bool__` isn't callable and leaves the
union/other cases untouched completely.

I picked this one because, at first glance, this _looked_ like a
lower hanging fruit. The conceptual improvement here is pretty
straight-forward: add annotations for relevant data. But it took me a
bit to figure out how to connect all of the pieces.


---

_Review requested from @carljm by @BurntSushi on 2025-04-23 16:36_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-23 16:36_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-23 16:36_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-23 16:36_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-23 16:36_

---

_Comment by @github-actions[bot] on 2025-04-23 16:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unsupported_bool_conversion.md_-_Different_ways_that_`UNSUPPORTED_BOOL_CONVERSION`_can_occur_-_Has_a_`__bool__`_method,_but_has_an_incorrect_return_type.snap`:44 on 2025-04-23 16:47_

I feel like there's a couple of missing steps in the explanation here. We're emitting the diagnostic on the expression `10 and a and True`, and then we're showing them where the `NotBoolable.__bool__` method is defined. But we don't say anywhere why the user should care about the `NotBoolable` class, or why the return method of `NotBoolable.__bool__` is important.

I think we need to explicitly say:
1. `a.__bool__()` is implicitly called due to the use of the `and` operator
2. In order for boolean conversion to succeed, a `__bool__` method must return an instance of `bool`
3. `a` is an instance of `NotBoolable`
4. Here is the `NotBoolable.__bool__` method. It returns `str`, which is not assignable to `bool`

You do step (4) here but you skip steps (1), (2) and (3) in the explanation!

---

_@AlexWaygood reviewed on 2025-04-23 16:48_

Nice!!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/unsupported_bool_conversion.md`:3 on 2025-04-23 17:20_

nit -- let's use the user-facing name of a diagnostic code in mdtests, not the shouty Rust constant name
```suggestion
# Different ways that `unsupported-bool-conversion` can occur
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/snapshots/membership_test.md_-_Comparison___Membership_Test_-_Return_type_that_doesn't_implement_`__bool__`_correctly.snap`:31 on 2025-04-23 17:22_

This can remain a TODO, but really this diagnostic should explain why we are looking at `NotBoolable` in the first place (because we did an `in` operation on `WithContains`, and its `__contains__` returns `NotBoolable`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/snapshots/not.md_-_Unary_not_-_Object_that_implements_`__bool__`_incorrectly.snap`:32 on 2025-04-23 17:23_

In all of these cases, it would be ideal to have an annotation pointing at the not-callable `__bool__` in question (doesn't need to happen in this PR)

---

_Label `red-knot` added by @AlexWaygood on 2025-04-23 17:25_

---

_Label `diagnostics` added by @AlexWaygood on 2025-04-23 17:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unsupported_bool_conversion.md_-_Different_ways_that_`UNSUPPORTED_BOOL_CONVERSION`_can_occur_-_Has_a_`__bool__`_method,_but_has_an_incorrect_return_type.snap`:44 on 2025-04-23 17:32_

I feel like all of 1-3 are pretty well implied by what's here. Pointing at `a` and discussing boolean conversion and the type `NotBoolable` pretty strongly implies both that we have to convert `a` to a boolean, and that it's of type `NotBoolable`. Saying that `str` is an incorrect return type because it's not assignable to `bool` pretty strongly implies that `__bool__` needs to return something assignable to `bool`.

I'm not necessarily opposed to spelling some things out more, but I do feel like what's here is a pretty reasonable level of spelling things out, and there is some inherent cost to added verbosity.

I think it is higher priority to improve some other diagnostics, than it is to make this one more verbose than this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unsupported_bool_conversion.md_-_Different_ways_that_`UNSUPPORTED_BOOL_CONVERSION`_can_occur_-_Part_of_a_union_where_at_least_one_member_has_incorrect_`__bool__`_method.snap`:35 on 2025-04-23 17:42_

In this case I think ideally we would provide the same information about _why_ `NotBoolable1` doesn't implement `__bool__` correctly as we do in the non-union examples above.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:5831 on 2025-04-23 17:49_

Yeah, this has overlap with go-to-definition; it requires adding APIs to not just get a symbol, but get the location(s) of its definition(s), and we don't currently have those APIs. I don't think they are inherently difficult to add over what we have now (the information is all there in the use-def map, it's just a Simple Matter of Programming), but it's a non-trivial amount of Programming.

cc @MichaReiser 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:5812 on 2025-04-23 17:50_

I think one fairly simple thing we could do here to improve this diagnostic (short of the below) would be to name the not-callable type we found for `__bool__`. That alone could really help someone to track down the source of the issue.

---

_@carljm approved on 2025-04-23 17:51_

This looks great! There are definitely areas in these diagnostics for further improvements, but IMO what's here is already clearly better than the status quo, so none of the improvements are blocking.

---

_@carljm reviewed on 2025-04-23 17:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4929 on 2025-04-23 17:53_

This API and `parameter_span` can both cause cross-module dependencies. We already discussed this with the improved invalid-call diagnostic, and I don't think there's anything to be done about that core fact -- it's just a reality of providing nice diagnostics.

I'm not sure if it would be useful to call this out in the doc comment, or via some kind of naming convention? I don't really think it's likely we'd attempt to use these methods unless we really need them for diagnostics, so tbh I'm not sure how much that would benefit us.

---

_@AlexWaygood reviewed on 2025-04-23 17:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unsupported_bool_conversion.md_-_Different_ways_that_`UNSUPPORTED_BOOL_CONVERSION`_can_occur_-_Has_a_`__bool__`_method,_but_has_an_incorrect_return_type.snap`:44 on 2025-04-23 17:59_

> I feel like all of 1-3 are pretty well implied by what's here. Pointing at `a` and discussing boolean conversion and the type `NotBoolable` pretty strongly implies both that we have to convert `a` to a boolean, and that it's of type `NotBoolable`. Saying that `str` is an incorrect return type because it's not assignable to `bool` pretty strongly implies that `__bool__` needs to return something assignable to `bool`.

Even the term "assignable to" is going to be pretty foreign to a newbie in Python typing. And if this type checker is successful, we'll have a whole bunch of users who are pretty new to Python altogether, let alone Python typing. (Mypy certainly does! I've seen many of their issues on the mypy tracker, and I've responded to many of their questions on StackOverflow.)

> I think it is higher priority to improve some other diagnostics, than it is to make this one more verbose than this.

Sure, it doesn't need to block this PR, of course! I also don't think it would take very long at all to address this, though. But I'm happy to park this for now.

---

_Renamed from "[red-knot] improve unsupport bool conversion diagnostics" to "[red-knot] improve unsupported bool conversion diagnostics" by @AlexWaygood on 2025-04-23 18:08_

---

_@MichaReiser reviewed on 2025-04-23 18:33_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:5831 on 2025-04-23 18:33_

I'm on the run so I haven't looked closely at the code but we now have `Type::definition`... in case you have a type pointing to `__bool__` somewhere.

---

_@carljm reviewed on 2025-04-23 18:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:5831 on 2025-04-23 18:54_

That's true, in the common case where `__bool__` is a method, the `FunctionType::definition` ought to point to where it was defined?

EDIT: But in that case we wouldn't be getting this error about it not being callable 😆 

---

_@MichaReiser reviewed on 2025-04-23 19:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:5831 on 2025-04-23 19:18_

There's a method on Type that returns the definition which also works for classes and even literals

---

_@carljm reviewed on 2025-04-23 19:33_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:5831 on 2025-04-23 19:33_

I know, but it goes to the definition of the _type_, which won't be useful for many invalid `__bool__` values. E.g. if we have

```py
class NotBoolable
    __bool__ = "foo"
```

using `Type::definition` would go to the definition of `str` in typeshed, which is not useful; we want to go to the `__bool__ = "foo"` assignment.

This is precisely the same as the distinction between goto-definition and goto-type-definition in the LSP, I think.

---

_@BurntSushi reviewed on 2025-04-24 13:04_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unsupported_bool_conversion.md_-_Different_ways_that_`UNSUPPORTED_BOOL_CONVERSION`_can_occur_-_Has_a_`__bool__`_method,_but_has_an_incorrect_return_type.snap`:44 on 2025-04-24 13:04_

I did consider spelling this out a bit more. And in particular, highlighting the class definition `NotBoolable` itself. It _felt_ like a little too much information, so I biased toward brevity. But I can absolutely believe that spelling it out a bit more could be more beneficial. Perhaps we leave it this way for now until we get a better idea of what verbosity level is ideal.

---

_@BurntSushi reviewed on 2025-04-24 13:26_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/membership_test.md_-_Comparison___Membership_Test_-_Return_type_that_doesn't_implement_`__bool__`_correctly.snap`:31 on 2025-04-24 13:26_

Ah yeah it looks like this is even mentioned in the corresponding mdtest:

``````
TODO: Ideally the message would explain to the user what's wrong. E.g,

```ignore
error: [operator] cannot use `in` operator on object of type `WithContains`
    note: This is because the `in` operator implicitly calls `WithContains.__contains__`, but `WithContains.__contains__` is invalidly defined
    note: `WithContains.__contains__` is invalidly defined because it returns an instance of `NotBoolable`, which cannot be evaluated in a boolean context
    note: `NotBoolable` cannot be evaluated in a boolean context because its `__bool__` attribute is not callable
```
``````

That would be a really nice diagnostic!

---

_@BurntSushi reviewed on 2025-04-24 13:27_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/not.md_-_Unary_not_-_Object_that_implements_`__bool__`_incorrectly.snap`:32 on 2025-04-24 13:27_

Yeah I called this out in the code. I actually tried to do it, but it didn't seem straight-forward. (And I mean that in the sense that it is maybe actually not straight-forward, or more likely, only not straight-forward to _me_ haha.)

---

_@AlexWaygood reviewed on 2025-04-24 13:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/membership_test.md_-_Comparison___Membership_Test_-_Return_type_that_doesn't_implement_`__bool__`_correctly.snap`:31 on 2025-04-24 13:30_

I think I'd prefer to fix that as part of tackling https://github.com/astral-sh/ruff/issues/16371#issuecomment-2682014149. This error should be propagated up so that it's reported as an `[operator]` error rather than a `[not-boolable]` error; that will allow us to provide more details in the diagnostic, but to do that we need to cleanup some of our internals a little bit.

---

_@BurntSushi reviewed on 2025-04-24 14:05_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unsupported_bool_conversion.md_-_Different_ways_that_`UNSUPPORTED_BOOL_CONVERSION`_can_occur_-_Part_of_a_union_where_at_least_one_member_has_incorrect_`__bool__`_method.snap`:35 on 2025-04-24 14:05_

Yeah, 100%. I looked into doing this too, but seemed like more work that I wanted to bite off here. I even specifically wrote this test such that `NotBoolable1` and `NotBoolable3` are both not usable as booleans for different reasons, thinking that I could improve that case. And perhaps even include diagnostics for _all_ (or at least more than 1) parts of a union. There's some refactoring involved so that we re-use the diagnostic generation for each of the base cases, and then some way to figure out how to tie off the recursion of union types themselves.

---

_@BurntSushi reviewed on 2025-04-24 14:19_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types.rs`:5831 on 2025-04-24 14:19_

Yeah this is all pretty much the same conclusion I arrived at. I had indeed found `Type::definition`.

I'm at least happy that I wasn't missing something dumb. :-)

(Totally agree about this being a "simple matter of programming.")

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types.rs`:5812 on 2025-04-24 14:21_

Is that not done in the couple lines above this one? Or am I misunderstanding? That is, I think `not_boolable_type` is the not-callable type, and its name is included in the main diagnostic message.

Or maybe you're saying I should repeat it here? That is, something like:

```
`__bool__` on `{not_boolable_type}` must be callable
```

---

_@BurntSushi reviewed on 2025-04-24 14:21_

---

_@BurntSushi reviewed on 2025-04-24 14:24_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types.rs`:4929 on 2025-04-24 14:24_

I added this as a doc comment to this method and `parameter_span`:

```
    /// # Performance
    ///
    /// Note that this may introduce cross-module
    /// dependencies. This can have an impact on
    /// the effectiveness of incremental caching
    /// and should therefore be used judiciously.
    ///
    /// An example of a good use case is to improve
    /// a diagnostic.
```

I think the cross module dependency thing is really subtle, so a naming convention might actually help here. But yeah, I also don't have a good feel for how much of a problem this will be in practice.

---

_@BurntSushi reviewed on 2025-04-24 15:36_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types.rs`:5812 on 2025-04-24 15:36_

I ended up making that change.

---

_Merged by @BurntSushi on 2025-04-24 15:43_

---

_Closed by @BurntSushi on 2025-04-24 15:43_

---

_Branch deleted on 2025-04-24 15:43_

---
