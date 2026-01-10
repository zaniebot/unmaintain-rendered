```yaml
number: 111
title: Advanced dataclass support
type: issue
state: open
author: sharkdp
labels:
  - dataclasses
assignees: []
created_at: 2025-04-28T08:13:21Z
updated_at: 2025-12-13T17:30:11Z
url: https://github.com/astral-sh/ty/issues/111
synced_at: 2026-01-10T01:54:59Z
```

# Advanced dataclass support

---

_Issue opened by @sharkdp on 2025-04-28 08:13_

We already have initial support for dataclasses, but some more advanced features are still missing. The following list is most probably not complete:

- [x] Make sure that `dataclass` can be used as a non-decorator: `class C: …; C = dataclass(C)`. See empty [test here](https://github.com/astral-sh/ruff/blob/dbc137c9516808baa292da7a928e50ba36a14d39/crates/red_knot_python_semantic/resources/mdtest/dataclasses.md?plain=1#L703C1-L703C6).
- [x] Emit an error if >=2 variables in a dataclass class body are annotated with `KW_ONLY` (failing test [here](https://github.com/astral-sh/ruff/blob/913f136d33fbc1b7d20fc91190431ccef2e2834c/crates/ty_python_semantic/resources/mdtest/dataclasses.md?plain=1#L744-L757))
- [x] Support for `dataclasses.KW_ONLY`
- [x] Support for `Final[…]` fields and `ClassVar[Final[…]]` fields.
- [x] Improve support for frozen dataclasses to [handle subclasses of frozen dataclasses also](https://github.com/astral-sh/ruff/pull/17974#discussion_r2101813542)
- [x] Support for `dataclasses.InitVar` https://github.com/astral-sh/ruff/pull/19527
- [x] Support for [dataclass `field`s](https://docs.python.org/3/library/dataclasses.html#dataclasses.field)
- [x] Add support for [other synthesized functions / arguments](https://github.com/astral-sh/ruff/blob/dbc137c9516808baa292da7a928e50ba36a14d39/crates/red_knot_python_semantic/resources/mdtest/dataclasses.md?plain=1#L366-L388)
  - [x] unsafe_hash
  - [x] match_args
  - [x] kw_only
  - [x] slots: astral-sh/ruff#20278
  - [x] weakref_slot
  - [x] The synthesized `__replace__` method on Python 3.13+ https://github.com/astral-sh/ruff/pull/19545
- [ ] Emit diagnostic when defining a field without a default after a field with a default, see [existing TODO](https://github.com/astral-sh/ruff/blob/dbc137c9516808baa292da7a928e50ba36a14d39/crates/red_knot_python_semantic/resources/mdtest/dataclasses.md?plain=1#L120): https://github.com/astral-sh/ruff/pull/19825
- [ ] Emit diagnostic when setting `order=True` on a dataclass that has a custom `__lt__` (or similar), see [existing TODO](https://github.com/astral-sh/ruff/blob/dbc137c9516808baa292da7a928e50ba36a14d39/crates/red_knot_python_semantic/resources/mdtest/dataclasses.md?plain=1#L361)
- [ ] Verify signature of methods such as `__post_init__`, see [this comment](https://github.com/astral-sh/ruff/pull/19527#discussion_r2228364684)
- [ ] Emit diagnostic if `frozen=True` but the class has a custom `__setattr__` or `__delattr__` method in the class body (this raises `TypeError` at runtime) https://github.com/astral-sh/ruff/pull/21430
- [x] Add support for `dataclasses.field(kw_only=True)`

---

_Comment by @AlexWaygood on 2025-04-28 10:41_

We'll eventually want to support a generalised feature for slotted classes, where something like this is reported as an error:

```py
class Foo:
    __slots__ = ("a,")
    b: int
```

`Foo` cannot declare `b` as an instance variable here, because `Foo` is a slotted class and `b` is not present in `Foo.__slots__`; an assignment such as `Foo().b = 42` will therefore fail. When implementing that feature, we'll want to make sure that we understand slotted dataclasses (both those that use `slots=True` and those that manually define `__slots__` in the class namespace) consistently with how we understand other slotted classes.

---

_Comment by @thejchap on 2025-05-04 02:39_

@sharkdp once i wrap https://github.com/astral-sh/ruff/pull/17697 i'd be interested in taking a look at this, in particular:
> Add support for frozen dataclasses

would it make sense to spin up individual issues for each of these features?

---

_Comment by @sharkdp on 2025-05-04 05:59_

I think it's a good idea to attempt to solve these items separately. I think it's fine to keep just this one ticket but implement it in several PRs. However, if you think that there is significant discussion needed for a sub-item,  please feel free to create additional issues and we can link them here.

---

_Renamed from "[red-knot] Advanced dataclass support" to "Advanced dataclass support" by @MichaReiser on 2025-05-07 15:25_

---

_Label `dataclasses` added by @AlexWaygood on 2025-05-10 17:59_

---

_Removed from milestone `GA` by @carljm on 2025-06-11 00:45_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:45_

---

_Comment by @abhijeetbodas2001 on 2025-06-12 13:20_

> Support for [dataclass fields](https://docs.python.org/3/library/dataclasses.html#dataclasses.field)

I'd like to work on this. I have a way in mind, but would like to get it validated:

In typeshed, `field` is annotated as returning the type of the `default` argument (or of `default_factory`'s return), and *not* a `Field` instance (which it will be at runtime):

```py
# NOTE: Actual return type is 'Field[_T]', but we want to help type checkers
# to understand the magic that happens at runtime.
if sys.version_info >= (3, 14):
    @overload  # `default` and `default_factory` are optional and mutually exclusive.
    def field(
        *,
        default: _T,
        default_factory: Literal[_MISSING_TYPE.MISSING] = ...,
        # more kwargs
    ) -> _T: ...
```

We need access to the value of, for example, `kw_only` when we synthesize the `__init__` method, but we do not have it right now. All we have is `_T`.

I think we'd need to special case the inference of `field(...)` calls to consider their type as `Field[_T]` instead of `_T`, and then do another bit of special-casing in `is_assignable_to`, to consider `Field[_T]` to be assignable to `_T`.
That way we will internally have access to all of the `field` call's params.
Does that seem reasonable?

---

_Comment by @carljm on 2025-06-13 21:24_

@abhijeetbodas2001 Thanks!

It seems like that approach will also require the `Field` type to be a special-cased `Type` variant (something like `DataclassField`, similar to e.g. `DataclassTransformer` or `DataclassDecorator`), so it can carry along the parameter values from the `field` call? Simply having a `NominalInstance` of `dataclasses.Field` as defined in typeshed wouldn't give us that information.

It's also important that we design our approach here so it works for other `dataclass_transform` users, not just for stdlib dataclasses. This means that any special-casing we apply should be applied to any function/class listed in the `field_specifiers` option of `dataclass_transform` (https://typing.python.org/en/latest/spec/dataclasses.html#dataclass-transform-parameters ), not just to `dataclasses.field`.

I could also see an argument for possibly tackling something like `dataclasses.KW_ONLY` support first. It's also an important feature to support, and it's notable because it depends on lexical ordering of declarations, which isn't totally trivial to obtain in our model. It might be useful to understand the changes required to support this, before doing full field support?

@sharkdp implemented our dataclass support and may have other thoughts here.

---

_Comment by @abhijeetbodas2001 on 2025-06-14 17:02_

Yup, it will need a new `Type` variant.

Re: non-stdlib field specifiers,
The current implementation of `dataclass_transformer` does not store `field_specifiers`, and even once it does, there'd be some work needed to treat a given `field_specifier` as one.
But modulo that, I think a single `Type` variant for `Field` should be able to serve both the stdlib's and 3rd party field specifiers, because the parameters of 3rd party field specifiers which type checkers are expected to understand [are a superset of stdlib's `field` parameters](https://typing.python.org/en/latest/spec/dataclasses.html#field-specifier-parameters). But maybe I'm jumping the gun here 😄 , things will be more clear once I have a draft.

---

> It's also an important feature to support, and it's notable because it depends on lexical ordering of declarations, which isn't totally trivial to obtain in our model.

I did not understand this. I thought, the `public_places` and similar structures do keep track of declarations in the order they appear in the code? Wouldn't the order be also required for correctly synthesizing the `__init__` method, for example (which we already support)?

---

_Comment by @carljm on 2025-06-17 01:58_

> I thought, the `public_places` and similar structures do keep track of declarations in the order they appear in the code? Wouldn't the order be also required for correctly synthesizing the `__init__` method, for example (which we already support)?

Yes, you're right, I wasn't thinking clearly about this; we already do preserve lexical order, and it's necessary for `__init__` synthesis. Thanks anyway for the PR implementing `KW_ONLY`!

---

_Comment by @carljm on 2025-06-17 02:10_

#657 has a good Pydantic example that should be solved by dataclass fields support (specifically, support for the `alias` parameter to a field constructor.)

---

_Comment by @sharkdp on 2025-06-17 09:42_

> Yes, you're right, I wasn't thinking clearly about this; we already do preserve lexical order, and it's necessary for `__init__` synthesis.

I believe part of the confusion here may come from the use of the term "lexical order" which is also a synonym for ["lexicographic order"](https://en.wikipedia.org/wiki/Lexicographic_order), and that would imply some sort of alphabetical ordering of fields. I think everyone here is referring to "order of appearance" / "source code order".

---

_Comment by @carljm on 2025-06-17 15:09_

I think everyone was talking about source order, so there was no confusion about the meaning of "lexical". The confusion arose solely from me being wrong! I was forgetting that the use-def map does already guarantee returning definitions in source order, and we already rely on that for synthesizing `__init__`.

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:00_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:00_

---

_Comment by @sharkdp on 2025-11-14 10:11_

https://github.com/astral-sh/ruff/pull/21446 added support for `__weakref__` and `__match_args__`.

---

_Comment by @carljm on 2025-11-14 17:38_

It looks to me like the remaining listed features are all additional diagnostics we could emit, not features that impact our understanding of the dataclass itself. Does that seem accurate?

That's why I have this as only a P2 for stable; it would be nice to get these additional diagnostics in if it isn't hard, but it doesn't seem critical.

---

_Comment by @sharkdp on 2025-11-15 10:46_

> It looks to me like the remaining listed features are all additional diagnostics we could emit, not features that impact our understanding of the dataclass itself. Does that seem accurate?

That does seem accurate, yes. I marked `unsafe_hash` as done yesterday because I thought we wouldn't need to do anything. I read the documentation again now, and decided that we could easily model the actual semantics here (I opened https://github.com/astral-sh/ruff/pull/21470).



> That's why I have this as only a P2 for stable; it would be nice to get these additional diagnostics in if it isn't hard, but it doesn't seem critical.

👍 

---
