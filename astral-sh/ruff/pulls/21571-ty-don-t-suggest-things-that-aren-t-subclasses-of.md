```yaml
number: 21571
title: "[ty] Don't suggest things that aren't subclasses of `BaseException` after `raise`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/filter-non-exceptions-after-raise
created_at: 2025-11-21T19:36:27Z
updated_at: 2025-11-24T17:55:32Z
url: https://github.com/astral-sh/ruff/pull/21571
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Don't suggest things that aren't subclasses of `BaseException` after `raise`

---

_@BurntSushi_

This only applies to items that have a type associated with them. That
is, things that are already in scope. For items that don't have a type
associated with them (i.e., suggestions from auto-import), we still
suggest them since we can't know if they're appropriate or not. It's not
quite clear on how best to improve here for the auto-import case. (Short
of, say, asking for the type of each such symbol. But the performance
implications of that aren't known yet.)

Note that because of auto-import, we were still suggesting
`NotImplemented` even though astral-sh/ty#1262 specifically cites it as
the motivating example that we *shouldn't* suggest. This was occuring
because auto-import was including symbols from the `builtins` module,
even though those are actually already in scope. So this PR also gets
rid of those suggestions from auto-import.

Overall, this means that, at least, `raise NotImpl` won't suggest
`NotImplemented`.

Fixes astral-sh/ty#1262

---

_Review requested from @carljm by @BurntSushi on 2025-11-21 19:36_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-11-21 19:36_

---

_Review requested from @sharkdp by @BurntSushi on 2025-11-21 19:36_

---

_Review requested from @dcreager by @BurntSushi on 2025-11-21 19:36_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-11-21 19:36_

---

_Renamed from "ag/filter non exceptions after raise" to "[ty] Don't suggest things that aren't subclasses of `BaseException` after `raise`" by @BurntSushi on 2025-11-21 19:37_

---

_Review request for @dcreager removed by @BurntSushi on 2025-11-21 19:37_

---

_Review request for @carljm removed by @BurntSushi on 2025-11-21 19:37_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-11-21 19:37_

---

_Label `server` added by @BurntSushi on 2025-11-21 19:37_

---

_Label `ty` added by @BurntSushi on 2025-11-21 19:37_

---

_Comment by @BurntSushi on 2025-11-21 19:37_

Demo:

https://github.com/user-attachments/assets/94fd6788-c6d2-4bc3-9f5b-b3ed210268ee



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 19:39_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 19:41_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @Gankra on `crates/ty_ide/src/completion.rs`:1417 on 2025-11-21 20:21_

Wow I need to review more of your PRs, y'all are wildin' in here. I assume you can't more-normally walk up the AST because autocomplete has to operate in malformed ASTs more?

---

_@Gankra approved on 2025-11-21 20:23_

On paper it makes sense? 

---

_@BurntSushi reviewed on 2025-11-21 21:40_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1417 on 2025-11-21 21:40_

I think it's more that I've been operating under the presumption that "if we can do things directly from the tokens, then that's probably better." In part to avoid the problem you bring up (although I've found our AST to be pretty good at dealing with malformed input) but also because it's very cheap to just look at a few tokens. It's possible that doing an AST traversal is also cheap enough. I honestly don't have a great sense of it yet, but for completions we do a lot of "check if we're in context foo, or bar, or baz, or quux..." so the faster each of those checks are (which I assume will continue to grow over time), the better. Although I did just remove a bunch of those checks for imports and consolidated it into one single check.

TL;DR - We could probably look at the AST for cases like this, but I'm starting simple.

---

_@MichaReiser reviewed on 2025-11-22 14:05_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:1417 on 2025-11-22 14:05_

Walking tokens is certainly faster, but walking the AST also isn't crazy expensive (all Ruff lint rules just do that). That's why I'd pick whatever is easier. 

The bigger challenge with our AST is that you can't walk upwards (or sideways). At least not without building another intermediate representation that stores upward pointers for each AST node

One downside of using tokens is that there are cases where the parsed AST and token stream can disagree. E.g. the parser sometimes synthesizes name expressions if a required expression is missing (`e.g. a.` should synthesize a name expression for the attribute). However, we don't synthesize a name token in that case). 

However, our lexing is, to some extent, parser-directed during error recovery. For example, a "normal" lexer would parse the whitespace before `pass` as such whitespace because there's an unclosed `(`. This is not the case for our token stream because the parser will inform the lexer that it will start error recovery after `(` because `pass` isn't a valid argument name and that the lexer should try to lex the current token, assuming it's in a statement context, in which case the whitespace is lexed as an indent.

```py
def test(
	pass
```

https://play.ruff.rs/f1d64305-c9ff-47cc-8edf-3a7216871932

However, there are a few cases where the parsed AST and the token stream can disagree, e.g. the token stream doesn't contain a newline before `pass` even though this is parsed as `def test():\npass`


```py
def test(pass
```

https://play.ruff.rs/d9e759b6-d676-4871-b421-abbd58514fc9

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1383 on 2025-11-22 14:08_

```suggestion
    // But we may not always want to treat it specially. So we're
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:415 on 2025-11-22 14:11_

I suppose you can also raise instances of exceptions. So if a user has something like

```py
exception_to_raise = NotImplementedError("foooooooooooooooooooooooooooo")
raise exception_to<CURSOR>
```

we should probably suggest `exception_to_raise` there. That implies that this should be

```suggestion
    if is_raising_exception(tokens) {
        let raisable_type = UnionType::from_elements(
            db,
            [KnownClass::BaseException.to_subclass_of(db), KnownClass::BaseException.to_instance(db)]
        );
        completions.retain(|c| {
            let Some(ty) = c.ty else { return true };
            ty.is_assignable_to(db, raisable_type)
        });
    }
```

basically, the same as this bit in the type-inference builder:

https://github.com/astral-sh/ruff/blob/3410041b4ca8226641b72e2f947ebd612dd2ea28/crates/ty_python_semantic/src/types/infer/builder.rs#L6003-L6025

---

_@MichaReiser reviewed on 2025-11-22 14:22_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:1427 on 2025-11-22 14:22_

I think you want to, at least, skip all trivia tokens and parentheses (`(`, `)`)) to properly support

```py
raise (
	a  # comment
	.Error
)
```

---

_@AlexWaygood reviewed on 2025-11-22 14:25_

Thank you! This looks fantastic.

Though... the more I think about this, the more edge cases I think of. What if a user has something like this?

```py
import jsonschema.exceptions

if condition:
    raise jsons<CURSOR>
```

The user wants to type `raise jsonschema.exceptions.SchemaError`, but we refuse to suggest `jsonschema` here, and even after they've typed `jsonschema.<CURSOR>`, we refuse to suggest `jsonschema.exceptions`, since `jsonschema` and `jsonschema.exceptions` both have module-literal types, and module-literal types aren't assignable to `type[BaseException] | BaseException`.

I can think of three ways round this:
1. Make things more complicated: say that we'll also include module-literal types after `raise` keywords
2. Make things simpler: rather than trying to do a full analysis of which types make sense after a `raise` keyword in the autocompletion engine, just special-case a few symbols (like `NotImplemented`) that we _know_ don't make sense after a `raise` keyword, and make sure that they're removed.
3. Somewhere in the middle? Rather than _removing_ types that aren't assignable to `type[BaseException] | BaseException` from the list of suggestions, just make sure that they're all ranked below symbols that _are_ assignable to `type[BaseException] | BaseException`. And maybe _also_ special-case `NotImplemented` so that it's removed entirely?

I think I'm leaning towards (3). (1) just feels like it has too many edge cases attached to it, because there are lots of ways of creating nested namespaces in Python (not just modules and submodules!). Something like this is unusual in Python, but I can imagine it would be pretty surprising if a user tried it and then found they didn't have the autocompletions they expected (they want to type `raise Namespace.Exception1`):

```py
class Namespace:
    class Exception1(ValueError): ...
    class Exception2(TypeError): ...
    class Exception3(RuntimeError): ...

raise Name<CURSOR>
```

(3) also seems like it'll have a lot less complexity associated with it than (1)

---

_@BurntSushi reviewed on 2025-11-22 15:29_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1417 on 2025-11-22 15:29_

Thanks for that explanation! I didn't know some of that. I think sticking with tokens for now is fine. We can always switch to AST in response to user feedback.

---

_Comment by @BurntSushi on 2025-11-22 15:31_

@AlexWaygood Great points. I like (3) too. I think that also fits better with symbols from auto-import not having a type associated with it. So it recasts the problem more as a ranking problem than a "don't show symbols" problem.

---

_Comment by @BurntSushi on 2025-11-24 16:47_

@AlexWaygood OK, this should be updated now to do your (3) idea. Review is appreciated, particularly on how I did the check for `NotImplemented`. :-)

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:414 on 2025-11-24 16:57_

I think here you can simplify this a bit -- the big reason that folks mix up `NotImplemented` and `NotImplementedError` all the time is that they not only have very similar names, they're also both builtins. `type(NotImplemented)` doesn't really have the same issue, because it's not exposed as a builtin (you have to import the `NotImplementedType` class from the `types` module)

```suggestion
        // As a special case, and because it's a common footgun, we
        // specifically disallow `NotImplemented` in this context.
        // `NotImplementedError` should be used instead. So if we can
        // definitively detect `NotImplemented`, then we can safely
        // omit it from suggestions.
        completions.retain(|item| {
            let Some(ty) = item.ty else { return true };
            !ty.is_notimplemented(db)
        });
```

this will of course require making yet _another_ `ty_python_semantic` API `pub` rather than `pub(crate)` 😆

https://github.com/astral-sh/ruff/blob/a57e29131125bf05db7379e90c7616eec32624fe/crates/ty_python_semantic/src/types.rs#L895-L897

---

_@AlexWaygood approved on 2025-11-24 17:02_

This is awesome!! It makes me ridiculously happy that `NotImplemented` is no longer suggested after `raise`, and that all the exceptions are suggested first here:

<img width="1354" height="706" alt="image" src="https://github.com/user-attachments/assets/9acc4412-22cb-4692-b679-a3c8d7f63234" />

😃

---

_@BurntSushi reviewed on 2025-11-24 17:15_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:414 on 2025-11-24 17:15_

Yeah I guess I was thinking about

```
class Tricksy(NotImplementedType): pass
```

But I guess it's probably not worth that and better to focus on the specific footgun?

---

_@BurntSushi reviewed on 2025-11-24 17:17_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:414 on 2025-11-24 17:17_

My approach also covered
```
tricksy = NotImplemented
raise tricksy
```
But probably also not worth handling that?

Especially since both of my examples will get down-ranked anyway.

---

_@AlexWaygood reviewed on 2025-11-24 17:18_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:414 on 2025-11-24 17:18_

yeah, I think I'd just focus on the known footgun -- apart from anything else, `NotImplementedType` cannot be subclassed, so the user will get an error about `Tricksy` at runtime and ty will also complain about the invalid class definition: https://play.ty.dev/03244fb4-7141-43e4-baf8-240d405ac1d3

---

_@AlexWaygood reviewed on 2025-11-24 17:20_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:414 on 2025-11-24 17:20_

> My approach also covered
> 
> ```
> tricksy = NotImplemented
> raise tricksy
> ```

that's also covered by my suggested approach! `tricksy` has the same type as the `NotImplemented` singleton here

---

_@BurntSushi reviewed on 2025-11-24 17:26_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:414 on 2025-11-24 17:26_

Interesting. For whatever reason, the type that completions sees for `tricksy` is `Never`:

```
[crates/ty_ide/src/completion.rs:413:17] item = Completion {
    name: Name("tricksy"),
    insert: None,
    ty: Some(
        Never,
    ),
    kind: None,
    module_name: None,
    import: None,
    builtin: false,
    is_type_check_only: false,
    is_definitively_raisable: true,
    documentation: None,
}
```

Example:

https://github.com/user-attachments/assets/fc93503a-6b8f-496c-98e5-8ee557004c8e



---

_@BurntSushi reviewed on 2025-11-24 17:27_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:414 on 2025-11-24 17:27_

I think it has something to do with it being used in a `raise` context?

---

_@BurntSushi reviewed on 2025-11-24 17:28_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:414 on 2025-11-24 17:28_

(This is for my own edification at this point. I updated the PR to use your approach. :))

---

_@AlexWaygood reviewed on 2025-11-24 17:32_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:414 on 2025-11-24 17:32_

> Interesting. For whatever reason, the type that completions sees for `tricksy` is `Never`:

hmm... that seems incorrect; there must be a bug somewhere. (Where it is I don't know -- probably in `ty_python_semantic` rather than `ty_ide`, though it might well be in `ide_support.rs` rather than one of the "core type-inference" parts of that crate.)

> I think it has something to do with it being used in a `raise` context?

yes, that seems likely

---

_Merged by @BurntSushi on 2025-11-24 17:55_

---

_Closed by @BurntSushi on 2025-11-24 17:55_

---

_Branch deleted on 2025-11-24 17:55_

---
