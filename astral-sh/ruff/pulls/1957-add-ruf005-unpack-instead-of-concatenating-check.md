```yaml
number: 1957
title: "Add RUF005 \"unpack instead of concatenating\" check"
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: list-plus-to-splat
created_at: 2023-01-18T14:24:50Z
updated_at: 2023-01-19T22:47:17Z
url: https://github.com/astral-sh/ruff/pull/1957
synced_at: 2026-01-12T15:55:07Z
```

# Add RUF005 "unpack instead of concatenating" check

---

_@akx_

This PR adds a new check that turns expressions such as `[1, 2, 3] + foo` into `[1, 2, 3, *foo]`, since the latter is easier to read and faster:

```
~ $ python3.11 -m timeit -s 'b = [6, 5, 4]' '[1, 2, 3] + b'
5000000 loops, best of 5: 81.4 nsec per loop
~ $ python3.11 -m timeit -s 'b = [6, 5, 4]' '[1, 2, 3, *b]'
5000000 loops, best of 5: 66.2 nsec per loop
```

However there's a couple of gotchas:

* This felt like a `simplify` rule, so I borrowed an unused `SIM` code even if the upstream `flake8-simplify` doesn't do this transform. If it should be assigned some other code, let me know 😄 
* **More importantly** this transform could be unsafe if the other operand of the `+` operation has overridden `__add__` to do something else. What's the `ruff` policy around potentially unsafe operations? (I think some of the suggestions other ported rules give could be semantically different from the original code, but I'm not sure.)
* I'm not a very established Rustacean, so there's no doubt my code isn't quite idiomatic. (For instance, is there a neater way to write that four-way `match` statement?)

Thanks for `ruff`, by the way! :)

---

_Comment by @charliermarsh on 2023-01-18 16:05_

Thank you for this contribution! It looks great. I'll look to add comments soon.

The main thing I'm trying to decide is whether this should be in `SIM`, or `RUF`. I'm not sure yet. Historically, we've avoided adding checks to the "ported" plugins even if they make sense to include conceptually, instead opting to put novel rules in `RUF`. The thinking is that the use of `SIM` is mostly about compatibility -- it lets users that are already using flake8-simplify adopt Ruff and enable `SIM` without any additional work. (The other argument would be that enabling `SIM` is a sign that you want simplification checks, which I may not be explaining well, but is subtly different.) So in that light, it'd be surprising if you enabled `SIM` and saw new checks that didn't exist "upstream".

In the future (not certain when), we'll likely recategorize the rules under more meaningful categories (like `complexity` -- see the initial proposal in #1774), while retaining this compatibility layer (so the `flake8-simplify` checks would continue to exist under `flake8-simplify` in some form, but also as part of a broader `complexity` category). At that point, the above issue wouldn't matter much, since anyone that enables the `complexity` category would get this rule regardless.

My gut is to keep this in `RUF`, or perhaps even introduce a new category along the lines of "experimental" or "nursery" (#1774) for now.

What do you think? Would love to hear your reactions.


---

_Comment by @akx on 2023-01-18 17:25_

Hi @charliermarsh, thanks for the kind words!

I'm absolutely more than fine moving this to `RUF`, or even some sort of `RUX` (for eXperimental or, um, possiblyXunsafe) category; I felt a bit bad shoving this under `flake8-simplify`'s namespace to begin with anyway.

Beyond the categorization effort in #1774, maybe it might be worth it to be able to mark checks (like this) separately as `unsafe` or `aggressive` because you'll have to think a bit before blindly applying the suggestion  – e.g. you'd have to opt-in to `--fix-aggressive`?

As an aside, I fixed the `cargo fmt` issue that appeared in the checks (and added it as a pre-commit check in #1968...)

---

_Comment by @andersk on 2023-01-18 18:18_

> Beyond the categorization effort in https://github.com/charliermarsh/ruff/issues/1774, maybe it might be worth it to be able to mark checks (like this) separately as `unsafe` or `aggressive` because you'll have to think a bit before blindly applying the suggestion – e.g. you'd have to opt-in to `--fix-aggressive`?

Some prior art, if only for the amusement value: [`__CARGO_FIX_YOLO=1`](https://users.rust-lang.org/t/cargo-clippy-fix-doesnt-fix-anything/68453)

---

_Comment by @charliermarsh on 2023-01-18 18:21_

> Beyond the categorization effort in https://github.com/charliermarsh/ruff/issues/1774, maybe it might be worth it to be able to mark checks (like this) separately as unsafe or aggressive because you'll have to think a bit before blindly applying the suggestion – e.g. you'd have to opt-in to --fix-aggressive?

One way we could model this is that we could omit this from the `fixable` list by default. (We already support turning autofix on and off on a per-rule basis: https://github.com/charliermarsh/ruff#unfixable.) Right now, `fixable`, by default, includes every rule. But we could instead start to omit some rules from `fixable`, and thus require users to opt-in explicitly. I'm not sure how we'd message this to users though.


---

_Comment by @bluetech on 2023-01-18 19:22_

From @andersk's link I saw that rustc/clippy assigns "applicability" to suggestions: https://doc.rust-lang.org/stable/nightly-rustc/rustc_errors/enum.Applicability.html

---

_Comment by @charliermarsh on 2023-01-18 19:36_

@akx - Let's go ahead and move this under `RUF` for now.

@bluetech - Applicability looks nice and is something we should support (defined at the Rule level, configurable at the command-line and in `pyproject.toml`).

\cc @not-my-profile who's probably interested in that too.


---

_Comment by @akx on 2023-01-19 09:34_

@charliermarsh Moved to RUF005. I also added a commented-out "failing test" in the fixture – chained concatenations aren't handled gracefully at all at present, but I suppose that could be improved later :)

---

_Comment by @not-my-profile on 2023-01-19 15:08_

I agree about rule applicability being something we should have ... I had already noticed it when looking at the clippy lint documentation ... I just opened #1997 for tracking that :)

---

_Comment by @charliermarsh on 2023-01-19 15:37_

Great, I'll look to get this merged today!

---

_Renamed from "Add new potentially unsafe "unpack instead of concatenating" check" to "Add RUF005 "unpack instead of concatenating" check" by @akx on 2023-01-19 15:52_

---

_Review comment by @charliermarsh on `src/violations.rs`:3232 on 2023-01-19 16:46_

I believe we need to merge in `main`, then remove this method, and add `#[derive_message_formats]` just above `fn message`.

---

_Review comment by @charliermarsh on `src/checkers/tokens.rs`:60 on 2023-01-19 16:46_

Nit: can we revert this change?

---

_Review comment by @charliermarsh on `src/rules/ruff/rules/unpack_instead_of_concatenating_to_collection_literal.rs`:58 on 2023-01-19 16:56_

It'd be nice if we could thread through the `level` argument here to `generator.unparse_expr`. That would enable use to maintain the parens around the tuple, by incrementing the `level` a bit.

Alternatively, maybe we can just manually add the parens here?

(Specifically, I think `Consider `7, 8, 9, *bar` instead of concatenation` would be clearer as `Consider `(7, 8, 9, *bar)` instead of concatenation`.)

---

_Review comment by @charliermarsh on `resources/test/fixtures/ruff/RUF005.py`:7 on 2023-01-19 16:57_

I think we should resolve this prior to merging.

Maybe we avoid applying this if one of the expressions is itself a BinOp? So we'd flag `Consider `['a', 'b', 'c', *eggs]` instead of concatenation` but not `Consider `*['a', 'b', 'c'] + eggs, 'yes', 'no', 'pants'` instead of concatenation`.

---

_@charliermarsh reviewed on 2023-01-19 16:57_

---

_Review comment by @akx on `src/violations.rs`:3232 on 2023-01-19 19:00_

Rebased!

---

_@akx reviewed on 2023-01-19 19:00_

---

_Review comment by @akx on `src/rules/ruff/rules/unpack_instead_of_concatenating_to_collection_literal.rs`:58 on 2023-01-19 19:01_

I opted for just wrapping the `new_expr` in parens. Wasn't quite sure how to use `level` since it's, well, undocumented...

---

_@akx reviewed on 2023-01-19 19:01_

---

_@akx reviewed on 2023-01-19 19:01_

---

_Review comment by @akx on `resources/test/fixtures/ruff/RUF005.py`:7 on 2023-01-19 19:01_

I added a guard for the splattee, which seems to clean things up.

---

_Comment by @akx on 2023-01-19 19:02_

@charliermarsh Thanks for the review! 

I think this looks pretty good now:

```diff
 class Fun:
     words = ("how", "fun!")

     def yay(self):
         return self.words

 yay = Fun().yay
 
 foo = [4, 5, 6]
-bar = [1, 2, 3] + foo
+bar = [1, 2, 3, *foo]
 zoob = tuple(bar)
-quux = (7, 8, 9) + zoob
-spam = quux + (10, 11, 12)
+quux = (7, 8, 9, *zoob)
+spam = (*quux, 10, 11, 12)
 spom = list(spam)
-eggs = spom + [13, 14, 15]
-elatement = ("we all say", ) + yay()
-excitement = ("we all think", ) + Fun().yay()
-astonishment = ("we all feel", ) + Fun.words
+eggs = [*spom, 13, 14, 15]
+elatement = ("we all say", *yay())
+excitement = ("we all think", *Fun().yay())
+astonishment = ("we all feel", *Fun.words)
 
-chain = ['a', 'b', 'c'] + eggs + list(('yes', 'no', 'pants') + zoob)
+chain = ["a", "b", "c", *eggs, *list(("yes", "no", "pants", *zoob))]
```

---

_Comment by @rhkleijn on 2023-01-19 19:49_

An interesting edge case to test would be empty tuples. Does it handle the following correctly?

```python
baz = () + zoob
```

Unpacking as `(*zoob)` would be invalid here, I guess. It would need a trailing comma `(*zoob,)`.

---

_Comment by @akx on 2023-01-19 20:20_

@rhkleijn I'm a bit surprised myself, but it does the right thing!

```diff
-baz = () + zoob
+baz = (*zoob,)
```

---

_Comment by @charliermarsh on 2023-01-19 22:35_

@akx - How would you feel if I subsequently turned off autofix (by default) on this rule for now, instead requiring it to be opt-in (via the `fixable` setting in `pyproject.toml`)? I'm not completely decided either way, but I wanted to run it by you before I made that change.

---

_Merged by @charliermarsh on 2023-01-19 22:38_

---

_Closed by @charliermarsh on 2023-01-19 22:38_

---

_Comment by @charliermarsh on 2023-01-19 22:38_

(Merging regardless but may follow-up to tweak the default fix list -- LMK what you think!)

---

_Comment by @akx on 2023-01-19 22:47_

@charliermarsh Sounds good to me! Thanks for the merge :) 

---
