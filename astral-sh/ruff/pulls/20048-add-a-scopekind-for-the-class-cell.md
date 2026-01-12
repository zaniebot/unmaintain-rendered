```yaml
number: 20048
title: "Add a `ScopeKind` for the `__class__` cell"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: alex-brent/class-cell
created_at: 2025-08-22T17:42:58Z
updated_at: 2025-08-26T13:49:11Z
url: https://github.com/astral-sh/ruff/pull/20048
synced_at: 2026-01-12T15:56:53Z
```

# Add a `ScopeKind` for the `__class__` cell

---

_@ntBre_

Summary
--

This PR aims to resolve (or help to resolve) #18442 and #19357 by encoding the CPython semantics around the `__class__` cell in our semantic model. Namely,

> `__class__` is an implicit closure reference created by the compiler if any methods in a class body refer to either `__class__` or super.

from the Python [docs](https://docs.python.org/3/reference/datamodel.html#creating-the-class-object).

As noted in the variant docs by @AlexWaygood, we don't fully model this behavior, opting always to create the `__class__` cell binding in a new `ScopeKind::DunderClassCell` around each method definition, without checking if any method in the class body actually refers to `__class__` or `super`.

As such, this PR fixes #18442 but not #19357.

Test Plan
--

Existing tests, plus the tests from #19783, which now pass without any rule-specific code.

Note that we opted not to alter the behavior of F841 here because flagging `__class__` in these cases still seems helpful. See the discussion in https://github.com/astral-sh/ruff/pull/20048#discussion_r2296252395 and in the test comments for more information.


---

_Label `rule` added by @ntBre on 2025-08-22 17:43_

---

_Label `rule` removed by @ntBre on 2025-08-22 17:43_

---

_Label `bug` added by @ntBre on 2025-08-22 17:43_

---

_@AlexWaygood reviewed on 2025-08-22 17:44_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/binding.rs`:459 on 2025-08-22 17:44_

oops, I think this is unused and can go (leftover from an earlier version of the PR!)

```suggestion
```

---

_Comment by @github-actions[bot] on 2025-08-22 17:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-08-22 20:50_

I pulled in the tests from #19783 (and added @mikeleppane as a co-author, thank you!) and the issue with those rules is now resolved, with one additional change to the semantic model to reuse the cell binding for `__class__` in `add_binding` (c1604dd1c4).

I also tried the tests from #19424, but I think these still need some rule-specific code, so I decided to leave that as a separate PR. The changes here should still help, but they aren't sufficient on their own.

Finally, I applied our `class_variables_visible` change in a couple more places. Those were some of the last references to `"__class__"` that I found from grepping.

---

_Marked ready for review by @ntBre on 2025-08-22 21:09_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/scope.rs`:172 on 2025-08-23 11:28_

a couple of nits:
- I think maybe `DunderClassCell` might be a better name for the variant? `ClassCell` might imply that it's something to do with classes, which it _is_, but we're really using "class" here to refer to the dunder-class variable rather than the class itself
- I wonder if we should move the long comment we added in `crates/ruff_linter/src/checkers/ast/mod.rs` to be a doc-comment above this enum variant, and add a new comment where the long comment currently is that just says to see the doc-comment for this enum variant to find out what this `DunderClassCell` thing is. That way I'll see the long comment if I'm hovering over the variant in VSCode. I think I'd also expect to see some docs here if I clicked go-to definition in my IDE.

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:438 on 2025-08-23 11:31_

I guess we could also write this as

```suggestion
            class_variables_visible =
                matches!((scope.kind, index), (ScopeKind::Type, 0) | (ScopeKind::ClassCell, 1));
```

I'm happy with either! (If you do update this, it would be good to update the other occurence in this file too)

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/binding.rs`:692 on 2025-08-23 11:32_

I guess I'd prefer `DunderClassCell` as the name for this variant too

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:2495 on 2025-08-23 18:20_

I know that if you remove this branch of code, we get a lot more F841 errors in the snapshot you added saying that various `__class__` definitions are assigned to but never used. But I actually feel like most of those diagnostics are desirable? I know that there's an example in https://github.com/astral-sh/ruff/issues/18442#issue-3114385820 of where assigning to `__class__` in one method will change the value of `__class__` when it's accessed in a very specific way from another method. But that honestly seems like extremely far-fetched code that neither a human nor an LLM would ever write. To me a diagnostic telling me that the assignment to `__class__` in `F.method()` here is unused seems useful -- for all common ways of accessing `__class__`, mutations made in one method will not affect the value as seen by other methods:

```pycon
>>> class F:
...     def method(self):
...         __class__ = 42
...     def method2(self):
...         print(__class__)
...         
>>> f = F()
>>> f.method()
>>> f.method2()
<class '__main__.F'>
```

IMO, practicality beats purity here -- if there's a bug in F841 regarding `__class__`, it's only that it refers to it as a "local variable". We could consider making this edit to the error message if the variable name is `__class__` specifically:

```diff
- f841_ple0117.py:14:9: F841 Local variable `__class__` is assigned to but never used
+ f841_ple0117.py:14:9: F841 Variable `__class__` is assigned to but never used
```

But even _that_ feels just as likely to cause confusion as it is to clarify things -- honestly, I'd be inclined to just leave F841 as it is (but add more tests for how it behaves with `__class__`, and ideally add some comments to those tests!)

---

_@AlexWaygood approved on 2025-08-23 18:20_

This is great!! Would you be up for trying to apply the same change to ty as well?

---

_@ntBre reviewed on 2025-08-25 13:59_

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/model.rs`:438 on 2025-08-25 13:59_

Yeah that seems a little nicer, thanks!

---

_@ntBre reviewed on 2025-08-25 14:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:2495 on 2025-08-25 14:07_

That makes sense to me! I didn't really look closely at the `__getattribute__` stuff in the issue until you mentioned it, that does seem pretty uncommon! I can comment there after we merge this and close the issue.

Are there any other tests you would add, or are the ones from #19783 enough once I add some comments?

---

_@AlexWaygood reviewed on 2025-08-25 14:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:2495 on 2025-08-25 14:16_

Those tests LGTM! We can always add some more if more issues come up in the future. The big user-facing improvement here to me is that we no longer think this is a syntax error, which it obviously would be for any non-`__class__` variable:

```py
class F:
    def method(self):
        nonlocal __class__
```

To me that seems like a somewhat serious bug on `main`, because obviously syntax errors are unsuppressable!

And I think the fact that modelling it as a separate scope (emulating the semantics at runtime precisely) means that we can remove special-casing for `__class__` elsewhere is another great reason to make this change.

---

_@ntBre reviewed on 2025-08-25 14:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:2495 on 2025-08-25 14:48_

I wrote a comment on this case to explain why we don't flag it:

```py
class A:
    __class__ = 1
```

I said that it was a normal class variable, not the special `__class__` cell, but maybe that's not the case:

```pycon
>>> class A:
...     __class__ = 1
...
>>> A.__class__
<class 'type'>
```

Should we be flagging that as well? That might be moving beyond the scope of this PR, though.

---

_@AlexWaygood reviewed on 2025-08-25 14:52_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:2495 on 2025-08-25 14:52_

I regret to inform you that there's sometimes a ~~cursed~~ "good" reason to assign to `__class__` in a class body:

https://github.com/python/typing_extensions/blob/d372913fd71f8f01008c20276fbfe01519ddf1df/src/typing_extensions.py#L1910-L1911

So... no, we shouldn't be flagging that 😆

---

_@AlexWaygood reviewed on 2025-08-25 14:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:2495 on 2025-08-25 14:53_

Specifically:

```pycon
>>> class Foo:
...     __class__ = bool
...     
>>> Foo().__class__
<class 'bool'>
>>> isinstance(Foo(), bool)
True
```

---

_@AlexWaygood reviewed on 2025-08-25 14:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:2495 on 2025-08-25 14:54_

I think it's some crazy descriptor magic that means that if you access it on the class object itself, it doesn't pay attention to the value you set in the class body, but if you access it on the instance it does

---

_@ntBre reviewed on 2025-08-25 14:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:2495 on 2025-08-25 14:55_

That's good news to me! I think this PR is good to go then :laughing: Well, maybe "normal class variable" isn't exactly right still...

---

_@ntBre reviewed on 2025-08-25 14:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:2495 on 2025-08-25 14:57_

I guess there's an implicit "for the purposes of F841" in an F841 test file, so it seems close enough.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F841_0.py`:157 on 2025-08-25 16:05_

```suggestion
# OK -- `__class__` in this case is not the special `__class__` cell,
# so we don't emit a diagnostic. (It has its own special semantics --
# see https://github.com/astral-sh/ruff/pull/20048#discussion_r2298338048 --
# but those aren't relevant here.)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F841_0.py`:165 on 2025-08-25 16:06_

```suggestion
# error, and we still emit a diagnostic. See https://github.com/astral-sh/ruff/pull/20048#discussion_r2296252395
```

---

_@AlexWaygood approved on 2025-08-25 16:06_

Looks great!

---

_Comment by @dscorbett on 2025-08-25 16:37_

This comment is not true:
```
# The following three cases should technically be allowed because `__class__`
# is a nonlocal variable in the method scope. However, `__class__` isn't easily
# accessible in other methods, so setting it like this is still likely to be an
# error, and we still emit a diagnostic. See https://github.com/astral-sh/ruff/pull/20048#discussion_r2296252395
class A:
    def set_class(self, cls):
        __class__ = cls
```

`__class__` is a local variable there because it is assigned. It would only be nonlocal with a `nonlocal __class__` statement.

---

_Comment by @ntBre on 2025-08-25 17:08_

Ah I see, thanks. For some reason I thought this should work even without the explicit `nonlocal`, but I think this resolves my confusion. This case is now fully resolved, not triggering PLE0117 or F841:

```py
class A:
    def set_class(self, cls):
        nonlocal __class__
        __class__ = cls
```

and these cases are properly flagged because the local `__class__` variable _is_ unused:

```py
class A:
    class B:
        def set_class(self, cls):
            __class__ = cls


class A:
    def foo():
        class B:
            print(__class__)
            def set_class(self, cls):
                __class__ = cls
```

I'll update the first test case with the actual `nonlocal` statement and also fix the comment.

---

_Comment by @dscorbett on 2025-08-25 17:34_

In the following example, `__class__` is a local variable, which breaks the zero-argument `super`.
```python
class C:
    def __str__(self):
        if False:
            __class__ = C
        return super().__str__()
print(str(C()))
```
With this PR, will that be handled properly?

---

_Comment by @AlexWaygood on 2025-08-25 17:39_

> With this PR, will that be handled properly?

No, that issue will still be outstanding after this PR has merged. (That doesn't mean that we won't necessarily fix it in the future. But while it's a valid bug report, it's probably low priority for us to fix it since it seems unlikely that a human or LLM would ever write that in real code :-)

---

_Comment by @ntBre on 2025-08-25 17:43_

I think https://github.com/astral-sh/ruff/pull/19424 will still be necessary to fix that, if that's the same issue. But the local variable should shadow the nonlocal binding from the cell after this PR, so I think it makes it slightly easier to resolve the other issue too, at least.

---

_Merged by @ntBre on 2025-08-26 13:49_

---

_Closed by @ntBre on 2025-08-26 13:49_

---

_Branch deleted on 2025-08-26 13:49_

---
