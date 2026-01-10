```yaml
number: 16479
title: "[syntax-errors] Type parameter lists before Python 3.12"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-type-params
created_at: 2025-03-03T19:11:10Z
updated_at: 2025-03-05T13:24:49Z
url: https://github.com/astral-sh/ruff/pull/16479
synced_at: 2026-01-10T19:49:01Z
```

# [syntax-errors] Type parameter lists before Python 3.12

---

_Pull request opened by @ntBre on 2025-03-03 19:11_

Summary
--

Another simple one, just detect type parameter lists in `type` statements, functions, and classes. [pyright] only reports the `type` statement in the alias case, whereas we'll currently report both diagnostics, in combination with #16478.

Test Plan
--
Inline tests.

[pyright]: https://pyright-play.net/?pythonVersion=3.8&strict=true&code=C4TwDgpgBAHg2gFQLpQLxQJYDthA

---

_Label `parser` added by @ntBre on 2025-03-03 19:11_

---

_Label `preview` added by @ntBre on 2025-03-03 19:11_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-03 19:11_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-03 19:11_

---

_Comment by @github-actions[bot] on 2025-03-03 19:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @NeilGirdhar on 2025-03-03 22:38_

This is [W2604](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/using-generic-type-syntax-in-unsupported-version.html) from Pylint.  You may want to add it [here](https://github.com/astral-sh/ruff/issues/970) and tick it off if you want :smile: I think your other PRs are similarly listed in Pylint.

---

_Comment by @ntBre on 2025-03-03 23:50_

Ah, thank you! I've only been tracking them in #6591. I'll update #970 too.

---

_Comment by @MichaReiser on 2025-03-04 08:07_

For reference: https://docs.python.org/3/reference/compound_stmts.html#type-parameter-lists


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:452 on 2025-03-04 08:08_

Should this be `TypeParameterList` or should we rename the message to `type parameters`? 

It might also be helpful to start documenting the variants. E.g. by adding a link to the Python documentation https://docs.python.org/3/reference/compound_stmts.html#type-parameter-lists

---

_@MichaReiser approved on 2025-03-04 08:09_

---

_Comment by @dhruvmanila on 2025-03-04 11:24_

> [pyright](https://pyright-play.net/?pythonVersion=3.8&strict=true&code=C4TwDgpgBAHg2gFQLpQLxQJYDthA) only reports the `type` statement in the alias case, whereas we'll currently report both diagnostics, in combination with #16478.

I think this is because the `type` statement itself was added in 3.12 so maybe it might be worth not raising this error for `type` statement as the entire statement is invalid?

---

_@dhruvmanila reviewed on 2025-03-04 11:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:3119 on 2025-03-04 11:26_

Can we add a case with multiple type parameters? Or, we can just mix them up by using single type parameter in function and multiple type parameters in class.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:452 on 2025-03-04 17:31_

`TypeParameterList` sounds good. I'll start documenting them too, I think that's a great idea. As in #16404, I included something similar to ruff rule docs in case we want to show these to users eventually.

---

_@ntBre reviewed on 2025-03-04 17:31_

---

_@ntBre reviewed on 2025-03-04 17:42_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:3119 on 2025-03-04 17:42_

I added an example from the [`UP046`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046_0.py) tests with bounds and constraints, as well as an unpacked `TypeVarTuple` and a `ParamSpec`.

We just mark the whole list at once, so there aren't additional errors for each type parameter.

---

_Comment by @ntBre on 2025-03-04 17:52_

> > [pyright](https://pyright-play.net/?pythonVersion=3.8&strict=true&code=C4TwDgpgBAHg2gFQLpQLxQJYDthA) only reports the `type` statement in the alias case, whereas we'll currently report both diagnostics, in combination with #16478.
> 
> I think this is because the `type` statement itself was added in 3.12 so maybe it might be worth not raising this error for `type` statement as the entire statement is invalid?

I think that's fair. I can just hoist the check to where `try_parse_type_params` is called for classes and functions instead of keeping it in `parse_type_params`.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1800 on 2025-03-05 02:47_

What's the reason for this condition? As a user I'd find it confusing if Ruff told me to first add the type parameters and then tell me to remove the entire list. Sorry if I missed this in my initial review if it was already present.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1933 on 2025-03-05 02:47_

Same as above.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:477 on 2025-03-05 02:59_

Does this work? Shouldn't the link be after `[..]:` on the same line?

---

_@dhruvmanila approved on 2025-03-05 02:59_

---

_@ntBre reviewed on 2025-03-05 03:08_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:1800 on 2025-03-05 03:08_

Oh that's a good point... I was just trying to avoid duplicates, but if anything, it might make sense to suppress the `ParseError` in this case. Removing the condition is certainly easiest though.

And you didn't miss it, I added it here in the revision when I added the empty test cases.

---

_@ntBre reviewed on 2025-03-05 03:08_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:477 on 2025-03-05 03:08_

It appeared to work in Emacs, but I can put it on one line to be safe!

---

_@dhruvmanila reviewed on 2025-03-05 03:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1800 on 2025-03-05 03:17_

I think I'd prefer keep it both. I don't think we should suppress any syntax errors.

I know this goes against what I said earlier in https://github.com/astral-sh/ruff/pull/16479#issuecomment-2697193450 but my argument for that would be that it's a new statement altogether. Maybe for now we can just avoid suppressing _any_ syntax errors based on other syntax errors and we can act on it based on user feedback? Happy to hear others thoughts on this.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1800 on 2025-03-05 07:27_

The parser does have some logic to avoid multiple errors at the same location for invalid syntax errors. https://github.com/astral-sh/ruff/blob/37fbe58b13b905bfafac326a74c922f72f3b54a1/crates/ruff_python_parser/src/parser/mod.rs#L424-L438

But I agree that we should solve this holisticly because it will otherwise be very error prone to avoid all possible errors at the same location

---

_@MichaReiser approved on 2025-03-05 07:28_

---

_@ntBre reviewed on 2025-03-05 13:08_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:1800 on 2025-03-05 13:08_

Ah nice! I'll remove these checks for now, and then maybe we can add `UnsupportedSyntaxError`s to that check or a similar one later.

---

_Merged by @ntBre on 2025-03-05 13:19_

---

_Closed by @ntBre on 2025-03-05 13:19_

---

_Branch deleted on 2025-03-05 13:19_

---
