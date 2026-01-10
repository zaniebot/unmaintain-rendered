```yaml
number: 19376
title: "[ty] Add support for `@warnings.deprecated`"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: gankra/warn-dep
created_at: 2025-07-15T20:22:48Z
updated_at: 2025-07-18T23:59:17Z
url: https://github.com/astral-sh/ruff/pull/19376
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Add support for `@warnings.deprecated`

---

_Pull request opened by @Gankra on 2025-07-15 20:22_

* [x] basic handling
  * [x] parse and discover `@warnings.deprecated` attributes
  * [x] associate them with function definitions
  * [x] associate them with class definitions
  * [x] add a new "deprecated" diagnostic
  * [x] ensure diagnostic is styled appropriately for LSPs (DiagnosticTag::Deprecated) 

* [x] functions
  * [x] fire on calls
  * [x] fire on arbitrary references 
* [x] classes
  * [x] fire on initializers
  * [x] fire on arbitrary references
* [x] methods
  * [x] fire on calls
  * [x] fire on arbitrary references
* [ ] overloads
  * [ ] fire on calls
  * [ ] fire on arbitrary references(??? maybe not ???)
  * [ ] only fire if the actual selected overload is deprecated 

* [ ] dunder desugarring (warn on deprecated `__add__` if `+` is invoked)
* [ ] alias supression? (don't warn on uses of variables that deprecated items were assigned to)

* [ ] import logic
  * [x] fire on imports of deprecated items
  * [ ] suppress subsequent diagnostics if the import diagnostic fired (is this handled by alias supression?)
  * [x] fire on all qualified references (`module.mydeprecated`)
  * [x] fire on all references that depend on a `*` import
    


Fixes https://github.com/astral-sh/ty/issues/153

---

_Review requested from @carljm by @Gankra on 2025-07-15 20:22_

---

_Review requested from @AlexWaygood by @Gankra on 2025-07-15 20:22_

---

_Review requested from @sharkdp by @Gankra on 2025-07-15 20:22_

---

_Review requested from @dcreager by @Gankra on 2025-07-15 20:22_

---

_Renamed from "Add support for `@warnings.deprecated`" to "[ty] Add support for `@warnings.deprecated`" by @Gankra on 2025-07-15 20:22_

---

_Label `ty` added by @Gankra on 2025-07-15 20:22_

---

_Label `diagnostics` added by @Gankra on 2025-07-15 20:23_

---

_Comment by @Gankra on 2025-07-15 20:28_

The big thing left to do here is to generalize to "fire on all references". I haven't started on it but I assume it's non-trivial?

---

_@Gankra reviewed on 2025-07-15 20:30_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/infer.rs`:5556 on 2025-07-15 20:30_

I don't love this current formatting -- pylance displays

```
The class "{class_name}" is deprecated
    {message}
```

Which is presumably some kind of sub-diagnostic and not literally a newline and indent? But Maybe I'm wrong (tried `diag.info(message)` but that didn't display in vscode).

---

_@Gankra reviewed on 2025-07-15 20:32_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:236 on 2025-07-15 20:32_

I didn't obviously see how to get this precision but I didn't look hard, something to ask @dhruvmanila about.

---

_@Gankra reviewed on 2025-07-15 20:32_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:36 on 2025-07-15 20:32_

I was surprised methods were somehow special here, oh well, gotta find the code that handles those!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:5593 on 2025-07-15 20:54_

I think this is where to get the more precision you mentioned in https://github.com/astral-sh/ruff/pull/19376#discussion_r2208606501.  Instead of checking whether `function_literal` is deprecated (which checks if any overload of the function is deprecated), you only want to check if `overload` is (that is, the particular overload that was matched for this call).

(My _hope_ is that `overload.binding.binding_type` is the `FunctionLiteral` that points right at the particular overload that was matched. If not, you'll have to get the overload index from the `matching_overloads_mut` iterator (which is already enumerated for you), and use that index to walk through the overloads looking for the right one. I'm about to sign off for the day but I can help you look at this tomorrow)

---

_@dcreager reviewed on 2025-07-15 21:01_

---

_@Gankra reviewed on 2025-07-16 03:28_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/infer.rs`:5593 on 2025-07-16 03:28_

I believe no matter what you need to iterate a list of overloads, as e.g. `int | str` might match an `int` overload *and* a `str` overload (but not say, a third `bool` one).

---

_@sharkdp reviewed on 2025-07-16 06:46_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:98 on 2025-07-16 06:46_

It's only been added very recently in 3.13: https://github.com/python/typeshed/blob/main/stdlib/warnings.pyi#L120-L121

Mdtests check with version set to 3.9 by default (which makes that particular definition in typeshed invisible/unreachable), but you can change this by adding a `toml`-block in this section to switch the version to 3.13: https://github.com/astral-sh/ruff/blob/main/crates/ty_test/README.md#configuration

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:36 on 2025-07-16 06:54_

> I was surprised methods were somehow special

They are, yes. When you access the `amethod` attribute through an instance of `MyClass` (`MyClass().amethod`), the [descriptor protocol](https://docs.python.org/3/howto/descriptor.html#id25) will be invoked (function objects are descriptors), which turns the function `amethod` into a bound method object. You can also see that at runtime:

It's a function when accessed through the class object itself (the descriptor protocol is also invoked here, but acts like a no-op):
```pycon
>>> MyClass.amethod
<function MyClass.amethod at 0x7f6251ca6160>
```

But it becomes a bound method object when accessed through an instance
```
>>> MyClass().amethod
<bound method MyClass.amethod of <__main__.MyClass object at 0x7f6251c45a90>>
```

The difference is that the `self` argument is already bound to the instance of the class, which allows you to call it with one parameter fewer.

In ty, this is modeled via a special type variant called `Type::BoundMethod`, which keeps a reference to the original function inside, so you should be able to get the deprecation message from there.

---

_@sharkdp reviewed on 2025-07-16 06:54_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:89 on 2025-07-16 07:22_

```suggestion
version in `warnings` is canonical, the other versions are compatibility shims.
```

---

_@sharkdp reviewed on 2025-07-16 07:22_

---

_@sharkdp reviewed on 2025-07-16 07:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:88 on 2025-07-16 07:24_

Does `typing.deprecated` really exist? Couldn't find any reference to it, and can't import it at runtime in 3.13. It might eventually appear, given that it's present in `typing_extensions`, but it seems like not yet.

---

_@AlexWaygood reviewed on 2025-07-16 07:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:88 on 2025-07-16 07:27_

No, we'll never add it to `typing` in CPython, since it already exists in the stdlib in the `warnings` module, which is a more logical place for it overall!

`typing_extensions` also includes a few other backports from other stdlib modules that will never be in the typing module but may be useful to users of type hints.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:47 on 2025-07-16 07:29_

That should ideally generate an error here, I think? Passing `invalid_deco` to `deprecated`'s constructor should yield an error (because it requires `message: LiteralString`)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/module.rs`:282 on 2025-07-16 07:31_

This is more of a question for our semantic model experts. Is it okay to return `warnings` here even if it represents `typing.warnings`, `typing_extensions.warnings`, ... or do we need different variants for each?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:65 on 2025-07-16 07:34_

That's interesting. We only see this `missing-argument` diagnostic because `invalid_deco` is turned into a `deprecated` object above (which is sort of the right outcome of this incorrect call). So calling `invalid_deco` here is actually a call to `deprecated.__call__`, which takes one argument.

I don't think it's a huge deal, but given that I could see someone genuinely making this mistake, we might attempt to improve the experience here? Instead of returning a `deprecated` object from that invalid use of `@deprecated`, maybe we could return the function itself instead (with an empty deprecation message)? (assuming that we will emit an error on the construct above).

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:796 on 2025-07-16 07:36_

Using name here probably makes sense but it's a stretch for what the type was intended for because this is not a name ;)

We might just want to use `compactstr` here directly or use a regular `String` (they're not that common)

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:180 on 2025-07-16 07:38_

Do we want to error on every *use* (load) of these deprecated symbols, i.e. whenever they are read?

---

_@sharkdp reviewed on 2025-07-16 07:41_

Thank you!

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:5556 on 2025-07-16 07:48_

It would be good if you add a [diagnostic snapshot](https://github.com/astral-sh/ruff/tree/main/crates/ty_test#diagnostic-snapshotting) test to your mdtest so that "we can see" how the diagnostics get rendered. You might also want to try running ty with `output-format=concise` just to see how it looks. 

I would then give `set_primary_message` a try (setting it to `message`). It will give you the `: {message}` rendering in concise mode but the message is rendered inside the code frame when using `output-format=full`. 

It might also be worth taking a look at what rustc does.

I'm not entirely sure but I'm wondering if we should add a sub diagnostic that highlights the deprecated declaration. 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/function.rs`:202 on 2025-07-16 08:02_

Use `returns(ref)` or `returns(deref)` to prevent that the accessor clones `Name` on every read
```suggestion
    /// If `Some` then contains the message from @deprecated
    #[returns(ref)]
    pub(crate) deprecated: Option<ast::name::Name>,
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:796 on 2025-07-16 08:03_

Use `returns(ref)` so that the salsa generated getter returns a reference instead of cloning the field

```suggestion
		#[returns(ref)]
    pub(crate) deprecated: Option<ast::name::Name>,
```

---

_@MichaReiser reviewed on 2025-07-16 08:04_

Thank you. 

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/module_resolver/module.rs`:282 on 2025-07-16 08:30_

This is fine. It only establishes the `warnings` module as a `KnownModule`.

> even if it represents `typing.warnings`, `typing_extensions.warnings`

There's `typing_extensions.deprecated` as well, but `typing_extensions` is already a known module. `typing.deprecated` doesn't exist (see one of my other comments). `typing.warnings` or `typing_extensions.warnings` do not exist.

---

_@sharkdp reviewed on 2025-07-16 08:30_

---

_@Gankra reviewed on 2025-07-16 13:11_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:180 on 2025-07-16 13:11_

I believe so, yes. This is suggested in https://typing.python.org/en/latest/spec/directives.html#deprecated

---

_@Gankra reviewed on 2025-07-16 13:12_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/module.rs`:282 on 2025-07-16 13:12_

I believe we do this kind of conflation for other KnownClasses with typing_extensions shims

---

_@Gankra reviewed on 2025-07-16 13:14_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/class.rs`:796 on 2025-07-16 13:14_

Yeah I agree Name is very "uhhhhh this seems wrong" but it was the only type I could find a good precedent for in the code. Happy to change it to either of those other types.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:236 on 2025-07-16 13:51_

I think this might require invoking the call logic i.e., calling the `try_call` method on the type of `f` with the argument types. This should give you the `Bindings` object which would be populated with all the information about the call including the chosen overload which is `CallableBinding::matching_overload_index`. The problem with this is that the `matching_overload_index` for now is only populated for step 1 and step 4 of the algorithm.

For step 2, it can easily be done here:

https://github.com/astral-sh/ruff/blob/f3a27406c91438ccad488212c4a9651a7b8d2374/crates/ty_python_semantic/src/types/call/bind.rs#L1226-L1229

Step 3 will have more than one matching overloads so it might require updating the type of `matching_overload_index` or you could use the existing `MatchingOverloadIndex`.

Step 5 could also result in multiple matching overloads which can also be represented using `MatchingOverloadIndex`.

---

_@dhruvmanila reviewed on 2025-07-16 13:51_

---

_@dhruvmanila reviewed on 2025-07-16 13:51_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:236 on 2025-07-16 13:51_

But, if this PR doesn't result into too many false positives, we could do this in a follow-up PR as well.

---

_@Gankra reviewed on 2025-07-16 14:32_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:236 on 2025-07-16 14:32_

I think the false positives will be really annoying here, and deprecated-overloads is pretty narrow, so I think it's best to just disable all support for deprecated overloads until said followup.

---

_@dhruvmanila reviewed on 2025-07-16 14:56_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:236 on 2025-07-16 14:56_

That sounds reasonable. Can you open an issue to keep track of it?

---

_Comment by @Gankra on 2025-07-16 15:19_

I've pushed up an updated implementation that focuses on places like `infer_load_expr` and `infer_import_from`. I think the currently implemented functionality is shippable (mdtest TODOs cover the limitations I'm Knowningly shipping).

I still need to do some cleanups to the code though.

I also haven't looked at all at the specific clauses about inheritance and overrides.

---

_@dcreager reviewed on 2025-07-16 15:28_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:796 on 2025-07-16 15:28_

Or `Type<'db>`!

That would move your `into_string_literal` call from `KnownClass::check_call` to `TypeInferenceBuilder::check_deprecated` (i.e. don't worry about coercing the deprecation message into a string until you render the deprecation diagnostic).

It would mean not being strict about the deprecation warning being a string literal; the following would become allowed:

```py
message = "this is bad " + "don't use it"

@warnings.deprecated(message)
def bad_function(): ...
```

@JelleZijlstra mentioned in discord that the language in the spec about the message being a string literal was more meant to be advisory, to not _require_ type checkers to have to do things like constant folding. But we already do, and I think it won't be any more complex to store the deprecation message as a `Type<'db>` as we pass it around various places.

---

_@dcreager reviewed on 2025-07-16 15:31_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:5593 on 2025-07-16 15:31_

Right, I think I spoke poorly. The `for` loop over `matching_overloads_mut` handles what you're describing — the loop body will be invoked for each overload that the call actually matches. But for each of those matching overloads, you need to find the `OverloadLiteral` that describes its signature, so that you can see if it has a `deprecated` decorator attached. What I was describing was how to go from the `Binding` (which is what the `overload` variable is) to that `OverloadLiteral`. That might require walking through what is effectively a linked list.

---

_@dcreager reviewed on 2025-07-16 15:38_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:796 on 2025-07-16 15:38_

Hmm, although...I think that my example might work with how you've currently done this, too! You're not checking that the argument is syntactically a string literal. Because you're using `into_string_literal`, you're checking that its inferred type is a statically known string.

So given that, I think `Option<StringLiteralType<'db>>` is what you want to store here. That's an interned salsa struct, so they're cheap to store and pass around.

---

_@Gankra reviewed on 2025-07-17 16:07_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/class.rs`:796 on 2025-07-17 16:07_

That worked, thanks!

---

_Comment by @Gankra on 2025-07-17 16:34_

Rebased and addressed all reviews.

---

_Comment by @github-actions[bot] on 2025-07-17 16:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
+ warning[deprecated] parso/tree.py:1:33: The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
+ warning[deprecated] parso/tree.py:136:6: The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
+ warning[deprecated] parso/tree.py:144:6: The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
- Found 72 diagnostics
+ Found 75 diagnostics

DateType (https://github.com/glyph/DateType)
+ warning[deprecated] src/datetype/__init__.py:529:32: The function `utcfromtimestamp` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .fromtimestamp(datetime.timezone.utc)
+ warning[deprecated] src/datetype/__init__.py:533:32: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 6 diagnostics
+ Found 8 diagnostics

python-sop (https://gitlab.com/dkg/python-sop)
+ warning[deprecated] sop/__init__.py:424:29: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 2 diagnostics
+ Found 3 diagnostics

anyio (https://github.com/agronholm/anyio)
+ warning[deprecated] src/anyio/__init__.py:46:29: The function `wait_socket_readable` is deprecated: This function is deprecated; use `wait_readable` instead
+ warning[deprecated] src/anyio/__init__.py:47:29: The function `wait_socket_writable` is deprecated: This function is deprecated; use `wait_writable` instead
- Found 93 diagnostics
+ Found 95 diagnostics

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
+ warning[deprecated] backend/api/api_wrappers.py:56:33: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] backend/api/core_api.py:36:29: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] backend/api/core_api.py:37:29: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] backend/api/global_scoreboard_api.py:76:20: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] backend/services/user_updater.py:78:30: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 60 diagnostics
+ Found 65 diagnostics

comtypes (https://github.com/enthought/comtypes)
+ warning[deprecated] comtypes/test/__init__.py:137:21: The function `makeSuite` is deprecated: Deprecated in Python 3.11; removal scheduled for Python 3.13
+ warning[deprecated] comtypes/test/__init__.py:207:24: The function `makeSuite` is deprecated: Deprecated in Python 3.11; removal scheduled for Python 3.13
- Found 420 diagnostics
+ Found 422 diagnostics

alerta (https://github.com/alerta/alerta)
+ warning[deprecated] alerta/auth/oidc.py:99:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/auth/utils.py:52:20: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/commands.py:66:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:190:56: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:214:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:1116:79: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:1194:51: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:1259:52: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:1301:67: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:1517:41: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:1603:44: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:1608:41: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:1618:100: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:1668:95: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/base.py:1718:95: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/utils.py:250:79: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/utils.py:250:120: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/utils.py:252:68: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/utils.py:254:67: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/utils.py:345:70: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/database/backends/mongodb/utils.py:347:69: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/dev.py:4:23: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/management/views.py:144:39: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/alert.py:66:72: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/alert.py:75:74: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/alert.py:282:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/alert.py:335:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/alert.py:379:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/alert.py:430:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/alert.py:574:34: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/alert.py:594:34: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/alert.py:609:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/alert.py:637:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/blackout.py:37:65: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/blackout.py:60:91: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/blackout.py:84:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/heartbeat.py:56:52: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/heartbeat.py:59:74: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/heartbeat.py:61:31: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/history.py:16:72: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/key.py:31:52: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/key.py:42:49: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/key.py:164:57: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/note.py:25:91: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/user.py:41:72: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/models/user.py:44:72: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/utils/audit.py:94:36: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/utils/client.py:44:45: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] alerta/views/alerts.py:309:27: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] tests/test_blackouts.py:688:42: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 447 diagnostics
+ Found 497 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ warning[deprecated] dulwich/porcelain.py:5638:25: The function `warn` is deprecated: Deprecated; use warning() instead.
- Found 169 diagnostics
+ Found 170 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ warning[deprecated] discord/member.py:929:91: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] discord/member.py:985:61: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 519 diagnostics
+ Found 521 diagnostics

httpx-caching (https://github.com/johtso/httpx-caching)
+ warning[deprecated] httpx_caching/_heuristics.py:14:29: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 27 diagnostics
+ Found 28 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ warning[deprecated] github/MainClass.py:134:42: The class `AppAuthentication` is deprecated: Use github.Auth.AppInstallationAuth instead
+ warning[deprecated] github/MainClass.py:179:19: The class `AppAuthentication` is deprecated: Use github.Auth.AppInstallationAuth instead
+ warning[deprecated] github/Requester.py:118:36: The class `AppAuthentication` is deprecated: Use github.Auth.AppInstallationAuth instead
+ warning[deprecated] github/Requester.py:289:17: The class `AppAuthentication` is deprecated: Use github.Auth.AppInstallationAuth instead
+ warning[deprecated] github/__init__.py:55:32: The class `AppAuthentication` is deprecated: Use github.Auth.AppInstallationAuth instead
+ warning[deprecated] tests/Authentication.py:95:31: The class `AppAuthentication` is deprecated: Use github.Auth.AppInstallationAuth instead
- Found 301 diagnostics
+ Found 307 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ warning[deprecated] bson/codec_options.py:63:10: The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
+ warning[deprecated] bson/codec_options.py:82:10: The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
- Found 423 diagnostics
+ Found 425 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ warning[deprecated] pydantic/__init__.py:62:46: The function `root_validator` is deprecated: Pydantic V1 style `@root_validator` validators are deprecated. You should migrate to Pydantic V2 style `@model_validator` validators, see the migration guide for more details
+ warning[deprecated] pydantic/__init__.py:62:62: The function `validator` is deprecated: Pydantic V1 style `@validator` validators are deprecated. You should migrate to Pydantic V2 style `@field_validator` validators, see the migration guide for more details
+ warning[deprecated] pydantic/__init__.py:63:36: The class `BaseConfig` is deprecated: BaseConfig is deprecated. Use the `pydantic.ConfigDict` instead.
+ warning[deprecated] pydantic/__init__.py:63:48: The class `Extra` is deprecated: Extra is deprecated. Use literal values instead (e.g. `extra='allow'`)
+ warning[deprecated] pydantic/color.py:89:32: The class `Color` is deprecated: The `Color` class is deprecated, use `pydantic_extra_types` instead. See https://docs.pydantic.dev/latest/api/pydantic_extra_types_color/.
+ warning[deprecated] pydantic/color.py:242:56: The class `Color` is deprecated: The `Color` class is deprecated, use `pydantic_extra_types` instead. See https://docs.pydantic.dev/latest/api/pydantic_extra_types_color/.
+ warning[deprecated] pydantic/color.py:252:34: The class `Color` is deprecated: The `Color` class is deprecated, use `pydantic_extra_types` instead. See https://docs.pydantic.dev/latest/api/pydantic_extra_types_color/.
+ warning[deprecated] pydantic/deprecated/decorator.py:85:72: The function `validate_arguments` is deprecated: The `validate_arguments` method is deprecated; use `validate_call` instead.
+ warning[deprecated] pydantic/deprecated/json.py:16:21: The class `Color` is deprecated: The `Color` class is deprecated, use `pydantic_extra_types` instead. See https://docs.pydantic.dev/latest/api/pydantic_extra_types_color/.
+ warning[deprecated] pydantic/deprecated/json.py:56:5: The class `Color` is deprecated: The `Color` class is deprecated, use `pydantic_extra_types` instead. See https://docs.pydantic.dev/latest/api/pydantic_extra_types_color/.
+ warning[deprecated] pydantic/deprecated/json.py:132:16: The function `pydantic_encoder` is deprecated: `pydantic_encoder` is deprecated, use `pydantic_core.to_jsonable_python` instead.
+ warning[deprecated] pydantic/deprecated/parse.py:78:12: The function `load_str_bytes` is deprecated: `load_str_bytes` is deprecated.
+ warning[deprecated] pydantic/deprecated/tools.py:101:9: The function `schema_of` is deprecated: `schema_of` is deprecated. Use `pydantic.TypeAdapter.json_schema` instead.
+ warning[deprecated] pydantic/main.py:267:30: The function `dict` is deprecated: The `dict` method is deprecated; use `model_dump` instead.
+ warning[deprecated] pydantic/main.py:278:39: The function `dict` is deprecated: The `dict` method is deprecated; use `model_dump` instead.
+ warning[deprecated] pydantic/main.py:288:30: The function `dict` is deprecated: The `dict` method is deprecated; use `model_dump` instead.
+ warning[deprecated] pydantic/main.py:436:10: The function `dict` is deprecated: The `dict` method is deprecated; use `model_dump` instead.
+ warning[deprecated] pydantic/main.py:546:10: The function `dict` is deprecated: The `dict` method is deprecated; use `model_dump` instead.
+ warning[deprecated] pydantic/main.py:937:34: The function `dict` is deprecated: The `dict` method is deprecated; use `model_dump` instead.
+ warning[deprecated] pydantic/main.py:1094:31: The function `dict` is deprecated: The `dict` method is deprecated; use `model_dump` instead.
+ warning[deprecated] pydantic/main.py:1105:35: The function `dict` is deprecated: The `dict` method is deprecated; use `model_dump` instead.
+ warning[deprecated] pydantic/main.py:1248:29: The function `dict` is deprecated: The `dict` method is deprecated; use `model_dump` instead.
+ warning[deprecated] pydantic/main.py:1362:25: The function `load_str_bytes` is deprecated: `load_str_bytes` is deprecated.
+ warning[deprecated] pydantic/main.py:1415:21: The function `load_file` is deprecated: `load_file` is deprecated.
+ warning[deprecated] pydantic/main.py:1557:38: The function `pydantic_encoder` is deprecated: `pydantic_encoder` is deprecated, use `pydantic_core.to_jsonable_python` instead.
+ warning[deprecated] pydantic/main.py:1561:21: The function `pydantic_encoder` is deprecated: `pydantic_encoder` is deprecated, use `pydantic_core.to_jsonable_python` instead.
+ warning[deprecated] pydantic/types.py:1949:78: The class `PaymentCardNumber` is deprecated: The `PaymentCardNumber` class is deprecated, use `pydantic_extra_types` instead. See https://docs.pydantic.dev/latest/api/pydantic_extra_types_payment/#pydantic_extra_types.payment.PaymentCardNumber.
+ warning[deprecated] pydantic/v1/_hypothesis_plugin.py:105:20: The class `Color` is deprecated: The `Color` class is deprecated, use `pydantic_extra_types` instead. See https://docs.pydantic.dev/latest/api/pydantic_extra_types_color/.
+ warning[deprecated] pydantic/v1/_hypothesis_plugin.py:126:22: The class `PaymentCardNumber` is deprecated: The `PaymentCardNumber` class is deprecated, use `pydantic_extra_types` instead. See https://docs.pydantic.dev/latest/api/pydantic_extra_types_payment/#pydantic_extra_types.payment.PaymentCardNumber.
+ warning[deprecated] pydantic/v1/_hypothesis_plugin.py:139:14: The class `PaymentCardNumber` is deprecated: The `PaymentCardNumber` class is deprecated, use `pydantic_extra_types` instead. See https://docs.pydantic.dev/latest/api/pydantic_extra_types_payment/#pydantic_extra_types.payment.PaymentCardNumber.
- Found 748 diagnostics
+ Found 778 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ warning[deprecated] strawberry/utils/typing.py:221:65: The class `Index` is deprecated: Deprecated since Python 3.9. Use the index value directly instead.
+ warning[deprecated] strawberry/utils/typing.py:258:65: The class `Index` is deprecated: Deprecated since Python 3.9. Use the index value directly instead.
+ warning[deprecated] strawberry/utils/typing.py:276:65: The class `Index` is deprecated: Deprecated since Python 3.9. Use the index value directly instead.
- Found 365 diagnostics
+ Found 368 diagnostics

mkosi (https://github.com/systemd/mkosi)
+ warning[deprecated] mkosi/documentation.py:32:29: The function `warn` is deprecated: Deprecated; use warning() instead.
- Found 90 diagnostics
+ Found 91 diagnostics

stone (https://github.com/dropbox/stone)
+ warning[deprecated] test/test_python_gen.py:145:33: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] test/test_python_gen.py:221:33: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] test/test_python_gen.py:413:33: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 113 diagnostics
+ Found 116 diagnostics

SinbadCogs (https://github.com/mikeshardmind/SinbadCogs)
+ warning[deprecated] antimentionspam/antimentionspam.py:239:26: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] embedmaker/serialize.py:81:34: The function `utcfromtimestamp` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .fromtimestamp(datetime.timezone.utc)
+ warning[deprecated] embedmaker/time_utils.py:42:54: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] modnotes/modnotes.py:148:32: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] rolemanagement/events.py:41:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] scheduler/message.py:82:57: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] scheduler/time_utils.py:42:54: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 142 diagnostics
+ Found 149 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
+ warning[deprecated] dragonchain/lib/authorization.py:51:30: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] dragonchain/lib/dto/smart_contract_model.py:100:95: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] dragonchain/lib/dto/smart_contract_model.py:300:121: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] dragonchain/scheduler/scheduler.py:65:51: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 302 diagnostics
+ Found 306 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ warning[deprecated] tornado/autoreload.py:339:26: The function `get_loader` is deprecated: Use importlib.util.find_spec() instead. Will be removed in Python 3.14.
+ warning[deprecated] tornado/test/httpclient_test.py:932:43: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] tornado/test/httputil_test.py:506:42: The function `utcfromtimestamp` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .fromtimestamp(datetime.timezone.utc)
- Found 243 diagnostics
+ Found 246 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
+ warning[deprecated] src/bandersnatch/utils.py:54:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 128 diagnostics
+ Found 129 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
+ warning[deprecated] freqtrade/rpc/api_server/api_ws.py:42:41: The function `dict` is deprecated: The `dict` method is deprecated; use `model_dump` instead.
- Found 368 diagnostics
+ Found 369 diagnostics

paasta (https://github.com/yelp/paasta)
+ warning[deprecated] paasta_tools/cli/cmds/logs.py:1018:46: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] paasta_tools/cli/cmds/logs.py:1315:42: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] paasta_tools/cli/cmds/logs.py:1327:40: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] paasta_tools/cli/cmds/start_stop_restart.py:276:77: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] paasta_tools/utils.py:1514:30: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] paasta_tools/utils.py:3710:52: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] paasta_tools/utils.py:3718:52: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 878 diagnostics
+ Found 885 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
+ warning[deprecated] AutoDuck/fixHelpCompression.py:16:10: The function `WriteProfileVal` is deprecated: This function is obsolete, applications should use the registry instead.
+ warning[deprecated] Pythonwin/pywin/framework/editor/vss.py:45:29: The function `GetProfileVal` is deprecated: This function is obsolete, applications should use the registry instead.
+ warning[deprecated] Pythonwin/pywin/framework/editor/vss.py:46:28: The function `GetProfileVal` is deprecated: This function is obsolete, applications should use the registry instead.
+ warning[deprecated] Pythonwin/pywin/framework/sgrepmdi.py:606:29: The function `GetProfileSection` is deprecated: This function is obsolete, applications should use the registry instead.
+ warning[deprecated] Pythonwin/pywin/framework/sgrepmdi.py:620:30: The function `WriteProfileVal` is deprecated: This function is obsolete, applications should use the registry instead.
+ warning[deprecated] com/win32com/test/testMSOffice.py:147:38: The function `MakeTime` is deprecated: Use pywintypes.Time() instead.
+ warning[deprecated] com/win32comext/shell/demos/servers/folder_view.py:25:18: The function `MakeIID` is deprecated: Use pywintypes.IID() instead.
+ warning[deprecated] com/win32comext/shell/demos/servers/folder_view.py:91:17: The function `MakeIID` is deprecated: Use pywintypes.IID() instead.
+ warning[deprecated] com/win32comext/shell/demos/servers/folder_view.py:92:17: The function `MakeIID` is deprecated: Use pywintypes.IID() instead.
+ warning[deprecated] com/win32comext/shell/demos/servers/folder_view.py:93:18: The function `MakeIID` is deprecated: Use pywintypes.IID() instead.
+ warning[deprecated] com/win32comext/shell/demos/servers/folder_view.py:94:18: The function `MakeIID` is deprecated: Use pywintypes.IID() instead.
+ warning[deprecated] com/win32comext/shell/demos/servers/folder_view.py:99:16: The function `MakeIID` is deprecated: Use pywintypes.IID() instead.
+ warning[deprecated] com/win32comext/shell/demos/servers/folder_view.py:100:17: The function `MakeIID` is deprecated: Use pywintypes.IID() instead.
+ warning[deprecated] com/win32comext/shell/demos/servers/folder_view.py:101:17: The function `MakeIID` is deprecated: Use pywintypes.IID() instead.
+ warning[deprecated] com/win32comext/shell/demos/servers/folder_view.py:102:17: The function `MakeIID` is deprecated: Use pywintypes.IID() instead.
+ warning[deprecated] com/win32comext/shell/demos/servers/folder_view.py:103:17: The function `MakeIID` is deprecated: Use pywintypes.IID() instead.
+ warning[deprecated] win32/Lib/win32serviceutil.py:139:28: The function `GetProfileVal` is deprecated: This function is obsolete, applications should use the registry instead.
- Found 2014 diagnostics
+ Found 2031 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ warning[deprecated] docs/conf.py:22:23: The function `utcfromtimestamp` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .fromtimestamp(datetime.timezone.utc)
- Found 144 diagnostics
+ Found 145 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ warning[deprecated] examples/contrib/webscanner_helper/urlindex.py:93:42: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] examples/contrib/webscanner_helper/watchdog.py:74:51: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] examples/contrib/webscanner_helper/watchdog.py:78:51: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] mitmproxy/contentviews/__init__.py:20:22: The function `get` is deprecated: Use `mitmproxy.contentviews.registry` instead.
+ warning[deprecated] mitmproxy/contentviews/__init__.py:22:22: The function `remove` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] mitmproxy/contentviews/__init__.py:42:19: The class `View` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] mitmproxy/contentviews/__init__.py:166:32: The class `View` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] mitmproxy/contentviews/_compat.py:20:45: The class `View` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] mitmproxy/contentviews/_compat.py:60:37: The class `View` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] mitmproxy/contentviews/_compat.py:73:18: The class `View` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] mitmproxy/contentviews/base.py:80:34: The class `View` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] mitmproxy/contentviews/base.py:120:12: The function `format_pairs` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test__compat.py:6:41: The class `View` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test__compat.py:10:20: The class `View` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test__compat.py:53:28: The function `get` is deprecated: Use `mitmproxy.contentviews.registry` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test__compat.py:59:28: The function `get` is deprecated: Use `mitmproxy.contentviews.registry` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test__compat.py:67:17: The function `remove` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test_base.py:9:20: The function `format_dict` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test_base.py:14:20: The function `format_dict` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test_base.py:19:20: The function `format_dict` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test_base.py:27:20: The function `format_pairs` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test_base.py:32:20: The function `format_pairs` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] test/mitmproxy/contentviews/test_base.py:37:20: The function `format_pairs` is deprecated: Use `mitmproxy.contentviews.Contentview` instead.
+ warning[deprecated] test/mitmproxy/test_certs.py:236:53: The function `to_pyopenssl` is deprecated: Use `to_cryptography` instead.
- Found 1797 diagnostics
+ Found 1821 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ warning[deprecated] openlibrary/accounts/model.py:241:52: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/accounts/model.py:605:40: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/admin.py:147:31: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/edits.py:237:43: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/edits.py:257:39: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/edits.py:286:39: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/edits.py:302:39: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/edits.py:328:44: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/imports.py:284:46: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/lending.py:838:35: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/lending.py:928:67: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/statsdb.py:32:48: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/statsdb.py:58:48: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/waitinglist.py:69:35: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/waitinglist.py:75:74: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/waitinglist.py:84:52: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/waitinglist.py:265:70: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/core/waitinglist.py:273:31: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/coverstore/db.py:41:29: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/coverstore/db.py:115:29: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/coverstore/db.py:130:29: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/data/db.py:157:29: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/data/sitemap.py:88:35: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/admin/code.py:616:30: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/ol_infobase.py:301:33: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/openlibrary/borrow_home.py:52:33: The function `utcfromtimestamp` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .fromtimestamp(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/openlibrary/borrow_home.py:68:33: The function `utcfromtimestamp` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .fromtimestamp(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/openlibrary/borrow_home.py:69:31: The function `utcfromtimestamp` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .fromtimestamp(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/openlibrary/code.py:1125:27: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/openlibrary/connection.py:213:50: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/openlibrary/infobase_hook.py:21:35: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/upstream/addbook.py:46:36: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/upstream/borrow.py:418:21: The function `utcfromtimestamp` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .fromtimestamp(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/upstream/borrow.py:524:77: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/upstream/code.py:213:52: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/plugins/worksearch/subjects.py:344:46: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/utils/bulkimport.py:80:39: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] openlibrary/utils/bulkimport.py:191:39: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
- Found 681 diagnostics
+ Found 719 diagnostics

meson (https://github.com/mesonbuild/meson)
+ warning[deprecated] mesonbuild/build.py:532:10: The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
+ warning[deprecated] mesonbuild/linkers/linkers.py:127:10: The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
- Found 909 diagnostics
+ Found 911 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ warning[deprecated] src/prefect/input/actions.py:46:49: The function `json` is deprecated: The `json` method is deprecated; use `model_dump_json` instead.
- Found 3739 diagnostics
+ Found 3740 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
+ warning[deprecated] ci/cleanup/aws.py:105:20: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] ci/cleanup/launchdarkly.py:32:24: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] ci/load/periodic.py:31:29: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] ci/load/periodic.py:36:40: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] misc/python/materialize/buildkite_insights/util/data_io.py:75:21: The function `utcfromtimestamp` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .fromtimestamp(datetime.timezone.utc)
+ warning[deprecated] misc/python/materialize/cli/cloudbench.py:290:44: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] misc/python/materialize/cli/scratch/create.py:148:40: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] misc/python/materialize/parallel_workload/action.py:80:46: The function `getName` is deprecated: Use the name attribute instead
+ warning[deprecated] misc/python/materialize/parallel_workload/action.py:1737:40: The function `getName` is deprecated: Use the name attribute instead
+ warning[deprecated] misc/python/materialize/parallel_workload/executor.py:109:50: The function `getName` is deprecated: Use the name attribute instead
+ warning[deprecated] misc/python/materialize/parallel_workload/worker.py:131:62: The function `getName` is deprecated: Use the name attribute instead
+ warning[deprecated] misc/python/materialize/scratch.py:418:34: The function `utcnow` is deprecated: Use timezone-aware objects to represent datetimes in UTC; e.g. by calling .now(datetime.timezone.utc)
+ warning[deprecated] test/rtr-combined/mzcompose.py:67:54: The function `getName` is deprecated: Use the name attribute instead
- Found 3237 diagnostics
+ Found 3250 diagnostics

sympy (https://github.com/sympy/sympy)
+ warning[deprecated] sympy/testing/runtests.py:1176:28: The class `Str` is deprecated: Replaced by ast.Constant; removed in Python 3.14
+ warning[deprecated] sympy/testing/runtests.py:1195:33: The class `Str` is deprecated: Replaced by ast.Constant; removed in Python 3.14
- Found 13539 diagnostics
+ Found 13541 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-07-18 08:08_

Rebased your branch, as I reviewed the changes in https://github.com/astral-sh/ruff/pull/19400 that caused the conflicts here. Hope that's fine

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-18 08:08_

---

_Comment by @github-actions[bot] on 2025-07-18 08:16_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `deprecated` | 240 | 0 | 0 |
| `call-non-callable` | 1 | 0 | 0 |
| `invalid-argument-type` | 1 | 0 | 0 |
| **Total** | **242** | **0** | **0** |

**[Full report with detailed diff](https://gankra-warn-dep.ecosystem-663.pages.dev/diff)**


---

_@sharkdp reviewed on 2025-07-18 08:31_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:3526 on 2025-07-18 08:31_

It feels like this is maybe too strict? The stub allows message to be a `LiteralString`, but here, we only allow arbitrary `Literal["…"]` string-literals, but not the common supertype `LiteralString`. Since `warnings.deprecated` also has a runtime effect, it seems fine to allow calls, even if we can't extract the message?

See this ecosystem hit in `altair` as an example where we currently emit this new diagnostic: https://github.com/vega/altair/blob/d30d030a89e2e2e72c6f5a4d497968b7dfb743bf/altair/utils/deprecation.py#L82

I would probably allow `message` to be `LiteralString` here, and return an instance of `deprecated` with some generic message?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:3526 on 2025-07-18 08:38_

I also think that this will be incorrect for very long strings because ty falls back to `LiteralString` for overlong strings:

https://github.com/astral-sh/ruff/blob/4aee0398cbed38b84e596b47a5e97e56aef5d725/crates/ty_python_semantic/src/types/infer.rs#L4964-L4968

I'm not entirely sure what the right behavior is in this case. We could look at the argument expression if the inferred type is a `LiteralString` to see if it's an overlong `ExprString` and, in that case, create a `StringLiteral` type. 

---

_@MichaReiser reviewed on 2025-07-18 08:38_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:5892 on 2025-07-18 08:42_

We commonly use backticks for variable message parts

```suggestion
            builder.into_diagnostic(format_args!(r#"The function `{func_name}` is deprecated"#));
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:5861 on 2025-07-18 08:42_


```suggestion
                builder.into_diagnostic(format_args!(r#"The class `{class_name}` is deprecated"#));
```

---

_@sharkdp reviewed on 2025-07-18 08:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:2322 on 2025-07-18 08:43_

You can fix this by adding a new
```rs
Type::KnownInstance(known_instance @ KnownInstanceType::Deprecated(_)) => {
    known_instance.instance_fallback(db).bindings(db)
}
```
branch to `Type::bindings`. The `continue` can then be removed here.

This will effectively lift the `def __call__(self, arg: _T, /) -> _T: ...` method of `warnings.deprecated` to the `KnownInstance` level.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6275 on 2025-07-18 08:43_

Can this just return `self`?
```suggestion
        self
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6258 on 2025-07-18 08:45_


```suggestion
    pub(crate) message: StringLiteralType<'db>,

    /// The `@warnings.deprecated(...)` invocation
    pub(crate) definition: Option<Definition<'db>>,
```

Is the `definition` used anywhere? If not, should we remove it or do you plan on using it in the diagnostic message?

---

_@sharkdp reviewed on 2025-07-18 08:56_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:8463 on 2025-07-18 08:56_

minor
```suggestion
                // run, so this bespoke additional validation is required here for a good diagnostic.
```

---

_@MichaReiser approved on 2025-07-18 08:57_

This overall looks good to me but I'm curious what the performance numbers will show. You'll have to fix the panic in the benchmark (review what new diagnostics there are) to get those numbers and push the updated benchmark. 

The other thing we have to look into is if using `StringLiteralType` is too strict and how we want to handle very long messages

---

_@sharkdp approved on 2025-07-18 09:01_

Thank you, this looks good!

Apart from the `invalid-argument-type` false positive, we should also look into this new diagnostic (also on `altair`):

```diff
altair (https://github.com/vega/altair)
+ error[call-non-callable] altair/vegalite/v6/theme.py:73:5: Object of type `warnings.deprecated` is not callable
```

---

_@Gankra reviewed on 2025-07-18 13:55_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/infer.rs`:2322 on 2025-07-18 13:55_

Oh perfect!

---

_@Gankra reviewed on 2025-07-18 13:59_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types.rs`:6275 on 2025-07-18 13:59_

The whole function can be removed, I figured it might be good to write it out in-case something gets added but... eh.

---

_@Gankra reviewed on 2025-07-18 14:00_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types.rs`:6258 on 2025-07-18 14:00_

It's totally useless right now, I figured it might be useful in the future and was presumably cheap to include. I can just remove it though.

---

_@Gankra reviewed on 2025-07-18 14:02_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/class.rs`:3526 on 2025-07-18 14:02_

The ~spec here says: https://typing.python.org/en/latest/spec/directives.html#deprecated

> The positional-only argument is of type str and contains a message that should be shown by the type checker when it encounters a usage of the decorated object. Tools may clean up the deprecation message for display, for example by using [inspect.cleandoc()](https://docs.python.org/3/library/inspect.html#inspect.cleandoc) or equivalent logic. **The message must be a string literal.** The content of deprecation messages is up to the user, but it may include the version in which the deprecated object is to be removed, and information about suggested replacement APIs.

---

_@Gankra reviewed on 2025-07-18 14:03_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/class.rs`:3526 on 2025-07-18 14:03_

But I can make it more lenient if it's messing up actual ecosystem crates.

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/class.rs`:3526 on 2025-07-18 14:24_

Typeshed also declares this as

```
        def __init__(self, message: LiteralString, /, *, category: type[Warning] | None = ..., stacklevel: int = 1) -> None: ...
```

---

_@Gankra reviewed on 2025-07-18 14:24_

---

_@Gankra reviewed on 2025-07-18 15:54_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/class.rs`:3526 on 2025-07-18 15:54_

Aha ok I have read more on LiteralString and I now understand and agree with this assessment.

---

_@carljm reviewed on 2025-07-18 16:32_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3526 on 2025-07-18 16:32_

> We could look at the argument expression if the inferred type is a `LiteralString` to see if it's an overlong `ExprString` and, in that case, create a `StringLiteral` type.

I don't think there's a need for such shenanigans. Super-long deprecation messages should be rare-to-nonexistent, I think it's fine to just use a fallback message in that case. If we see it in real code the best thing is probably just to increase our supported StringLiteral length.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6258 on 2025-07-18 16:46_

I'd remove this unless it's either used or we have a TODO for which it will be definitely needed (and I don't see that)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6275 on 2025-07-18 16:47_

Yeah I was wondering the same, I'd just remove this function entirely

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:805 on 2025-07-18 16:48_

Not sure what "this is `@warnings.deprecated`" is supposed to mean
```suggestion
    /// If this class is deprecated, this holds the deprecation message.
```

---

_@MichaReiser reviewed on 2025-07-18 16:53_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:3526 on 2025-07-18 16:53_

Fair, it's 4096 bytes (I assumed it would be much shorter)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3498 on 2025-07-18 16:54_

I suspect that in the future we will want to validate the entire call against the typeshed stub for `warnings.deprecated` so as to error on invalid additional arguments (even though we ignore them). Fine with leaving that as a TODO for now, though. (But I would add it as a TODO.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:277 on 2025-07-18 16:55_

```suggestion
    /// old_func()  # emits [deprecated] diagnostic
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3494 on 2025-07-18 16:59_

Am I missing something, or is this unused?

It looks like it might be copied from the TypeVar case below, but TypeVars are weird in that they care about the name they are assigned to; no such thing applies here.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:75 on 2025-07-18 17:01_

I'm pretty sure someone will file a bug about this sooner or later, and we really should fully validate the correctness of the call (even though we don't have special treatment for the other arguments). I think it's at least worth a TODO.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:2322 on 2025-07-18 17:03_

I don't think that new match arm needs to be limited to `KnownInstanceType::Deprecated`? It should be correct to fallback to the fallback type for any known-instance type.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:8465 on 2025-07-18 17:13_

I found this message pretty confusing when I saw it in the tests, because at runtime what happens with

```py
@warnings.deprecated
def f(): ...
```

is that `warnings.deprecated` is called with the function `f` as its sole argument, so you get a TypeError because the `message` argument is a function object, not a string (and `deprecated` checks the type of `message` internally and throws `TypeError` if it's not a string). Saying "missing message argument" implies that we had a call to `warnings.deprecated` with no `message` argument, which is not what's happening here.

I think if we add fallback-instance-type callability for `KnownInstance` types as discussed above, we should be able to remove this `check_decorator` thing entirely and just rely on our usual call checking to give that same argument type error as you get at runtime.

But if we think that matching the runtime error is too confusing (because users don't understand how decorators work) and we want to keep some special error handling here, we should make the special-case error message more explicit (e.g. "you forgot to call `warnings.deprecated`, please add parentheses and a deprecation message").

 I don't think I would agree with keeping this special case, though -- it doesn't really make sense to do it for `warnings.deprecated` when tons of other decorators could have identical failure modes and won't get this special handling.

---

_@carljm reviewed on 2025-07-18 17:16_

This is looking great!

---

_@carljm reviewed on 2025-07-18 17:19_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:8465 on 2025-07-18 17:19_

Hmm, I might have confused "checking the call of a `deprecated` instance" with "fully type-checking the call to `deprecated` itself". Fixing the former won't help here, it's fixing the latter (which I mentioned in a couple other comments as something that should at least be a TODO) which would.

---

_@carljm reviewed on 2025-07-18 17:23_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3494 on 2025-07-18 17:23_

Oh it's not unused, but it's all just to support adding the (unused) `definition` field to `DeprecatedInstance`. Yeah, I would just remove all this.

---

_@carljm reviewed on 2025-07-18 17:34_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:75 on 2025-07-18 17:34_

It looks like the issue here is that in `TypeInferenceBuilder::infer_call_expression` we have a different path for calling class constructors, and in that path we don't do any KnownClass handling, so we exempt some known classes from the normal constructor-call checking entirely, in which case they go into the default path that calls `Type::bindings` (which doesn't fetch constructor signatures, it just returns an accept-anything signature for most classes) and then does KnownClass special-cased call handling.

I think this should be integrated better, so that we can do the default constructor-call checking from typeshed _and_ do KnownClass special case handling layered on top of that, rather than having to choose one or the other. But that doesn't have to be in this PR.

---

_@carljm reviewed on 2025-07-18 17:34_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:8465 on 2025-07-18 17:34_

Even if we don't fix the constructor call checking in this PR, I would still be inclined to eliminate this special casing and just have it be a TODO to issue a diagnostic here through normal constructor call checks.

---

_@Gankra reviewed on 2025-07-18 20:34_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:75 on 2025-07-18 20:34_

I manually added (roughly) the signature of `KnownClass::Deprecated` to `Type::bindings` and removed basically all other special validation.

---

_Comment by @github-actions[bot] on 2025-07-18 22:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/ruff_benchmark/benches/ty.rs`:620 on 2025-07-18 22:09_

What are the two new diagnostics here? Are they legit?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/deprecated.md`:75 on 2025-07-18 22:11_

Yeah, that seems like a reasonable temporary solution for this PR!

Looks like this prose line in the mdtest is now obsolete and should be removed/changed? Maybe "We don't implement any special understanding of the other arguments, but we validate their names and types."

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4498 on 2025-07-18 22:25_

This fallback is valid for any known instance, we should implement it generally and not hardcode knowledge here about which specific ones are callable. For the ones that are not callable, we'll simply find that their class has no `__call__` in typeshed and continue to treat them as not-callable.

```suggestion
            Type::KnownInstance(known_instance) => {
                known_instance.instance_fallback(db).bindings(db)
            }

            Type::PropertyInstance(_)
```

---

_@carljm approved on 2025-07-18 22:26_

Modulo a couple small inline comments, this looks land-ready to me!

---

_@Gankra reviewed on 2025-07-18 23:23_

---

_Review comment by @Gankra on `crates/ruff_benchmark/benches/ty.rs`:620 on 2025-07-18 23:23_

Yeah it uses two deprecated time apis, same ones that the ecosystem checks flagged on a ton of packages.

---

_Comment by @Gankra on 2025-07-18 23:48_

Done.

---

_Merged by @Gankra on 2025-07-18 23:50_

---

_Closed by @Gankra on 2025-07-18 23:50_

---

_Branch deleted on 2025-07-18 23:50_

---
