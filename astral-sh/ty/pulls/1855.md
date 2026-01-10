```yaml
number: 1855
title: "[ty] Document ty features"
type: pull_request
state: merged
author: sharkdp
labels:
  - documentation
assignees: []
merged: true
base: main
head: david/document-ty-features
created_at: 2025-12-11T15:27:43Z
updated_at: 2025-12-15T13:21:21Z
url: https://github.com/astral-sh/ty/pull/1855
synced_at: 2026-01-10T02:34:11Z
```

# [ty] Document ty features

---

_Pull request opened by @sharkdp on 2025-12-11 15:27_

## Summary

Document some of ty's unique type system features and add some showcases of diagnostics.

Rendered version: https://shark.fish/ty-features/features/type-checking/



---

_Label `documentation` added by @sharkdp on 2025-12-11 15:27_

---

_Review comment by @sharkdp on `docs/features/language-server.md`:19 on 2025-12-11 20:31_

No, this is not in its final form.

---

_@sharkdp reviewed on 2025-12-11 20:31_

---

_Review comment by @carljm on `docs/features/type-checking.md`:40 on 2025-12-11 21:10_

I'm not sure if this implies any change to this section, but it's worth noting that all major type checkers have enough "implicit synthesized intersection" support that they support this example as well as we do. So this isn't a unique ty feature.

---

_Review comment by @carljm on `docs/features/type-checking.md`:46 on 2025-12-11 21:13_

I find this example slightly odd, because iterating over `Unknown` is also fine. It returns `Unknown`, whereas with the narrowing we iterate over `Iterable[object] & Unknown`, which returns `object & Unknown` (or it should -- it actually today returns a Todo type from calling an intersection) -- which simplifies to just `Unknown`. The intersection here would be more meaningful if it allowed us to have useful type information about some attributes that otherwise we wouldn't have?

I guess maybe I'm focusing on the wrong part of this sentence. The part that's unique to ty isn't "allows you to use `obj` as an iterable" (all type checkers do that), it's letting you use other attributes of the unknown type (no other type checker does that.)

---

_Review comment by @carljm on `docs/features/type-checking.md`:83 on 2025-12-11 21:19_

This is such a great example! No other type checker can do this fully. Pyrefly gets the closest (it's the only one that allows using the `name` attribute), but it still thinks `None` is a possible type. Might be interesting to extend the example such that excluding `None` becomes relevant? (It's not a type error to string-format `None`).

---

_Review comment by @carljm on `docs/features/type-checking.md`:84 on 2025-12-11 21:19_

I happened to notice that this playground link includes the `untyped_module.py` from the previous example still

---

_Review comment by @carljm on `docs/features/type-checking.md`:128 on 2025-12-11 21:24_

```suggestion
While this is correct, you might still want to distinguish between "actual" `Item`s and lists of
```

---

_Review comment by @carljm on `docs/features/type-checking.md`:199 on 2025-12-11 21:27_

```suggestion
    We are also planning to add a mode for users that prefer to have stricter types inferred by default in these
```

---

_@carljm reviewed on 2025-12-11 21:29_

This is great!

---

_@zsol reviewed on 2025-12-11 21:45_

---

_Review comment by @zsol on `docs/features/type-checking.md`:147 on 2025-12-11 21:45_

I liked that every single example & paragraph so far was genuinely relatable - it made reading this top to bottom flow really well. To me, this example didn't really add anything, and interrupted this flow. Maybe consider removing it and going straight to the pydantic example below?

---

_@zsol reviewed on 2025-12-11 21:47_

---

_Review comment by @zsol on `docs/features/type-checking.md`:204 on 2025-12-11 21:47_

The term "self-referential code" (and maybe even fixpoint iteration) could use some elaboration here IMO
Actually I wonder if we want users to learn the term "fixpoint iteration" - is the takeaway here mainly that ty can handle code that references itself?

---

_@carljm reviewed on 2025-12-11 21:48_

---

_Review comment by @carljm on `docs/features/type-checking.md`:147 on 2025-12-11 21:48_

Not relatable?? You don't use your type-checker to [brute-force Diophantine equations](https://play.ty.dev/00ac7e54-1e32-47b1-b18e-a786a6cee0b8)?

---

_@zsol reviewed on 2025-12-11 21:53_

---

_Review comment by @zsol on `docs/features/type-checking.md`:131 on 2025-12-11 21:53_

nit: consider changing the example to make `Item` `@final`, and adding an info box at the bottom about why it's there - it was a bit puzzling to read the code snippet, trying to work out what the type expression in the comment means before reading the answer below

---

_@zsol reviewed on 2025-12-11 21:58_

---

_Review comment by @zsol on `docs/features/type-checking.md`:147 on 2025-12-11 21:58_

Only when my coworkers force me to!

---

_@sharkdp reviewed on 2025-12-12 12:14_

---

_Review comment by @sharkdp on `docs/features/type-checking.md`:131 on 2025-12-12 12:14_

That's a great suggestion. Thank you.

---

_@sharkdp reviewed on 2025-12-12 12:22_

---

_Review comment by @sharkdp on `docs/features/type-checking.md`:46 on 2025-12-12 12:22_

> I guess maybe I'm focusing on the wrong part of this sentence. The part that's unique to ty isn't "allows you to use `obj` as an iterable" (all type checkers do that), it's letting you use other attributes of the unknown type (no other type checker does that.)

Yes, exactly. I tried to rephrase it to make that clearer.

---

_@sharkdp reviewed on 2025-12-12 12:23_

---

_Review comment by @sharkdp on `docs/features/type-checking.md`:40 on 2025-12-12 12:23_

Yes, I know :neutral_face:. I still found it useful to start with this basic example? The examples below demonstrate things that other type checkers can't do, but they are quite a bit more involved.

---

_@sharkdp reviewed on 2025-12-12 12:27_

---

_Review comment by @sharkdp on `docs/features/type-checking.md`:83 on 2025-12-12 12:27_

> Might be interesting to extend the example such that excluding `None` becomes relevant? (It's not a type error to string-format `None`).

I don't really understand what pyrefly is doing there, to be honest. It thinks that `being` is still of type `None`, but it still allows you to access `being.name`. But that attribute has a type of `Unknown`, so I guess there's no way to "force" pyrefly to fail here. If you look closely, it reveals a type of `Animal | Person | None (_.name: Unknown)`, so that `(_.name: Unknown)` part might be some sort of intersection-like behavior?

---

_@AlexWaygood reviewed on 2025-12-12 12:32_

---

_Review comment by @AlexWaygood on `docs/features/type-checking.md`:204 on 2025-12-12 12:32_

I might be tempted just to omit this section entirely... The fact that we use fixpoint iteration is interesting to other type-checker authors and compiler devs, but I'm not sure it will be that interesting to users. The _results_ of it _might_ be interesting to users -- it's kinda cool that we can infer `Literal[0, 1, 2, 3, 4] | Unknown` for `LoopingCounter.value`. But is that _significantly_ better than [mypy's inferred type](https://mypy-play.net/?mypy=latest&python=3.12&gist=5a85116acffab621fdc2df96ca6d5df9) of `int`, such that it's worth highlighting for users?

---

_Marked ready for review by @sharkdp on 2025-12-12 12:39_

---

_Review comment by @sharkdp on `docs/features/type-checking.md`:204 on 2025-12-12 12:40_

Talked about the same topic with Carl yesterday. We agree that it's not very useful, but it's just extremely cool, and I don't think there's any harm in leaving this at the bottom of this document here. I rephrased this section to hopefully make it clearer.

---

_@sharkdp reviewed on 2025-12-12 12:40_

---

_Review comment by @AlexWaygood on `docs/features/type-checking.md`:107 on 2025-12-12 14:26_

there _are_ ways of using this even if you support multiple type checkers, though it obviously gets more fiddly. For example, this snippet passes mypy out-of-the-box, because mypy treats any `MYPY` variables as always-true, the same as it does for `TYPE_CHECKING`. It doesn't pass pyright out of the box, but I believe it _would_ pass pyright if you had a pyright configuration file with `{"PYRIGHT": true}` specified for the `defineConstant` setting (https://microsoft.github.io/pyright/#/configuration?id=environment-options).

```py
from typing import TYPE_CHECKING

MYPY = False

class Serializable: ...
class Versioned: ...

if TYPE_CHECKING:
    if MYPY or PYRIGHT:
        type SerializableVersioned = Serializable
    else:
        from ty_extensions import Intersection
    
        type SerializableVersioned = Intersection[Serializable, Versioned]

def output_as_json(obj: SerializableVersioned) -> str:
    return "foo"
```

not sure whether or not it's worth mentioning this; it might be too complicated for it to be worth it

---

_@AlexWaygood reviewed on 2025-12-12 14:26_

---

_@sharkdp reviewed on 2025-12-12 15:00_

---

_Review comment by @sharkdp on `docs/features/type-checking.md`:107 on 2025-12-12 15:00_

Right. I guess we might include something like this if we have our own way of specifying a "this is being type checked by ty" test, but excluding every other type checker that you're using feels like a stretch.

---

_@AlexWaygood reviewed on 2025-12-12 15:11_

---

_Review comment by @AlexWaygood on `docs/features/type-checking.md`:107 on 2025-12-12 15:11_

Yeah. I feel quite sure we have an issue open _somewhere_ to add a ty equivalent of the mypy/pyright setting that enables this feature (the mypy config option is https://mypy.readthedocs.io/en/stable/config_file.html#confval-always_true). But who knows where that issue is in our list of 543...

---

_@carljm reviewed on 2025-12-12 21:16_

---

_Review comment by @carljm on `docs/features/type-checking.md`:83 on 2025-12-12 21:16_

Pyrefly is just applying its equivalent of our "place" narrowing. So it still thinks the type of `being` is `Animal | Person | None`, but it additionally knows that `being.name` exists with type `Unknown`.

---

_@carljm reviewed on 2025-12-12 21:19_

---

_Review comment by @carljm on `docs/features/type-checking.md`:83 on 2025-12-12 21:19_

> I guess there's no way to "force" pyrefly to fail here

What I was thinking of was not something based on the type of `being.name`, but something based on us knowing that `None` is excluded. For example, if `Person` and `Animal` both had a `greet` method, which we call as part of printing the greeting. This call would fail on `None` but succeed on `Person` and `Animal`.

---

_@carljm reviewed on 2025-12-12 21:19_

---

_Review comment by @carljm on `docs/features/type-checking.md`:40 on 2025-12-12 21:19_

Yes, I think that makes sense.

---

_@sharkdp reviewed on 2025-12-12 21:54_

---

_Review comment by @sharkdp on `docs/features/type-checking.md`:83 on 2025-12-12 21:54_

> What I was thinking of was not something based on the type of `being.name`, but something based on us knowing that `None` is excluded.

:+1: 

> For example, if `Person` and `Animal` both had a `greet` method, which we call as part of printing the greeting. This call would fail on `None` but succeed on `Person` and `Animal`.

Hm, I think I would find that a little contrived? Why would you check `hasattr(…, "name")` only to call a `.greet` method on the object? Or would we pass in the object's name into its own `.greet` method? That seems weird on its own... and I would still find it strange that the code relies on the implicit knowledge that every class in that union that has a `name` attribute also has a `greet` method. Happy to change it though if we can come up with something that looks a little bit more like it could be realistic code.

What's also related to the exclusion of `None` is the fact that we can infer the type of `being.name` as `str` as soon as you make `Animal` final. All other type checkers either infer `Any` or a union `Unknown | str`. Maybe that's something we could mention?

---

_@sharkdp reviewed on 2025-12-13 09:05_

---

_Review comment by @sharkdp on `docs/features/type-checking.md`:83 on 2025-12-13 09:05_

I added a new "Info: …" box that explains this, and changed the playground example to include a comment that encourages readers to play with adding `@final`.

---

_Review comment by @danielhollas on `docs/features/type-checking.md`:171 on 2025-12-14 01:31_

Should Gradual guarantee be at the top? Compared to the other somewhat more theoretical concepts, this one seems like it has very clear implications on how ty behaves in general.

(Sorry for a drive by comment, this document is super cool. Looking forward to the beta release! 🚀)

---

_@danielhollas reviewed on 2025-12-14 01:31_

---

_@sharkdp reviewed on 2025-12-15 07:32_

---

_Review comment by @sharkdp on `docs/features/type-checking.md`:171 on 2025-12-15 07:32_

Thank you for your comment!

It's somewhat deliberately *not* at the top, because we're not 100% sure yet if our current behavior will be the same once we release a stable version. If the answer is yes, we can/should certainly move this section to the top.

---

_@AlexWaygood approved on 2025-12-15 10:35_

---

_Merged by @sharkdp on 2025-12-15 10:36_

---

_Closed by @sharkdp on 2025-12-15 10:36_

---

_Branch deleted on 2025-12-15 10:36_

---

_@MichaReiser approved on 2025-12-15 10:38_

---

_Comment by @sharkdp on 2025-12-15 10:50_

@zanieb I merged this already, but feel free to add more review comments, and I will address them in a follow-up. I wanted to publish the latest version, but I don't have the necessary permissions to merge https://github.com/astral-sh/docs/pull/308. See these pages for the latest rendered version:

* https://ca6eabdc.docs-119.pages.dev/ty/features/type-checking/
* https://ca6eabdc.docs-119.pages.dev/ty/features/diagnostics/

---

_Review comment by @zsol on `docs/features/type-checking.md`:93 on 2025-12-15 13:10_

nit: `If ty is the only type checker you use` (you can use ty as a type checker as well as a code navigation tool)

---

_@zsol reviewed on 2025-12-15 13:10_

---

_@sharkdp reviewed on 2025-12-15 13:21_

---

_Review comment by @sharkdp on `docs/features/type-checking.md`:93 on 2025-12-15 13:21_

Thank you => https://github.com/astral-sh/ty/pull/1890

---
