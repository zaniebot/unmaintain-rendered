```yaml
number: 11233
title: "[`flake8-pyi`] Implement `PYI059` (`generic-not-last-base-class`)"
type: pull_request
state: merged
author: tusharsadhwani
labels: []
assignees: []
merged: true
base: main
head: pyi-059
created_at: 2024-05-01T18:24:10Z
updated_at: 2024-05-07T18:30:07Z
url: https://github.com/astral-sh/ruff/pull/11233
synced_at: 2026-01-12T15:55:37Z
```

# [`flake8-pyi`] Implement `PYI059` (`generic-not-last-base-class`)

---

_@tusharsadhwani_

## Summary

Implements `Y059` from `flake8-pyi`: `typing.Generic[]` should be the last base in a class' bases list.

If Multiple `Generic[]` classes are found, we don't generate a fix for it.

## Test Plan

`cargo test` and `cargo insta review`.


---

_Review requested from @AlexWaygood by @tusharsadhwani on 2024-05-01 18:24_

---

_Comment by @tusharsadhwani on 2024-05-01 18:26_

Is it okay to add the autofix in the same PR?

---

_Comment by @zanieb on 2024-05-01 18:28_

Just wondering, what happens if a class has two `Generic[]` bases?

---

_Comment by @zanieb on 2024-05-01 18:28_

> Is it okay to add the autofix in the same PR?

Yep!


---

_Comment by @tusharsadhwani on 2024-05-01 18:29_

> what happens if a class has two Generic[] bases?

It's a runtime error:

```pycon
>>> import typing
>>> T = typing.TypeVar('T')
>>> U = typing.TypeVar('U')
>>> class C(typing.Generic[T], typing.Generic[U]):
...   ...
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/opt/homebrew/Cellar/python@3.12/3.12.2_1/Frameworks/Python.framework/Versions/3.12/lib/python3.12/typing.py", line 1102, in _generic_init_subclass
    raise TypeError(
TypeError: Cannot inherit from Generic[...] multiple times.
```

---

_Comment by @zanieb on 2024-05-01 18:34_

Good to know. We should make sure the rule doesn't behave poorly on that case regardless, i.e. it could cause an infinite fix loop.

---

_Comment by @github-actions[bot] on 2024-05-01 18:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/085fec977e000c511ca2a60a761c94b0989cc125/rotkehlchen/db/filtering.py#L577'>rotkehlchen/db/filtering.py:577:25:</a> PYI059 [*] `Generic[]` should always be the last base class
+ <a href='https://github.com/rotki/rotki/blob/085fec977e000c511ca2a60a761c94b0989cc125/rotkehlchen/types.py#L977'>rotkehlchen/types.py:977:34:</a> PYI059 [*] `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI059 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @tusharsadhwani on 2024-05-01 18:39_

> We should make sure the rule doesn't behave poorly on that case regardless

Should multiple `Generic[]` case not raise the issue at all then?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:12 on 2024-05-03 10:37_

```suggestion
/// Checks for classes inheriting from `typing.Generic[]` where `Generic[]` is
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:19 on 2024-05-03 10:38_

```suggestion
/// The rule is also applied to stub files, but, unlike at runtime,
/// in stubs it is purely enforced for stylistic consistency.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:34 on 2024-05-03 10:39_

```suggestion
/// Use instead:
/// ```python
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:63 on 2024-05-03 10:40_

The `fix_title()` messages show up as "quick fix" options in VSCode when you right-click on a violation and the violation has an autofix, so it's good to keep these as short as possible

```suggestion
        Some("Move `Generic[]` to the end".to_string())
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:72 on 2024-05-03 10:42_

nit: for new rules, we generally prefer to keep these signatures as short and consistent as possible. Here we'd probably go for a signature like this:

```suggestion
pub(crate) fn generic_not_last_base_class(
    checker: &mut Checker,
    class_def: &StmtClassDef,
) {
```

since you can easily extract the `bases` from the `StmtClassDef` in the body of the rule

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:111 on 2024-05-03 10:57_

I might do something like this, to make it clear that both branches here end up pushing a new diagnostic:

```suggestion
    let diagnostic = {
	    if generic_base_indices.len() == 1 {
	        let base_index = generic_base_indices[0];
	        if base_index == bases.args.len() - 1 {
	            // Don't raise issue for the last base.
	            return;
	        }
	
	        Diagnostic::new(GenericNotLastBaseClass, class_def.identifier())
	            .with_fix(generate_fix(bases, base_index, checker.locator()))
	    } else {
	        // No fix if multiple generics are seen in the class bases.
	        Diagnostic::new(
	            GenericNotLastBaseClass,
	            class_def.identifier(),
	        )
	    }
	};
	
	checker.diagnostics.push(diagnostic);
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:118 on 2024-05-03 10:59_

I think this should be the very first thing we check in the `generic_not_last_base_class` function. It's a very cheap check, and it allows us to short-circuit the whole rule if the user hasn't imported `typing`

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:122 on 2024-05-03 11:03_

We have a helper function for this kind of thing, and I was about to suggest that you use it:

https://github.com/astral-sh/ruff/blob/349a4cf8cea9cfa5bfdd78e94c40220886008b7a/crates/ruff_python_ast/src/helpers.rs#L662-L673

_But_ if you did use that helper function, it would do a slightly different thing here to the PR's current behaviour: it would _also_ flag places where bare `typing.Generic` (unsubscripted with any type parameters) was used as a base class. That always causes an exception at runtime... but maybe it should be flagged by this rule anyway? I can't think of a good reason to _exclude_ bare `typing.Generic` from this check, even if it's going to cause other problems as well. Anyway, I don't feel too strongly on this point, but whichever way you decide to go -- it would be great to add a test that shows the current behaviour if bare `typing.Generic` appears in the bases tuple.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:127 on 2024-05-03 11:05_

This can be written more simply as the following (which does exactly the same thing as your current logic under the hood):

```suggestion
    semantic.match_typing_expr(value, "Generic")
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:130 on 2024-05-03 11:05_

What's with all the commented-out code here?

---

_@AlexWaygood reviewed on 2024-05-03 11:05_

Nice, thank you! Overall this looks good. Left some inline comments below :-)

---

_@tusharsadhwani reviewed on 2024-05-03 11:08_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:111 on 2024-05-03 11:08_

makes sense! and `.with_fix()` is very clean.

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:130 on 2024-05-03 11:09_

my bad, it's leftover testing code.

---

_@tusharsadhwani reviewed on 2024-05-03 11:09_

---

_Comment by @AlexWaygood on 2024-05-03 11:12_

> Should multiple `Generic[]` case not raise the issue at all then?

I'd say _probably_ not. The [behaviour we implemented at flake8-pyi](https://github.com/PyCQA/flake8-pyi/blob/f60d7e3cdc833dd358b493d2ca80fd187cc38cfb/pyi.py#L1762-L1764) is to ignore this rule if `Generic[]` appeared multiple times in the bases tuple, and I think it makes sense to do the same thing here. If you have something like `class Foo(int, Generic[T], Generic[S]): pass`, I think it's honestly just confusing for a user if they get a message about `Generic[]` "not being the last base class". There's a much more significant issue in their code (you can't have more than one `Generic[]` in the bases tuple), and if they solve the more significant issue, then `Generic[]` not being the last base class would be naturally solved as a side effect of that.

(We shouldn't flag multiple `Generic[]`s in this check; that could possibly be a separate check.)

---

_@AlexWaygood reviewed on 2024-05-03 11:13_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:130 on 2024-05-03 11:13_

no worries!

---

_@tusharsadhwani reviewed on 2024-05-03 11:28_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:122 on 2024-05-03 11:28_

I was thinking maybe one can subclass `Generic` by itself to do some metaprogramming type stuff, but since that's straight up `TypeError`, it'll probably be okay to go with your suggestion. Thanks for pointint it out!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:122 on 2024-05-03 11:33_

inheriting from plain `Generic` is specifically allowed if the name of the subclass is "Protocol" FYI:

```pycon
>>> from typing import Generic
>>> class Protocol(Generic): pass
... 
>>> 
```

but I didn't tell you that

---

_@AlexWaygood reviewed on 2024-05-03 11:33_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:74 on 2024-05-03 11:39_

```suggestion
    let Some(bases) = class_def.arguments.as_deref() else {
        return;
    };
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI059_PYI059.py.snap`:9 on 2024-05-03 11:41_

I think it would be better if the carets underlined the bases tuple here, rather than the name of the class, since that's where the error is. That means we need to pass the range of the bases tuple in as the second argument of `Diagnostic::new()` rather than `class_def.identifier()`

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:120 on 2024-05-03 11:42_

possibly this could function could just be inlined at this point?

---

_@AlexWaygood reviewed on 2024-05-03 11:43_

---

_@AlexWaygood reviewed on 2024-05-03 11:48_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:139 on 2024-05-03 11:48_

what happens for pathological cases like

```py
class Foo(  # comment about the bracket
    # Part 1 of multiline comment 1
    # Part 2 of multiline comment 1
    Generic[T]  # comment about Generic[T]
    # another comment?
    ,  # comment about the comma?
    # part 1 of multiline comment 2
    # part 2 of multiline comment 2
    int,  # comment about int
    # yet another comment?
):  # and another one for good measure
    pass
```

---

_@tusharsadhwani reviewed on 2024-05-03 11:52_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:139 on 2024-05-03 11:52_

It will technically work, but comments before int and after `Generic[T]` get eaten up. That is _fixable_, but the fix is subjective.

What's ruff's general stance in such cases? I know `libcst` has capabilities to try and identify which comment groups belong to which expression. But the easier thing would be to avoid moving comments at all, and just preserve comments.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:139 on 2024-05-03 11:59_

Different rules take different stances; we don't have a unified policy yet. Several of our autofixes will currently happily delete your comments (e.g. https://github.com/astral-sh/ruff/issues/9779); I think we all agree that this is bad, but we haven't yet decided _how_ bad it is (see #9790). Other rules approach comments with a perhaps-excessive level of caution, either using the raw token stream for their autofix (RUF022, RUF023), or using libcst. Both are much slower than simple text replacements, I think libcst especially.

> the fix is subjective.

absolutely! But I think that's always the case for something like this. I don't mind that much if some comments end up in the "wrong place", but I _do_ think we should try to avoid _deleting_ any comments if it's not too hard or costly to do.

---

_@AlexWaygood reviewed on 2024-05-03 11:59_

---

_@tusharsadhwani reviewed on 2024-05-03 13:18_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:139 on 2024-05-03 13:18_

Trying to figure out how to get the start and end ranges of the comma after the Generic base. Tried using libcst to obtain the Comma itself, but does it have position information?

---

_@zanieb reviewed on 2024-05-03 13:24_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:139 on 2024-05-03 13:24_

I would probably just read one character at a time after the end of the generic base until you see a comma — does that make sense here? I'd avoid LibCST unless it's absolutely necessary (it's slow).

---

_@AlexWaygood reviewed on 2024-05-03 13:38_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:139 on 2024-05-03 13:38_

> I would probably just read one character at a time after the end of the generic base until you see a comma — does that make sense here? I'd avoid LibCST unless it's absolutely necessary (it's slow).

Agree -- `memchr` is probably the best tool for that (you can grep in the `crates/ruff_linter` directory for existing uses). Other possibilities are using libCST (but as Zanie says, it's really slow), or using the raw tokens (also can be slow, and probably more complicated here than using `memchr`).

If you look at https://play.ruff.rs/b2b834e8-0247-47fe-995a-c0ade0e6b240, with the following snippet:

```py
class Foo(int, str): pass
```

On the AST tab on the right, we can see:
- The `ExprName` node for the `int` base has range `10..13`
- The `ExprName` node for the `str` base has range `15..18`

On the tokens tab on the right, we can see that:
- The comma in between the two nodes has range `13..14`

---

_@tusharsadhwani reviewed on 2024-05-03 14:29_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:139 on 2024-05-03 14:29_

First try at this, I've used `.find()` (which I suppose can be swapped with `memchr`)  and `.position()` (which I'm not sure if it can be replaced)

---

_@AlexWaygood reviewed on 2024-05-03 15:51_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:111 on 2024-05-03 15:51_

There's quite a lot of `unwrap()` and `expect()` calls in the `generate_fix()` function currently. I think we could retrieve the node for the `Generic` base and the node for the last base in a more type-safe way, and get rid of some of those calls. That would mean that we'd be using the type system to our advantage rather than "fighting against it". You could maybe do something like this:

```rs
    let Some(last_base) = bases.args.last() else {
        return;
    };

    // Now we know both that the bases tuple is non-empty
    // and we've retrieved the node for the last base

    let mut generic_base_iter = bases
        .args
        .iter()
        .find(|node| semantic.match_typing_expr(map_subscript(node), "Generic"));

    // grab the first `Generic[]` base in the bases tuple, if there is one
    let Some(generic_base) = generic_base_iter.next() else {
        return;
    }

    // AKA they are the same node
    if generic_base.range() == last_base.range() {
        return;
    }

    let multiple_generic_bases = generic_base_iter.next().is_some();
```

You'd then have all the information you need to determine whether it's fixable or not (whether or not there are multiple `Generic[]` bases -- though I'm still not _sure_ we should emit the lint at _all_ if there are); and you'd have extracted the two nodes you need for the autofix (the `Generic[]` base and the last base).

---

_@tusharsadhwani reviewed on 2024-05-03 16:30_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:111 on 2024-05-03 16:30_

The `.find()` should be `.filter()`, but impressively the rest of the code was pretty much spot on. Updated.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:108 on 2024-05-03 16:36_

or maybe:

```suggestion
    let mut diagnostic = Diagnostic::new(GenericNotLastBaseClass, bases.range());

    // No fix if multiple generics are seen in the class bases.
    if generic_base_iter.next().is_none() {
        diagnostic.set_fix(generate_fix(
            generic_base, last_base, checker.locator(),
        ));
    }

---

_@AlexWaygood reviewed on 2024-05-03 16:36_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:117 on 2024-05-06 20:05_

I don't really understand what's going on here, I'm afraid :(

Is this the best name for this variable? Would you mind adding some comments to this function to explain how it works (preferably with some examples)?

---

_@AlexWaygood reviewed on 2024-05-06 20:05_

---

_@tusharsadhwani reviewed on 2024-05-06 20:09_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:117 on 2024-05-06 20:09_

I'm trying to find the first index in the source code after `Generic[...],`, which doesn't contain a whitespace.

Basically, deleting just `Generic[T]` and the comma after it would leave a trailing whitespace around (usually a space).

Without the whitespace detection, this code:

```python
class Foo(Generic[T], Bar):
```

would be autofixed into:

```python
class Foo( Bar, Generic[T]):
```

i.e. The space before `Bar` was not deleted.

---

_@tusharsadhwani reviewed on 2024-05-06 20:11_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:117 on 2024-05-06 20:11_

I'm honestly not sure if this is the best way to write this, please feel free to refactor or comment on this.

---

_Comment by @AlexWaygood on 2024-05-06 20:13_

(Everything looks great to me now except the autofix code, which I'm still not _quite_ sure about -- both in terms of code readability and in terms of the number of `.expect()` and `.unwrap()` calls in it :-)

---

_Comment by @AlexWaygood on 2024-05-06 20:47_

@tusharsadhwani, I've just pushed https://github.com/astral-sh/ruff/pull/11233/commits/6fce0fd868344efb7e3d4e1336e252fd8f622f35, which gets rid of the remaining `.expect()` and `.unwrap()` calls from `generate_fix()`. Instead, the errors are bubbled up by returning an `anyhow::Result<Fix>` from `generate_fix()`. In combination with the `Diagnostic::try_set_fix()` method, that means that, rather than panicking, we'll gracefully log the error and move on if we fail to construct a `Fix`. That's usually preferable for this kind of thing, since it's not 100% essential that a `Fix` is constructed, but panicking would be pretty bad.

---

_@tusharsadhwani reviewed on 2024-05-06 20:50_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:117 on 2024-05-06 20:50_

I think deleting a positional argument from a function call during autofix is common enough that once we finalize on the way it should be done this can be lifted to make a helper instead.

---

_@AlexWaygood reviewed on 2024-05-06 20:52_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:117 on 2024-05-06 20:52_

Ah... in fact... https://github.com/astral-sh/ruff/blob/12b5c3a54c0a34bae9dfcfeaf27961f4432d5623/crates/ruff_linter/src/fix/edits.rs#L155-L237

---

_@AlexWaygood reviewed on 2024-05-06 20:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:117 on 2024-05-06 20:53_

I'm so sorry, I completely forgot that we had those helpers!

---

_@tusharsadhwani reviewed on 2024-05-06 20:54_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:117 on 2024-05-06 20:54_

I also didn't find them, so partly on me :P it uses the Tokenizer which is pretty clever

---

_@tusharsadhwani reviewed on 2024-05-06 20:57_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:117 on 2024-05-06 20:57_

I'll go ahead and refactor it.

---

_Comment by @tusharsadhwani on 2024-05-06 21:15_

Okay, this is much cleaner.

---

_@AlexWaygood approved on 2024-05-07 10:00_

Thanks! :D

---

_Renamed from "[flake8-pyi] Implement PYI059" to "[`flake8-pyi`] Implement `PYI059` (`generic-not-last-base-class`)" by @AlexWaygood on 2024-05-07 10:02_

---

_Merged by @AlexWaygood on 2024-05-07 10:07_

---

_Closed by @AlexWaygood on 2024-05-07 10:07_

---

_Branch deleted on 2024-05-07 18:30_

---
