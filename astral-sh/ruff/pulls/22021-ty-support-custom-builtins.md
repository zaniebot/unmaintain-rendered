```yaml
number: 22021
title: "[ty] Support custom builtins"
type: pull_request
state: merged
author: Wizzerinus
labels:
  - ty
assignees: []
merged: true
base: main
head: feature/custom-builtins
created_at: 2025-12-17T12:30:23Z
updated_at: 2025-12-23T13:48:42Z
url: https://github.com/astral-sh/ruff/pull/22021
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Support custom builtins

---

_Pull request opened by @Wizzerinus on 2025-12-17 12:30_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds the support for custom builtins in ty. They can now be added by placing a `__builtins__.pyi` file in the project root and defining any necessary builtins.

Right now `builtins.pyi` is not supported because it's shadowed by Typeshed (Pyright does support both AFAIK), but I think this is not really a problem.

Fixes https://github.com/astral-sh/ty/issues/374

## Test Plan

Added mdtests for custom builtins, did some manual tests on our project that makes heavy use of builtins.

---

_Review requested from @carljm by @Wizzerinus on 2025-12-17 12:30_

---

_Review requested from @AlexWaygood by @Wizzerinus on 2025-12-17 12:30_

---

_Review requested from @sharkdp by @Wizzerinus on 2025-12-17 12:30_

---

_Review requested from @dcreager by @Wizzerinus on 2025-12-17 12:30_

---

_Label `ty` added by @AlexWaygood on 2025-12-17 12:32_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 12:35_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 12:36_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 40 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5100 diagnostics
+ Found 5103 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @Wizzerinus on 2025-12-17 13:05_

Hm, I'm not sure why colour-science's performance fell down more than twice. Back to the drawing board I guess.

EDIT: nevermind, I checked the report once again and that's not actually the case. phew.

---

_@MichaReiser approved on 2025-12-17 14:39_

This looks good to me. Thank you. I'd appreciate a second opinion on the change itself as we never made a formal decision on the issue itself. 

It would be great to document this new feature (over in the ty repository)

I think this is useful and it might also provides an easy workaround for https://github.com/astral-sh/ty/issues/1297

---

_Comment by @Wizzerinus on 2025-12-17 15:41_

Yep, I'll document this once(/if) it's merged (looks like it's in a different repo so a separate PR is necessary anyway).

Does seem useful for IPython stuff, though currently it would require making a builtins file in the project, which seems slightly hacky.

---

_@AlexWaygood reviewed on 2025-12-18 11:09_

I agree we should let users customise the `builtins` module without having to provide a whole custom typeshed directory. My only reservations here are whether this is the best design. It seems strange to me to have "implicit magic" treatment of a `__builtins__` name:
- Why `__builtins__` specifically? Do modules with that name have any special treatment at runtime? (I don't think so?)
- Do any other type checkers give implicit special treatment to a `__builtins__.pyi` stub that happens to be in the workspace root, or is this currently a pyright-specific feature?

To me a more intuitive design would be to have this be an opt-in configurable setting, e.g. you'd put something like this in your configuration file:

```toml
custom-builtins = "__builtins__.pyi"
```

---

_Comment by @MichaReiser on 2025-12-18 11:12_

I like convention over configuration but also wouldn't mind it being an option. The performance is probably comparable between the two because one requires resolving the settings first and the other does an unnecessary `resolve_module` call

> `custom-builtins = "__builtins__.pyi"`

This would be under a new `analysis` section.



---

_Comment by @AlexWaygood on 2025-12-18 11:15_

> I like convention over configuration but also wouldn't mind it being an option.

Right, that's why I was curious if there really was a convention here, or if this was currently just a pyright-specific thing. If e.g. pyrefly or mypy also does the same thing as pyright here, I'm happy to go with the crowd. But this is not the design I would choose otherwise

---

_Comment by @MichaReiser on 2025-12-18 11:19_

We could establish the convention. That's how conventions happen :)

It just feels slightly easier if all you have to do is: Create a `__builtins__.pyi`, done compared to create a `builtins.pyi`, set `custom-builtins`. It also makes it easier for users migrating from pyright

Edit: pyrefly also supports `__builtins__.py` https://pyrefly.org/en/docs/migrating-from-pyright/#extending-builtins


---

_Comment by @Wizzerinus on 2025-12-18 11:19_

Yeah, it does seem that mypy does not support this feature (and it makes sense that Pyrefly does not support it since it only just released recently). I would be happy to swap to any other way to determine where (and whether) the project stores custom builtins if necessary.

---

_Comment by @MichaReiser on 2025-12-18 11:22_

I think one issue with the current implementation is that `resolve_module` resolves `__builtins__.py` from any search path, including third-party ones. We should definitely change that.

---

_Comment by @Wizzerinus on 2025-12-18 11:23_

Yeah, that I definitely should look into fixing :D 

---

_Comment by @AlexWaygood on 2025-12-18 11:26_

It still seems very strange to me that, without any explicit configuration to opt into it, we would have very special treatment of a module name that (as far as I know) has no special treatment at runtime by the interpreter. That doesn't seem like intuitive behaviour at all to me; we'll have to make sure we document it well.

But if both pyright and pyrefly already agree on this design, then I'm okay with moving forward here.

Curious if @sharkdp or @carljm have any thoughts

---

_Comment by @Wizzerinus on 2025-12-18 11:32_

I don't think Pyrefly agrees on this behavior, and quite frankly, from what I've seen in the Pyrefly GitHub this was not brought up to Pyrefly's devs at all (at least publicly). so 🤷🏼‍♂️ and yeah, when you put it this way it makes much less sense that I did it like this now

---

_Comment by @AlexWaygood on 2025-12-21 19:11_

> Edit: pyrefly also supports `__builtins__.py` [pyrefly.org/en/docs/migrating-from-pyright#extending-builtins](https://pyrefly.org/en/docs/migrating-from-pyright/#extending-builtins)

but (according to those docs), it only supports this feature if you put the `__builtins__.pyi` file in a directory that you've explicitly listed as an extra search path via pyrefly's [`site-package-path` config setting](https://pyrefly.org/en/docs/configuration/#site-package-path). Pyrefly's `site-package-path` config setting is their equivalent of our `extra-paths` configuration setting, though unlike us (and contradicting the spec?) they give the paths specified by this setting the lowest priority in import resolution.

Giving this _very_ special behaviour to `__builtins__.pyi` files that we specifically find in an `extra-paths` search path seems much more defensible to me than implicitly giving this very special behaviour to _any_ `__builtins__.pyi` file from any search path.

But at that point, you've already said that the user will need some kind of explicit configuration to opt into this behaviour (they need to specify a certain directory in the `extra-paths` config setting). So it still seems to _me_ that the clearest behaviour would be to have a specific `custom-builtins` config setting just for opting into this feature, where you can specify the specific file you want ty to treat as a custom `builtins` stub.

---

_Comment by @MichaReiser on 2025-12-21 19:46_

> But at that point, you've already said that the user will need some kind of explicit configuration to opt into this behaviour (they need to specify a certain directory in the extra-paths config setting). So it still seems to me that the clearest behaviour would be to have a specific custom-builtins config setting just for opting into this feature, where you can specify the specific file you want ty to treat as a custom builtins stub.

One concern I have with the ability to specify an arbitrary path is that we may fail to resolve imports in that file if it isn't in one of the search paths. This is not an argument against it, it's just something that should be called out in the documentation.

We probably also want to make sure that ty raises typing errors in `__builtins__.pyi`, and the easiest way to do that is to require the file to be in `project.root`.

---

_Comment by @AlexWaygood on 2025-12-21 19:50_

I think the documentation for this option should be quite extensive in general. It's an advanced option that should be treated with extreme care, in my opinion. Python's builtin classes are some of the most important types in Python. Typeshed updates and refines its annotations for these methods on a regular basis, but by using this option you won't benefit from any upstream improvements to these types. There's also a much higher risk of ty crashing if you use a custom builtins file, because nearly all of our tests are written using typeshed's `builtins.pyi`, and it's very fundamental to type-checking most Python code.

---

_Comment by @AlexWaygood on 2025-12-21 20:04_

Ah no, sorry, please disregard my last comment -- I misunderstood the feature. This layers additional symbols on top of typeshed's `builtins.pyi` file, rather than replacing typeshed's `builtins.pyi` file entirely.

---

_Comment by @Wizzerinus on 2025-12-21 21:23_

Yeah, this does not replace builtins but rather add to them, replacing entirely sounds crazy since basic types that are likely hardcoded to some extent (i.e. types of literals) can get 'destroyed', so it's for the best this is not done.

From my comment in the ty issue:
> I'm thinking that we could probably have __builtins__.pyi in project root detected by default, and allow overriding the path to the stub in the config. That would mean overriding stubPath would require putting typings.__builtins__ instead of just typings in the config, but also means we're not cornercasing a name that doesn't really mean anything (which alleviates a concern I've heard earlier).

Which is basically the same version proposed in https://github.com/astral-sh/ruff/pull/22021#issuecomment-3679305754 but with a defaut. Though I am unsure whether having the default set to `__builtins__` or have no default value is better.

I will implement this some time next week, I am somewhat sick right now so gotta hope I won't take too long

---

_Comment by @MichaReiser on 2025-12-22 08:58_

IMO, defaulting to `__builtins__.pyi` seems fine and we could consider an option that allows you to rename the module name (it would be a module name and not a path). I would be okay not adding the option in this initial version. It's something we can easily add later based on user requests unless we're concerned that users make use of this feature by accident (again, I think that's unlikely, given the very specific module name)

---

_Comment by @carljm on 2025-12-23 00:59_

Both pyright and pyrefly support `__builtins__.pyi` in the project root or in an extra-path; I think that's sufficient to establish a convention, and we should just make life easier for users and follow the same convention. I'm not that worried about special-casing the module name: it's a dunder name, which Python already tells people to expect to have special behavior, and it's not used as a module name by Python itself. It might not be the design I'd have chosen from a blank slate, but it's easy for users, and I don't think it's sufficiently problematic to warrant introducing an incompatibility with other type checkers over.

I also don't think we need to discriminate which search path we found `__builtins__.pyi` in. Neither pyright nor pyrefly actually do this (despite their documentation arguably implying otherwise). I tested this: if you create a venv and put `__builtins__.pyi` in its `site-packages` directory, and type check using that virtualenv, both pyright and pyrefly will discover the `__builtins__.pyi` and treat it as extra builtins.

Also, both pyright and pyrefly are happy to use it even if it is `__builtins__.py`, not `__builtins__.pyi`.

> but (according to those docs), it only supports this feature if you put the `__builtins__.pyi` file in a directory that you've explicitly listed as an extra search path

No, pyrefly also supports it in the project root, which I expect is the common case. The docs do say that, and I tested and verified it.

---

_Comment by @carljm on 2025-12-23 01:01_

I also don't think we should implement a config option to specify the path to the `__builtins__.py[i]` file. That just seems like extra complexity in our implementation, and extra cognitive load for readers of our docs, to support something that no user has asked for and doesn't have any clear use case.

---

_Comment by @carljm on 2025-12-23 01:02_

So basically I think we should implement the simplest-possible (and easiest to implement!) version of this feature, which matches pyright and pyrefly, and not overthink it :)

---

_@carljm approved on 2025-12-23 01:08_

This looks good to me; thanks for the contribution!

Will hold off on merging for now to give @AlexWaygood and @MichaReiser another chance to consider if they want to advocate for any changes here -- but I'd suggest we merge this as-is and spend our time on other things.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:4961 on 2025-12-23 07:51_

What's the reason for making this change here rather than changing `module.static_member`? Are there other places where we call `module.static_member` that require the same treatment?


Should we use `known` module here instead of matching by name?

```suggestion
                let sym = if module.module(db).known(db).is_builtins() {
					builtins_symbol(db, attribute)
				} else {
					module.static_member(db, attribute)
                };
                
                if let Place::Defined(attr_ty, _, _) = sym.place {
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/place.rs`:403 on 2025-12-23 07:51_

```suggestion
    resolve_module_confident(db, &ModuleName::new_static("__builtins__").unwrap())
```

---

_@MichaReiser approved on 2025-12-23 08:13_

---

_Comment by @AlexWaygood on 2025-12-23 09:51_

If Micha and Carl both agree on this design, that's good enough for me :-)

(and sorry for misreading the Pyrefly docs!)

---

_@Wizzerinus reviewed on 2025-12-23 12:32_

---

_Review comment by @Wizzerinus on `crates/ty_python_semantic/src/types/infer/builder.rs`:4961 on 2025-12-23 12:32_

Getting back to this.

So, moving that change to `Type::static_member` allows us to pass the following test (assuming `foo: int` is defined in the stub):

```py
import builtins
import inspect

reveal_type(inspect.getattr_static(builtins, "foo"))  # revealed: int
```

If I had to guess (I have not tested it), moving it to `ModuleLiteralType::static_member` would also allow us to pass the following test:

```py
import builtins

reveal_type(getattr(builtins, "foo"))  # revealed: int
```

So far so good (though I do not want to think about the consequences of `builtins.__dict__` being type-revealed; I assume we could reveal an intersection type of the builtins dict and the `__builtins__` dict there).

However, the comment on `fn builtins_symbol` explicitly says:

```
/// Note that this function is only intended for use in the context of the builtins *namespace*
/// and should not be used when a symbol is being explicitly imported from the `builtins` module
/// (e.g. `from builtins import int`).
```

And the passing tests contradict this comment. I don't know why the comment is in here, since I barely have any experience working with ty's codebase, but I assume it's there for a reason and I shouldn't introduce behavior that directly goes against the comment.

---

_Comment by @Wizzerinus on 2025-12-23 12:41_

Resolved the comments and rebased to fix a conflict with the recent crate change. I have one additional question about moving the builtins check into `module.static_member`, described above.

---

_@MichaReiser reviewed on 2025-12-23 13:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:4961 on 2025-12-23 13:13_

Makes sense to keep it where it is for now. Thanks for taking a look

---

_Comment by @Wizzerinus on 2025-12-23 13:31_

had to force push to satisfy clippy and formatter, so automerge will have to be reenabled, whoops

---

_Merged by @MichaReiser on 2025-12-23 13:48_

---

_Closed by @MichaReiser on 2025-12-23 13:48_

---

_Branch deleted on 2025-12-23 13:48_

---
