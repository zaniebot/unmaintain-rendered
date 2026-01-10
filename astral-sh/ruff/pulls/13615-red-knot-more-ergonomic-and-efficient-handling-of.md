```yaml
number: 13615
title: "[red-knot] more ergonomic and efficient handling of known builtin classes"
type: pull_request
state: merged
author: Slyces
labels:
  - ty
assignees: []
merged: true
base: main
head: feat/builtins-enum
created_at: 2024-10-03T17:37:02Z
updated_at: 2024-10-07T08:40:58Z
url: https://github.com/astral-sh/ruff/pull/13615
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] more ergonomic and efficient handling of known builtin classes

---

_Pull request opened by @Slyces on 2024-10-03 17:37_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR introduces a new enumeration `BuiltinType` that serves 2 purposes:
- df16f218aaa40f319fc6dd3eaddefdc7c3b5bfb0: give a common syntax for the convenience shortcuts to get a builtin instance
  - For that purpose, the enum doesn't need to be exhaustive - just to have the types we often need
-  c98ce748f74841eabe73eb1f4dbc77f83454e5f0: save if a class is a builtin on creation, to save time on call when we need to check for custom behaviour (`str(...)`, `bool(...)`, ...)

I think for the first purpose that's mainly a syntax preference, you should be able to tell fairly fast if you prefer it that way.
For the second, that's mainly an optimisation that we might need once we handle more specific behaviour for `builtin` used as callable - this could clearly wait until we implement more of those.

## Test Plan

No tests were added, but the existing test suite is enough to check if this introduced a regression.


---

_Review requested from @carljm by @Slyces on 2024-10-03 17:37_

---

_Review requested from @MichaReiser by @Slyces on 2024-10-03 17:37_

---

_Review requested from @AlexWaygood by @Slyces on 2024-10-03 17:37_

---

_Comment by @github-actions[bot] on 2024-10-03 17:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-10-04 04:04_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:912 on 2024-10-04 04:04_

What's the reason for introducing and using `BuiltinType::iter` here instead of a match. I think I'd prefer to avoid adding a dependency for this use case as it feels a bit overkill :)

---

_@Slyces reviewed on 2024-10-04 08:11_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:912 on 2024-10-04 08:11_

I guess the reason was to make it easier to add more members as needed - but that's probably not worth a dependency!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:829 on 2024-10-04 17:25_

```suggestion
/// Feel free to expand this enum if you ever find yourself using the same builtin type in multiple
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:862 on 2024-10-04 17:26_

maybe

```suggestion
        self.to_class(db).to_instance(db)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:419 on 2024-10-04 17:31_

Can we add a method on `ClassType` to make this less verbose? I would probably even call it `is_builtin`:
```suggestion
                if class.is_builtin(db, BuiltinType::Int) =>
```

And then rename the `is_builtin` field to just `builtin`; `is_` prefix suggests a boolean, not an optional enum.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1283 on 2024-10-04 17:46_

As mentioned above, I'd name this just `builtin` -- `is_` prefix suggests a boolean.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:832 on 2024-10-04 17:54_

Naming nit: let's rename this to `BuiltinClass`. This actually only handles classes, not all kinds of types (functions are types too).

Speaking of which, I kinda want to unify this PR and `FunctionKind`, in terms of both naming and API. The one trick there is that we want to represent known functions that aren't actually builtins (e.g. `reveal_type`). The same may be true in future for other stdlib classes!

So maybe ultimately what we'll actually want is `KnownClass` and `KnownFunction` enums, and a `known` field (which is an `Option<KnownClass>` or `Option<KnownFunction>` - I like this Option approach better than `FunctionKind::Ordinary`), and `is_known` method, on both `ClassType` and `FunctionType`. But this would require generalizing what you have in this PR to not assume builtins module, and instead classify the known functions/classes by module as well. I would be happy with doing all of this now, in this PR, or waiting on it if you prefer to land a simpler version of this PR for now.

cc @AlexWaygood in case you hate my naming/API preferences :)

---

_@carljm reviewed on 2024-10-04 17:57_

This looks great as a general direction! A few nits and thoughts on naming/API.

---

_Label `red-knot` added by @carljm on 2024-10-04 18:03_

---

_Renamed from "Feat/builtins enum" to "[red-knot] more ergonomic and efficient handling of known builtin classes" by @carljm on 2024-10-04 18:03_

---

_@carljm reviewed on 2024-10-04 18:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1288 on 2024-10-04 18:06_

I don't like the name of this method currently because it implies that if it returns `None` the class is not a builtin. But that's not necessarily true, since our list of known builtins isn't exhaustive. I'd prefer `maybe_known_builtin`. Or just `maybe_known` if we go with the broader refactor/rename to handle any known class, even if it's not a builtin.

---

_@AlexWaygood reviewed on 2024-10-04 18:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:832 on 2024-10-04 18:10_

Hmm, I considered the pros and cons of `Option<KnownFunction>` vs having `FunctionKind::Ordinary` while working on my previous PR. I ended up going with `FunctionKind::Ordinary` because I have a general preference having a flat list of possible states rather than representing possible states using an enum of enums. It makes it easier to count exactly how many possible states there are, and giving the default state a name (`Ordinary`) rather than using `None` makes it abundantly clear to new readers of the code what the default state represents. It's also more ergonomic to pattern-match on a flat list rather than a nested enum.

I don't feel _too_ strongly, but a flat enum would be my preference :)

---

_@carljm reviewed on 2024-10-04 18:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:832 on 2024-10-04 18:16_

I think clearer naming is the primary reason I prefer `Option<KnownFunction>`. I don't like the name `FunctionKind` because it is so unnecessarily generic -- it doesn't clarify anything at all about what characteristic(s) of the function we are actually talking about. What happens when we later need to categorize functions according to some totally different cross-cutting categorization?

It seems clearer to me to have `known = Some(KnownFunction)` mean it's a known function (and then the enum specifies which one), and `known = None` to mean "not a known function." I suppose we could still use the `KnownFunction` name and have `KnownFunction::NotKnown` or `KnownFunction::None` -- it just seems weird to me to have a variant of `KnownFunction` that means... not a known function.

---

_@AlexWaygood reviewed on 2024-10-04 18:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:832 on 2024-10-04 18:22_

Yes, you're definitely right that `FunctionKind` is not a great name. And I think you're right that renaming it to `KnownFunction` addresses some of the concern I had that it wouldn't be obvious what the default state signified. When it's called `KnownFunction`, it's pretty obvious that `None` indicates that it's not a known function!

I'm still not a massive fan of nested enums, but I guess in this case it's _okay_. Feel free to proceed :)

---

_@Slyces reviewed on 2024-10-04 19:32_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:832 on 2024-10-04 19:32_

Would you like me to address this naming change in this PR?

---

_@Slyces reviewed on 2024-10-04 19:33_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:1288 on 2024-10-04 19:33_

Completely agree. I think I like this `known` prefix - what would you think of also adding it to the type we store internally?
I'm overall afraid that a new reader would come - see a builtin field - and trust that it covers all existing builtins

Edit: realised an above comment goes that way

---

_@carljm reviewed on 2024-10-04 19:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:832 on 2024-10-04 19:38_

Yes, if you're up for it I think it would be ideal to go ahead and make the switch to both `KnownClass` and `KnownFunction` enums in this PR, with `ClassType` having a field `known: Option<KnownClass>` and a method `is_known`, and similar for `FunctionType`. Where `KnownFunction` would replace the current `FunctionKind` enum, and the new `FunctionType::known` field would replace the current `FunctionType::kind`.

---

_@carljm reviewed on 2024-10-04 19:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1288 on 2024-10-04 19:39_

Agreed; I think this is a good enough reason to go ahead and fully make the change to "Known" terminology in this PR, as described in the other comment thread.

---

_@Slyces reviewed on 2024-10-04 22:06_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:832 on 2024-10-04 22:06_

The newest commit implements a first attempt at this. I did three things:
- Apply naming convention for `KnownFunction`
- Apply naming convention for `KnownClass`
- Try to extend the structure in `KnownClass` to easily support non-builtins

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:419 on 2024-10-05 05:20_

Is there a reason you went for `is_known_class` rather than `is_known` for this method? It's ok either way, just feels like an unnecessary level of repetition of the word "class" when we have `class.is_known_class(db, KnownClass::...)`

---

_@carljm approved on 2024-10-05 05:21_

One question about method naming, otherwise this looks excellent, thank you!

---

_@Slyces reviewed on 2024-10-05 07:18_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:419 on 2024-10-05 07:18_

No reason, is known is good!

---

_@Slyces reviewed on 2024-10-05 07:40_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:419 on 2024-10-05 07:40_

@carljm should be fixed!

---

_@AlexWaygood reviewed on 2024-10-05 10:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:365 on 2024-10-05 10:20_

Ah. @carljm, I think this might actually reveal a bug in `main`. The union of `Literal[True] | Literal[False]` should surely simplify to `Instance("bool")` rather than `Class("bool")`. If an object has type `Literal[True]`, it indicates that it the object is an instance of `bool`, not that it is the `bool` class itself.

The fact that the new idiom in this PR makes this bug more obvious is definitely a point in favour of us landing it @Slyces!!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:758 on 2024-10-05 10:24_

Nit: I'd probably add the other "fundamental" types in this list (`types.ModuleType`, `types.FunctionType`, `_typeshed.NoneType` and `builtins.type`) to the `KnownClass` enum as well, and take `typing.Any` out of the enum for now 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:902 on 2024-10-05 10:40_

this feels a bit repetitive, and it also feels like we're doing some work that isn't always necessary depending on what `self` is. Maybe something like this?

```suggestion
        let is_stdlib_module = module.search_path().is_standard_library();
        let module_name = module.name().as_str();
        match self {
            Self::Any => is_stdlib_module && matches!(module_name, "typing" | "typing_extensions")
            _ => is_stdlib_module && module_name == "builtins",
        }
```

I'd actually also prefer it if we tediously enumerated all the builtin classes rather than using `_` as a fallback branch. If we do it that way then the compiler will warn us when we add a new non-builtins variant but forget to update this method, which would lead to incorrect results.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:890 on 2024-10-05 10:43_

I wish Rust had some way of enforcing that the r.h.s. of a match statement is meant to be exhaustive :/ I guess that's what you were going for with your initial `EnumIter` solution from `strum`. But I agree with Dhruv that it's probably not really worth an extra dependency.

If this enum continues to grow and this becomes a maintenance problem, I guess we could consider writing a macro to generate this method, which would allow us to ensure that the r.h.s. of the `match` is exhaustive. But that comes with its own tradeoffs, of course.

---

_@AlexWaygood reviewed on 2024-10-05 10:43_

Seems fine to me other than a couple of nits!

---

_@Slyces reviewed on 2024-10-05 13:07_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:902 on 2024-10-05 13:07_

Actually we could also early return if we are not in stdlib as all cases in the close future would be in stdlib 

---

_@Slyces reviewed on 2024-10-05 13:40_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/builder.rs`:365 on 2024-10-05 13:40_

Great catch, definitely didn't see it myself!

---

_@AlexWaygood reviewed on 2024-10-05 13:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:365 on 2024-10-05 13:43_

(It doesn't need to be fixed in this PR, since I guess it'll require other changes elsewhere that your PR currently doesn't touch! Though it might actually be a pretty small change — if so, it's probably okay to fix it here if you feel like it 😆)

---

_@Slyces reviewed on 2024-10-05 15:55_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:758 on 2024-10-05 15:55_

Why is it `_typeshed.NoneType`? In my repl I see the `type(None)` from builtins?

---

_@carljm reviewed on 2024-10-05 15:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:758 on 2024-10-05 15:58_

The type of `None` (`NoneType`) is not exposed in the stdlib at all in some versions of Python, and is never exposed in builtins. The `None` singleton object is in builtins, but typeshed needs a place to give the definition of the _type_. 

---

_@Slyces reviewed on 2024-10-05 16:02_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:890 on 2024-10-05 16:02_

I agree, and indeed this is where my mind went first when I introduced `strum`: make sure that extending the enum is as painless as possible. I'm dropping a `// Note` in there for future reference. Also found a stackoverflow exploring exactly this limitation.

---

_@Slyces reviewed on 2024-10-05 16:03_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:758 on 2024-10-05 16:03_

Would you have a reference or strategy (maybe to drop in the comment of the enum) to find out where a given common symbol is defined?

---

_@AlexWaygood reviewed on 2024-10-05 16:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:758 on 2024-10-05 16:03_

The [`_typeshed` module](https://github.com/python/typeshed/tree/main/stdlib/_typeshed) is a fictitious module that typeshed pretends is part of the standard library. Because typeshed pretends it's part of the stdlib, and we treat typeshed's stubs as the sole source of truth for the stdlib, we think it's part of the stdlib too!

The purpose of the `_typeshed` module is twofold:
- to define various convenient aliases and protocols that don't exist at runtime but which are used extensively in typeshed's stubs for the stdlib. These need to be defined in a stdlib module, because typeshed's stubs for the stdlib can't depend on any non-stdlib stubs.
- to define certain compatibility workarounds for type checkers (that's us!) to use. `_typeshed.NoneType` [is an example of this](https://github.com/python/typeshed/blob/fc611d94c8edf225df33c8ccf66a62e6f4894679/stdlib/_typeshed/__init__.pyi#L304-L311). At runtime, the type of `None` is `types.NoneType`, but `types.NoneType` was [only added in 3.10](https://docs.python.org/3/library/types.html#types.NoneType). We can't use that as the type of `None`, as we still support Python 3.8 and 3.9, and all other type checkers also have this problem -- so typeshed defines `_typeshed.NoneType` as a little compatibility hack for us.

---

_@Slyces reviewed on 2024-10-05 16:06_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/builder.rs`:365 on 2024-10-05 16:06_

Might prefer a separate PR if that's ok!

---

_@Slyces reviewed on 2024-10-05 16:08_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/builder.rs`:365 on 2024-10-05 16:08_

https://github.com/astral-sh/ruff/issues/13642

---

_@Slyces reviewed on 2024-10-05 16:13_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:758 on 2024-10-05 16:13_

So, small question on the topic. I see that we have methods to lookup `typing_extensions`, but nothing from `typing` 

---

_@AlexWaygood reviewed on 2024-10-05 16:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:758 on 2024-10-05 16:13_

> Would you have a reference or strategy (maybe to drop in the comment of the enum) to find out where a given common symbol is defined?

I guess I'd probably `cd` into this directory and grep for the type's name? https://github.com/astral-sh/ruff/tree/main/crates/red_knot_vendored/vendor/typeshed/stdlib

---

_@AlexWaygood reviewed on 2024-10-05 16:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:758 on 2024-10-05 16:20_

> So, small question on the topic. I see that we have methods to lookup `typing_extensions`, but nothing from `typing`

Yeah, that's for a very similar reason! There's some symbols that only exist in `typing` on some versions of Python, but exist in `typing_extensions` on all versions of Python. That means that it's usually better for a type checker to look things up in `typing_extensions` rather than `typing`, since `typing_extensions` re-exports all symbols from `typing`, and typeshed [pretends `typing_extensions` is part of the stdlib](https://github.com/python/typeshed/blob/main/stdlib/typing_extensions.pyi). (I _promise_ typeshed doesn't do this for _loads_ of modules... only `_typeshed` and `typing_extensions`!!)

---

_Review requested from @AlexWaygood by @Slyces on 2024-10-05 16:29_

---

_@Slyces reviewed on 2024-10-05 16:31_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:890 on 2024-10-05 16:31_


If that would be more manageable, one option we would have for the maintainability of mapping str to KnownClass would be to only add a dependency for tests, which would be used to ensure we didn't miss a case.
Don't know if that's more acceptable, but it would ensure that we never miss a case without adding dependencies to the production code



---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:754 on 2024-10-05 16:34_

```suggestion
            Type::Function(_) => KnownClass::FunctionType.to_class(db),
            Type::Module(_) => KnownClass::ModuleType.to_class(db),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:758 on 2024-10-05 16:35_

```suggestion
            Type::None => KnownClass::NoneType.to_class(db),
            // TODO not accurate if there's a custom metaclass...
            Type::Class(_) => KnownClass::Type.to_class(db),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:843 on 2024-10-05 16:37_

nit

```suggestion
    NoneType, // Part of `types` for Python >= 3.10
```

---

_@AlexWaygood reviewed on 2024-10-05 16:38_

---

_@AlexWaygood approved on 2024-10-05 16:47_

Thanks!

---

_@AlexWaygood reviewed on 2024-10-05 16:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:890 on 2024-10-05 16:50_

Let's land the PR for now. Maybe we can create an issue to discuss this further and see what the other red-knot maintainers think :-)

---

_Merged by @AlexWaygood on 2024-10-05 17:03_

---

_Closed by @AlexWaygood on 2024-10-05 17:03_

---

_Branch deleted on 2024-10-05 17:08_

---

_@MichaReiser reviewed on 2024-10-07 08:40_

I like this a lot. This is excellent API design :)

---
