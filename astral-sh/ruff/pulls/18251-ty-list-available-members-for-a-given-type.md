```yaml
number: 18251
title: "[ty] List available members for a given type"
type: pull_request
state: merged
author: sharkdp
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: david/code-completion-members
created_at: 2025-05-22T10:56:45Z
updated_at: 2025-05-30T17:27:30Z
url: https://github.com/astral-sh/ruff/pull/18251
synced_at: 2026-01-12T15:56:15Z
```

# [ty] List available members for a given type

---

_@sharkdp_

## Summary

Provide a new `ide_support::all_members` function that lists all available attributes for a given type.

To do:
- [ ] Probably respect `__all__` when accessing modules?
- [ ] Decide if we want to keep `ty_extensions.all_members` and the new `<str_literal> in <tuple>` inference support, that is probably only useful for this purpose.
- [ ] More tests

## Test Plan

New Markdown tests

---

_Label `ty` added by @sharkdp on 2025-05-22 10:56_

---

_Comment by @github-actions[bot] on 2025-05-22 10:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp reviewed on 2025-05-22 11:33_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:197 on 2025-05-22 11:33_

This might be an interesting correctness-vs-UX tradeoff. When given a `obj: str | None` object, and tab-completing on `obj.<tab>`, it's technically correct to only show attributes that are available on `str` *and* `None` (so basically nothing, except for a few special methods on `object` that nobody wants to call directly).

But the more likely scenario is probably that the developer actually wants to access an attribute on `str`, and just forgot to eliminate the `None` possibility. So it might be more useful to show attributes that are available on `str` *or* `None` here instead? Not sure.

---

_Label `server` added by @sharkdp on 2025-05-22 11:49_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:197 on 2025-05-22 16:35_

I feel like this is a semantic/definition problem. `all_members()` is defined as:

> The `ty_extensions.all_members` function allows access to a list of accessible members/attributes on a given object.

...and that is exactly what it should do, no special cases allowed.

Cases like `str | None` demand a different function that would list members that are available on <em>at least one</em> union member.

---

_@InSyncWithFoo reviewed on 2025-05-22 16:35_

---

_@sharkdp reviewed on 2025-05-22 18:26_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:197 on 2025-05-22 18:26_

Well I would change that docstring obviously, *if* we would decide to deliberate return members that are not available :smile: This function is only here to support that precise use case (providing completion results for the LPS). Making it available via `ty_extensions.all_members` was just a convenient way for me to test it. I'm not sure if this should really be exposed. Or if we should maybe move it to `ty_extensions.internal` or similar.

---

_Comment by @BurntSushi on 2025-05-29 12:00_

> Decide if we want to keep ty_extensions.all_members and the new <str_literal> in <tuple> inference support, that is probably only useful for this purpose.

I think this seems like the main blocker here for getting this merged. I'd be in favor of moving it to `ty_extensions.internal`, and if a use case arises to expose it in `ty_extensions` proper, then we could consider moving it then. Are there any downsides to exposing it in `ty_extensions.internal`?


---

_@BurntSushi reviewed on 2025-05-29 17:18_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:197 on 2025-05-29 17:18_

I'm inclined to stick to what you have here for now (preferring correctness), and then loosen it up as use cases arise. Maybe, e.g., it makes sense to only loosen it in certain scenarios.

---

_Comment by @BurntSushi on 2025-05-29 19:01_

> Probably respect `__all__` when accessing modules?

From what I can tell, it _might_ be undesirable to respect `__all__`, which I am loosely basing on [this comment](https://github.com/microsoft/vscode-python/issues/494#issuecomment-354901034).

However, we _do_ want to respect it when there's a glob-star import, but ty already does that (and I've added tests for it).

A relayed question here is whether we should respect `__dir__` when defined on objects. I suspect not, since the cases where it's useful seem likely to be dynamic? Although I haven't done a survey.

> the new `<str_literal> in <tuple>` inference support, that is probably only useful for this purpose

This I am not sure about. How do we go about deciding these sorts of questions?

Otherwise, I think a review at this point would be helpful for me, so I'm going to take this out of draft.

---

_Marked ready for review by @BurntSushi on 2025-05-29 19:01_

---

_Review requested from @carljm by @BurntSushi on 2025-05-29 19:01_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-05-29 19:01_

---

_Review requested from @dcreager by @BurntSushi on 2025-05-29 19:01_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-05-29 19:01_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-05-29 19:02_

---

_@dhruvmanila reviewed on 2025-05-30 04:38_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:197 on 2025-05-30 04:38_

For reference, Pyright / Pylance does provide all members of the union type. In the following example, `decode` is only present on `bytes` while other methods are present on both:

<img width="521" alt="Screenshot 2025-05-30 at 10 06 56" src="https://github.com/user-attachments/assets/386be55e-90c5-4e44-aacb-e55bce7e30a3" />


---

_Review comment by @dhruvmanila on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:55 on 2025-05-30 04:46_

I'm sure you must've thought about this but in the future it would be useful to provide more information to the client like docstring, item kind, etc. and I'm not exactly sure how this API could help with that. I'm curious to hear your thoughts if any.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/ide_support.rs`:41 on 2025-05-30 04:49_

Do we need to account for the negative types here?

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:239 on 2025-05-30 05:11_

Can we add a test case which populates the negative set in intersection? Like, `if not isinstance(...)`

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:314 on 2025-05-30 05:22_

I'd argue that this is actually the correct behavior here as during runtime those attributes are available on the module.

But, I think we would need to consider it for stub files because that would provide an accurate picture of what symbols are actually present during runtime. Consider the following sources:

`module.py`:

```py
def evaluate(x=None):
    if x is None:
        return 0
    return x
```

`module.pyi`:

```py
from typing import Optional

__all__ = ["evaluate"]

def evaluate(x: Optional[int] = None) -> int: ...
```

`play.py`:

```py
import module

# This doesn't exists at runtime
module.Optional
# This exists
module.evaluate
```

Currently, it would include `Optional`:

```py
from ty_extensions import all_members

import module

# ty: Revealed type: `tuple[Literal["Optional"], Literal["__all__"], ..., Literal["evaluate"]]` [revealed-type]
reveal_type(all_members(module))
```

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/ide_support.rs`:138 on 2025-05-30 05:40_

Calling this function for the `Type::ModuleLiteral` might not provide an accurate picture because this uses `symbol_from_declarations` and `symbol_from_bindings` which defaults to not enforcing the re-export convention. This means it could include symbols from the module that aren't present during runtime because they're being implicitly re-exported from the stub file. 

For a concrete example, refer to my earlier comment about `__all__` and remove the `__all__` declaration from `module.pyi`. As `Optional` is not being re-exported explicitly (via `from typing import Optional as Optional`), it should not be included in the completion.

One way to solve this would be to special case `Type::ModuleLiteral` and use `imported_symbol` instead of `symbol_from_declarations` or `symbol_from_bindings`, so something like this:

```rs
self.extend_with_type(db, KnownClass::ModuleType.to_instance(db));

let Some(file) = literal.module(db).file() else {
    return;
};

let module_scope = global_scope(db, file);
let use_def_map = use_def_map(db, module_scope);
let symbol_table = symbol_table(db, module_scope);

for (symbol_id, _) in use_def_map.all_public_declarations() {
	let symbol_name = symbol_table.symbol(symbol_id).name();
	if imported_symbol(db, file, symbol_name, None) {}
}
```

---

_@dhruvmanila approved on 2025-05-30 05:40_

Overall, this looks good.

I've left a few comments stating the relevance of `__all__` and the re-export conventions for imported symbols but that all can be done in a follow-up.

---

_@dhruvmanila reviewed on 2025-05-30 05:43_

---

_Review comment by @dhruvmanila on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:55 on 2025-05-30 05:43_

We could just return a `Vec<Type<'db>>` or `FxHashSet<Type<'db>>` here but then there's also another question of whether to actually store information like docstring on the type itself or whether to store it somewhere else.

---

_@MichaReiser reviewed on 2025-05-30 05:43_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:197 on 2025-05-30 05:43_

I suspect that this will require some iteration and/or even configurations. E.g. r-a has an option that allows you to configure if r-a should hide members to which you don't have access to due to visibility constraint. Hiding them is probably the right default when working with third-party code, but for first-party code, I often want to see all members, and I'll then change the visibility of the member.

So I think we should move forward here as we can change this easily in the future

---

_Comment by @dhruvmanila on 2025-05-30 05:51_

> > the new `<str_literal> in <tuple>` inference support, that is probably only useful for this purpose

This behavior exists and is valid at runtime so I'm not sure what do you mean by "only useful for this purpose" (cc @sharkdp)

---

_Comment by @sharkdp on 2025-05-30 08:19_

> > > the new `<str_literal> in <tuple>` inference support, that is probably only useful for this purpose
> 
> This behavior exists and is valid at runtime so I'm not sure what do you mean by "only useful for this purpose" (cc @sharkdp)

There's (hopefully) nothing wrong with the precise True/False type inference for these binary expressions. I was merely questioning if we should keep the extra code just for the purpose of writing these mdtests. For users of ty, there's probably not much value in inferring literal True/False over bool for these cases that are unlikely to occur in real world code.

I guess @AlexWaygood could also use a this feature for some protocol tests (instead of always listing all members)? And maybe the __all__ tests could also use a similar technique 


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6739 on 2025-05-30 12:06_

if every element in the tuple is not a string literal but _is_ a type disjoint from `Literal["foo"]`, I think we could infer `Literal[False]` for the `"foo" in my_tuple` check 

```suggestion
                } else if !literal.is_disjoint_from(*element) {
                    definitely_false = false;
                }
```

---

_@BurntSushi reviewed on 2025-05-30 12:51_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:197 on 2025-05-30 12:51_

Yeah I think this should be easy to change.

I can imagine wanting configuration for respecting `__all__` too.

---

_@BurntSushi reviewed on 2025-05-30 12:58_

---

_Review comment by @BurntSushi on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:55 on 2025-05-30 12:58_

I 100% agree that we will want more information.

My rough plan here is that completions will use the `AllMembers` API directly, and I'd like to augment it to return more than just names. I don't know exactly _what_ it should return yet. A `Type` seems like a good start I think? But that `all_members` as a `ty_extension` could still just return a tuple of strings.

---

_@AlexWaygood reviewed on 2025-05-30 13:01_

---

_@BurntSushi reviewed on 2025-05-30 13:07_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:314 on 2025-05-30 13:07_

Nice! TIL. I turned your comment into a test and an issue: https://github.com/astral-sh/ty/issues/550

---

_@BurntSushi reviewed on 2025-05-30 13:14_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/ide_support.rs`:138 on 2025-05-30 13:14_

Hah! Well, that fixes https://github.com/astral-sh/ty/issues/550! I should take a quick scan of all comments next time. :P 

Thank you!!!

---

_@AlexWaygood reviewed on 2025-05-30 13:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6720 on 2025-05-30 13:14_

should we bail out here if it's a pathologically long tuple type, by checking the length of the tuple first and just inferring `bool` if it's "too long"?

---

_@AlexWaygood reviewed on 2025-05-30 13:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6720 on 2025-05-30 13:16_

Overall the feature here seems reasonable to me! I'm not sure I have an immediate use case for it in `Protocol` tests, but it's not too much extra code to support it, and precise literal inference is in line with our philosophy elsewhere.

It would be nice to also support other literal types if we're doing this, though -- it feels kinda inconsistent to only support literal strings, and not literal ints or literal bytes? Doesn't need to be tackled in this PR, though

---

_@BurntSushi reviewed on 2025-05-30 13:35_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:239 on 2025-05-30 13:35_

Done! I added this test:

``````
```py
from ty_extensions import all_members, static_assert

class A:
    on_all: int = 1
    only_on_a: str = "a"
    only_on_ab: str = "a"
    only_on_ac: str = "a"

class B:
    on_all: int = 2
    only_on_b: str = "b"
    only_on_ab: str = "b"
    only_on_bc: str = "b"

class C:
    on_all: int = 3
    only_on_c: str = "c"
    only_on_ac: str = "c"
    only_on_bc: str = "c"


def f(intersection: object):
    if isinstance(intersection, A):
        if isinstance(intersection, B):
            if not isinstance(intersection, C):
                reveal_type(intersection)  # revealed: A & B & ~C
                static_assert("on_all" in all_members(intersection))
                static_assert("only_on_a" in all_members(intersection))
                static_assert("only_on_b" in all_members(intersection))
                static_assert("only_on_c" not in all_members(intersection))
                static_assert("only_on_ab" in all_members(intersection))
                static_assert("only_on_ac" in all_members(intersection))
                static_assert("only_on_bc" in all_members(intersection))
```
``````

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/ide_support.rs`:41 on 2025-05-30 13:39_

I don't _think_ so. The [test case I added](https://github.com/astral-sh/ruff/pull/18251#discussion_r2115944532) seems to work as-is. And thinking about this, shouldn't the attributes available on the intersection type be precisely the attributes available in the union of all types in the intersection? Even if there is overlap between the attributes from the positive and negative types, if it's in a positive type, it should still be available.

---

_@BurntSushi reviewed on 2025-05-30 13:39_

---

_@BurntSushi reviewed on 2025-05-30 14:01_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/infer.rs`:6739 on 2025-05-30 14:01_

Oh nice! I added a test case for this (as well as other tests for inference of `"foo" in ("bar", "foo", "quux")` in isolation of `all_members`.

---

_@BurntSushi reviewed on 2025-05-30 14:03_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/infer.rs`:6720 on 2025-05-30 14:03_

Do y'all have a sense of what "pathological long" means?

I would maybe start at `2^24`?

---

_@AlexWaygood reviewed on 2025-05-30 14:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6720 on 2025-05-30 14:15_

Hmm... given that we were questioning whether we needed the feature in the first place, I feel like I'd start off a fair bit lower until someone complains, just to be safe? Maybe 4096? That feels like it's still safely larger than anybody would realistically need in real-world code (or that we'd ever need in our tests), and protects us from large tuples in which multiple elements are pathologically large unions (which might make the disjointness check expensive)

---

_@BurntSushi reviewed on 2025-05-30 14:34_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/infer.rs`:6720 on 2025-05-30 14:34_

That's fine with me!

---

_Comment by @BurntSushi on 2025-05-30 15:17_

I'm going to bring this in, but happy to address any other feedback in a follow-up PR!

---

_Comment by @BurntSushi on 2025-05-30 15:17_

And thank you @sharkdp for doing the lion's share of the work here!!!

---

_Merged by @BurntSushi on 2025-05-30 15:24_

---

_Closed by @BurntSushi on 2025-05-30 15:24_

---

_Branch deleted on 2025-05-30 15:24_

---

_@dhruvmanila reviewed on 2025-05-30 15:39_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/ide_support.rs`:41 on 2025-05-30 15:39_

Ah ofcourse, you're right! Thanks for adding the test case

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/binary/in.md`:26 on 2025-05-30 16:12_

I'm not sure what the "type error" being referred to here is -- the only error I see in this section is `static-assert-error`? I would expect this header to say something more like "statically unknown returns bool"

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:7 on 2025-05-30 16:13_

this calls it a list, but it's a tuple

---

_@carljm reviewed on 2025-05-30 16:23_

Looks great! Just a couple minor nits, not worth a separate PR but for consideration if a future PR touches these areas

---

_@BurntSushi reviewed on 2025-05-30 16:28_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/binary/in.md`:26 on 2025-05-30 16:28_

Yeah, I was calling `static-assert-error` a type error, which I guess is not quite right? Can you say more about "statically unknown returns bool"? ty is still returning an error here when `x in y` is both unknown and used in a static context. What about, "Statically unknown results in error in static context" instead?

---

_@carljm reviewed on 2025-05-30 16:34_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/binary/in.md`:26 on 2025-05-30 16:34_

It seems like you're more commenting on the behavior of `static_assert` here -- "static context" isn't really a thing otherwise. `static_assert` is really just a testing tool for us to easily make assertions about things being statically known. It asserts that some expression evaluates to `Literal[True]` and emits `static-assert-error` otherwise. But (outside of a different mdtest specifically for that purpose) I don't think mdtests should be commenting on the behavior of `static_assert`; the focus should be on the type system behavior under test (which in this case is `<literal-str> in <tuple>`). And the relevant behavior being tested here is that we infer the `in` expression as `bool` (not as `Literal[True]` or `Literal[False]`) if we can't statically determine the result of the `in` test.

On second look I think what this really underscores is that `static_assert` is just the wrong tool for this particular test; it would be clearer here to simply use `reveal_type` and assert that the revealed type of the expression is `bool`.

---

_@BurntSushi reviewed on 2025-05-30 17:27_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/binary/in.md`:26 on 2025-05-30 17:27_

Got it! Thank you for talking me through this. I really appreciate it!

I put up a new PR with these fixes: https://github.com/astral-sh/ruff/pull/18388

---
