```yaml
number: 15474
title: "[red-knot] Initial tests for instance attributes"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/instance-attributes
created_at: 2025-01-14T11:13:52Z
updated_at: 2025-01-15T17:39:44Z
url: https://github.com/astral-sh/ruff/pull/15474
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Initial tests for instance attributes

---

_@sharkdp_

## Summary

Adds some initial tests for class and instance attributes, mostly to document (and discuss) what we want to support eventually. **These tests are not exhaustive yet**. The idea is to specify the coarse-grained behavior first.

Things that we'll eventually want to test:

- Interplay with inheritance
- Support `Final` in addition to `ClassVar`
- Specific tests for `ClassVar`, like making sure that we support things like `x: Annotated[ClassVar[int], "metadata"]`
- … or making sure that we raise an error here:
  ```py
  class Foo:
      def __init__(self):
          self.x: ClassVar[str] = "x"
  ```
- Add tests for `__new__` in addition to the tests for `__init__`
- Add tests that show that we use the union of types if multiple methods define the symbol with different types
- Make sure that diagnostics are raised if, e.g., the inferred type of an assignment within a method does not match the declared type in the class body.
- https://github.com/astral-sh/ruff/pull/15474#discussion_r1916556284
- Method calls are completely left out for now.
- Same for `@property`
- … and the descriptor protocol

## Test Plan

New Markdown tests

---

_Label `red-knot` added by @sharkdp on 2025-01-14 11:13_

---

_Comment by @github-actions[bot] on 2025-01-14 11:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `testing` added by @sharkdp on 2025-01-15 12:18_

---

_Marked ready for review by @sharkdp on 2025-01-15 12:21_

---

_Review requested from @carljm by @sharkdp on 2025-01-15 12:21_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-15 12:21_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-15 12:21_

---

_@sharkdp reviewed on 2025-01-15 12:23_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/unsupported_type_qualifiers.md`:9 on 2025-01-15 12:23_

I removed `ClassVar` as "unsupported" here, but left the `invalid-base` test below, as it does not have an analog yet.

---

_@sharkdp reviewed on 2025-01-15 12:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:5350 on 2025-01-15 12:24_

This is obviously not the full `ClassVar` support, but it seems reasonable to already do the trivial type-inference part of it.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:24 on 2025-01-15 12:24_

For completeness, maybe you could also have an `__init__` instance variable that is explicitly declared on assignment and is bound to a value?

```suggestion
        self.pure_instance_variable3: bytes
        self.pure_instance_variable4: bytes = b"Declared and bound"
```

I suppose it might also be good to have an instance attribute that's possible unbound as well, e.g.

```py
class C:
    def __init__(self, flag: bool):
        if flag:
            self.x: str = "oh no"
```

Not sure if we should emit an error if a possibly unbound instance attribute is accessed on an instance -- I think probably not; my instinct is that this would have a large number of false positives.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5350 on 2025-01-15 12:28_

Per @carljm's comment in my Notion doc, though, we might actually want to "strip away" the `ClassVar` part of this in `infer_annotation_expression`, so that it's actually not possible for us to get a `ClassVar` inside `infer_type_expression` and the "lower" methods from that (such as this one).

Having said that, I think you are correct that this change will reduce false positives in the short term, and probably doesn't do any harm right now

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:11 on 2025-01-15 12:30_

```suggestion
Variables only defined in `__init__` are pure instance variables. They cannot be accessed on the
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:103 on 2025-01-15 12:32_

Not saying we shouldn't but this will require traversing the entire class body. This could be somewhat expensive (at least it's more expensive than only doing so for `__init__`). 

* Does that include accesses inside of nested classes, functions or lambdas? 
* Does this include accesses where the variable isn't self


```py
class C:
	pure_instance_variable: str
	
	def test(self, other: Self, value: str):
		other.pure_instance_variable = value
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:42 on 2025-01-15 12:33_

```suggestion
# TODO: this should be an error (pure instance variables cannot be overwritten on the class)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:66 on 2025-01-15 12:33_

```suggestion
# TODO: should ideally be `Literal["value set on instance"]`
# (due to earlier assignment of the attribute from the global scope)
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:194 on 2025-01-15 12:34_

What's the expected behavior if a variable is used both in a `@classmethod` and a regular method? 

```py
class C:
	@classmethod
	def class_method(cls):
		cls.a = "test"

	def instance_method(self):
		self.a = "test"
```

---

_@sharkdp reviewed on 2025-01-15 12:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:173 on 2025-01-15 12:35_

Wording stolen from @AlexWaygood 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:186 on 2025-01-15 12:35_

Should this be an error because the documentation only mentions that *reading the value from the class body is also permitted*?

---

_@MichaReiser reviewed on 2025-01-15 12:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:173 on 2025-01-15 12:36_

Though @carljm pointed out that the word "binding" here might be better than "definition"

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:64 on 2025-01-15 12:37_

I think the explicit declaration in the class body here makes it unambiguous that the public type (the type seen from other scopes) should be `str` rather than a `Literal` type

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:103 on 2025-01-15 12:41_

Mypy and pyright both support this, so I think we have to, expensive though it may be. Users will expect their type checker to support this and will complain if it doesn't. It's also unfortunately reasonably common in Python to do things like

```py
class Foo:
    def __init__(self):
        self.initialize_x_variable()

    def initialize_x_variable(self):
        # many lines of complicated logic
        self.x = 42
```

> * Does that include accesses inside of nested classes, functions or lambdas?
> 
> * Does this include accesses where the variable isn't self

I think we probably don't need to support these, certainly not initially

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:194 on 2025-01-15 12:42_

I think it would be reasonable in that case to complain about ambiguity if we're in strict mode, and demand an explicit annotation in the class body.

If strict mode is disabled, I guess we have to assume it's an instance variable with a class-level default?

---

_@AlexWaygood reviewed on 2025-01-15 12:43_

not a full review yet, but gotta go for lunch!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:194 on 2025-01-15 12:51_

The current CLI proposal doesn't propose a strict mode. But it could be an off-by-default rule that warns about this.

---

_@MichaReiser reviewed on 2025-01-15 12:51_

---

_@AlexWaygood reviewed on 2025-01-15 12:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:103 on 2025-01-15 12:57_

We can also experiment with varying degrees of laziness here. For first-party code that we're actually checking, I think we'll probably need to know all instance attributes of every class. But for third-party/stdlib code that our first-party code is interacting with, it often probably isn't necessary to know what instance attributes the class has and, even if it is, it might not be necessary to materialize the _full set_ of instance attributes the class has. For something like the following, we could plausibly short-circuit if we find the `foo` attribute defined in `__init__` or `__new__`, and not bother analyzing the other methods the `SomeClass` defines:

```py
from third_party import SomeClass

x = SomeClass()
print(x.foo + 5)  # all we need to know here is whether `SomeClass` has a `foo` attribute
                  # and what its type is
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:110 on 2025-01-15 12:59_

"cannot be accessed on instances" isn't correct -- they can be _read_ on instances but not _written to_ on instances; they can only be _written to_ on the class

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:118 on 2025-01-15 13:01_

ahh... a test that actually passes already 🎉 😆

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:148 on 2025-01-15 13:03_

you could also possibly include an example of a `ClassVar` attribute being overridden on a subclass with a consistent subtype, since this is one of the ways in which `ClassVar` class variables differ from `Final` class variables

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:151 on 2025-01-15 13:04_

(I vote for pyright's behaviour here, especially if we infer instance attributes even when they're assigned in non-`__init__`/`__new__` methods)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:169 on 2025-01-15 13:09_

I think this subheading is a bit confusing -- these aren't class variables at all; they're instance variables that just happen to have a default in the class body. Though it's a bit wordier, I'd go with

```suggestion
### Instance variables with class-level default values
```

or

```suggestion
### Instance variables with default values in the class body
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:186 on 2025-01-15 13:18_

For this specific example, I think so, yes. The `: str` annotation in the class body explicitly declares that this is an instance attribute rather than a class attribute, so it shouldn't be writable from the class, only from instances. The only way it should differ from "pure" instance variables is that it's readable from the class as well as from instances.

Where it gets muddier is if the variable is unannotated. What do we do for something like this?

```py
class Foo:
    X = 42

Foo.X = 56
```

To a human who can see the whole code holistically (including how it's used from outside the class scope), it's pretty clear here that `Foo.X` is meant to be a class variable. But red-knot won't be able to see the uses of the variable from outside the class when it's doing its initial analysis of the set of instance and class attributes, and there's no annotation here to help us figure out whether this should be inferred as a class variable or an instance variable with a class-level default. Nor are there any uses of the variable in the class's classmethods or instance methods to help us out.

I don't have a great answer for this right now. Curious if anybody else has thoughts for how we could avoid false positives here.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:179 on 2025-01-15 13:19_

same as my comment above about the subheading, I think calling this variable `regular_class_variable` is somewhat confusing, as this category really has more in common with "pure" instance variables than with pure class variables

---

_@AlexWaygood reviewed on 2025-01-15 13:25_

Great, this is fantastic! Good call on leaving out `Final` for now.

Two notable things that you haven't covered are that we need to infer an instance variable for any declaration in the class body, even if there is no binding for the variable in any method at all. We could have a disabled-by-default rule that warns if you try to access the variable when it's not bound in any method, but pyright's experience with such a rule has been very mixed; it's hard to get it right without lots of false positives:

```py
class Foo:
    x: int
```

and we need to infer an instance variable for a variable that is defined in a non-`__init__` variable but _not_ declared in a class body:

```py
class Foo:
    def __init__(self):
        self.initialize_x()

    def initialize_x(self):
        self.x = 42  # TODO: should the attribute have `Unknown`, `int`, `Literal[42]` or `Unknown | Literal[42]`
                     # from the perspective of code from other scopes?
```

---

_@sharkdp reviewed on 2025-01-15 14:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:169 on 2025-01-15 14:01_

"Regular class variable" is the term used in pyright's docu. I'm okay with deviating from that. I agree that it's slightly confusing.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:29 on 2025-01-15 14:03_

```suggestion
        # possibly undeclared/unbound
```

---

_@sharkdp reviewed on 2025-01-15 14:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:194 on 2025-01-15 14:04_

> What's the expected behavior if a variable is used both in a `@classmethod` and a regular method?

I'll leave this test case out for now, but I added a note in the PR description (which will eventually serve as a TODO list for me)

---

_@sharkdp reviewed on 2025-01-15 14:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:186 on 2025-01-15 14:07_

> Should this be an error because the documentation only mentions that _reading the value from the class body is also permitted_?

Yes. I removed this method for now. I think the behavior should be the same as from outside the class, so this should be an error.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:148 on 2025-01-15 14:07_

one more case that might be worth adding is the case where a variable is annotated with "bare" `ClassVar` -- here we know that it's a pure class variable, but we still have to infer the type!

```py
class Foo:
    X: ClassVar = 42  # `int`, `Unknown`, `Literal[42]`, or `Unknown | Literal[42]`?
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:131 on 2025-01-15 14:08_

```suggestion
cannot be overwritten on instances, but can be read from instances.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:487 on 2025-01-15 14:12_

is it also worth linking to https://typing.readthedocs.io/en/latest/spec/class-compat.html here?

---

_@AlexWaygood approved on 2025-01-15 14:14_

Looks great -- though I'd still personally add the cases I mentioned in https://github.com/astral-sh/ruff/pull/15474#pullrequestreview-2552685457 prior to landing

---

_Comment by @AlexWaygood on 2025-01-15 14:20_

Depending on how exhaustive you're looking to be for `ClassVar` tests here, you could also consider adding this test, on which we should emit a diagnostic:

```py
class Foo:
    def __init__(self):
        self.x: ClassVar[str] = "x"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:493 on 2025-01-15 14:26_

nit: this actually links to the spec (docs for folks writing type checkers -- that's us!) rather than docs that are meant to be user-facing. (I think that's the correct page for us to link to here but "typing documentation on `classvar`" is maybe a slightly confusing way to describe the page?)

---

_Comment by @sharkdp on 2025-01-15 14:26_

> though I'd still personally add the cases I mentioned in [#15474 (review)](https://github.com/astral-sh/ruff/pull/15474#pullrequestreview-2552685457) prior to landing

I was planning to, you are too fast Alex :smile:. I think those should be covered now. I actually changed a previous test to cover the "not declared in body, only bound in non-`__init__` method" case (by removing the declaration from the body). This should only make the test more general, I think. But again, I am planning to increase the overall test coverage eventually. This is mostly to get the initial design right. Thank you for the detailed review and your help!

---

_Comment by @sharkdp on 2025-01-15 14:27_

> Depending on how exhaustive you're looking to be for `ClassVar` tests here, you could also consider adding this test

I'll add it to the list in the PR description.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:166 on 2025-01-15 14:28_

(valid answers could also include `int`, `Unknown`, `Unknown | Literal[1]` and `Unknown | int`...)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:108 on 2025-01-15 14:31_

nit: I'd either remove the annotation, or also add an example without an annotation, because otherwise I think this could be seen to imply that we intend to only infer the instance attribute on non-`__init__`-method assignments if the attribute is declared as well as bound in the non-`__init__`-method

---

_@AlexWaygood approved on 2025-01-15 14:31_

---

_@sharkdp reviewed on 2025-01-15 14:36_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:493 on 2025-01-15 14:36_

Ahrg, I had "typing spec on …" first, then changed it to documentation after I saw the title of the page. Changing back. The `classvar` thing is done by mdformat. I actually wrote `ClassVar`, but that get's lowercased.

---

_Merged by @sharkdp on 2025-01-15 14:43_

---

_Closed by @sharkdp on 2025-01-15 14:43_

---

_Branch deleted on 2025-01-15 14:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:35 on 2025-01-15 17:10_

This is an interesting question. Both pyright and mypy have heuristics to widen literal types when they think it's "probably more useful." This seems a bit unprincipled to me, but I'm open to the possibility that we'll also need to do it. I guess my question is, if the only assignment to `self.pure_instance_variable1` that occurs anywhere in the class is `self.pure_instance_variable1 = "value set in __init__"`, then why _wouldn't_ you want it typed as `Literal["value set in __init__"]`? Just to accommodate external code setting it to some other value? That seems like an edge case, and not too much to ask you to add an explicit `: str` if that's what you wanted.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:52 on 2025-01-15 17:11_

Depending on how we answer the above question, arguably this should be an error

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:68 on 2025-01-15 17:14_

I think there is some question about the extent to which we'll want to do this narrowing. It's unsound in general (because we can't really say with any certainty what happens to `c_instance` between that assignment and this check), and particularly likely to be unsound in global scope. But we'll probably need to do some of it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:113 on 2025-01-15 17:19_

Probably the even more realistic example would have this method called from `__init__`, but if we do that in the test it raises more questions about whether we're trying to detect that call...

---

_@carljm reviewed on 2025-01-15 17:22_

Nice!

---

_@AlexWaygood reviewed on 2025-01-15 17:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:35 on 2025-01-15 17:34_

> if the only assignment to `self.pure_instance_variable1` that occurs anywhere in the class is `self.pure_instance_variable1 = "value set in __init__"`, then why _wouldn't_ you want it typed as `Literal["value set in __init__"]`? Just to accommodate external code setting it to some other value?

Subclasses also won't be able to assign a type to the attribute unless the type is assignable to the `Literal[...]` type

> That seems like an edge case

Hmm, I don't agree. I think it's quite common for code to only assign an instance variable in the `__init__` method of the class, but with the intention that it should be possible to reassign the attribute from other scopes.

> and not too much to ask you to add an explicit `: str` if that's what you wanted.

Surely that violates the gradual guarantee? And this doesn't seem to tackle at all the problem of typed code interacting with untyped third-party code, which may be unwilling to add type annotations in a timely manner?

I'm leaning towards either `Unknown | Literal[...]` or `Unknown | str` for cases like these. Haven't made my mind up about which is preferable there (haven't thought too deeply about it yet!)

---

_@AlexWaygood reviewed on 2025-01-15 17:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:68 on 2025-01-15 17:37_

We've had this conversation several times now 😆 I still favour the more pragmatic behaviour of mypy and pyright over the stricter behaviour of pyre here. But yeah, we can debate the details at a later stage

---

_@carljm reviewed on 2025-01-15 17:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:35 on 2025-01-15 17:39_

> Surely that violates the gradual guarantee

Yes, but so does widening the literal type to `str`. That's what I mean by "unprincipled" -- it doesn't fundamentally change anything, just arbitrarily chooses some wider type because we think it's "probably what you meant."

I like the idea of a union with `Unknown` for inferred attribute types! I think that is the principled gradual-guarantee approach here. It will be more forgiving than what people are used to, but I generally like it if we take the gradual guarantee more seriously than existing type checkers: if you want more strictness, annotate.

(I don't see why we would special-case converting it to `Unknown | str` instead of `Unknown | Literal[...]` if we're unioning with `Unknown` -- I think the union already takes care of the problematic aspects of the literal inference, by allowing you to assign anything.)

---
