```yaml
number: 14098
title: "[refurb] Parse more exotic decimal strings in `verbose-decimal-constructor (FURB157)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: decimal-parsing
created_at: 2024-11-04T23:37:12Z
updated_at: 2024-11-08T10:45:04Z
url: https://github.com/astral-sh/ruff/pull/14098
synced_at: 2026-01-10T20:50:57Z
```

# [refurb] Parse more exotic decimal strings in `verbose-decimal-constructor (FURB157)`

---

_Pull request opened by @dylwil3 on 2024-11-04 23:37_

FURB157 suggests replacing expressions like `Decimal("123")` with `Decimal(123)`. This PR extends the rule to cover cases where the input string to `Decimal` can be easily transformed into an integer literal.

For example:

```python
Decimal("1__000")   # fix: `Decimal(1000)`
```

Note: we do not implement the full decimal parsing logic from CPython on the grounds that certain acceptable string inputs to the `Decimal` constructor may be presumed purposeful on the part of the developer. For example, as in the linked issue, `Decimal("١٢٣")` is valid and equal to `Decimal(123)`, but we do not suggest a replacement in this case.

Closes #13807 


---

_Comment by @dylwil3 on 2024-11-04 23:40_

Happy to hear any suggestions around the strange mix of mutability and shadowing in the implementation. I was attempting to avoid allocating a String if we didn't have to, and to not change too much from the implementation that already existed... 

Also happy to delete some comments if they are contributing more to clutter than understanding.

---

_Comment by @github-actions[bot] on 2024-11-04 23:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:86 on 2024-11-05 06:55_

Nit: I don't think `memchr` gives us much here, considering that most literals are very short. We also don't have to use `replace` and allcoate a `Cow::Owned`. Instead, I would use `Cow::from(trimmed.trim_start_matches('_'))` for better readability (with the added benefit that it doesn't allocate)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:119 on 2024-11-05 07:05_

`replace` is technically `O(n)`. Not that it matters much here but I think I would find it slightly more readable if we explicitly tested for the `+` sign and the expected position.

```
if rest[index + 1] == b'+' {
    rest.into_owned().remove(index + 1)
}
```

The alternative is to use `remove_matches` which captures the semantic better than `repalce` with a empty string

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:110 on 2024-11-05 07:06_

It seems that the exponential syntax also supports negative numbers:

https://github.com/astral-sh/ruff/blob/51a998306a10aab1254740f760efaecc25213d71/crates/ruff_python_parser/src/lexer.rs#L1088-L1100

---

_@MichaReiser approved on 2024-11-05 07:08_

---

_@MichaReiser reviewed on 2024-11-05 07:14_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:129 on 2024-11-05 07:14_

Does this support `1_2_3_4`? or `1.1_2_3_4`?

---

_@sharkdp reviewed on 2024-11-05 07:41_

---

_Review comment by @sharkdp on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:118 on 2024-11-05 07:41_

I don't think the positive sign in the exponent is a syntax error?
```pycon
>>> 2e+3
2000.0
>>> Decimal(2e+3)
Decimal('2000')
```

---

_@sharkdp reviewed on 2024-11-05 07:50_

---

_Review comment by @sharkdp on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:129 on 2024-11-05 07:50_

I think the idea above was to replace all `_`s, not just leading ones.

---

_@MichaReiser reviewed on 2024-11-05 07:55_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:129 on 2024-11-05 07:55_

Good catch. I don't think replacing all of them is correct because `_` are not permitted in the exponent part.

---

_@UnknownPlatypus reviewed on 2024-11-05 07:59_

---

_Review comment by @UnknownPlatypus on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB157_FURB157.py.snap`:177 on 2024-11-05 07:59_

Maybe it would be nice to preserve the thousand separator in that case and suggest `1_000` instead of `1000`. (If it's not too annoying to do)

---

_@sharkdp reviewed on 2024-11-05 08:06_

---

_Review comment by @sharkdp on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:110 on 2024-11-05 08:06_

Yes, you can have negative exponents. But this lint should not suggest replacing `Decimal("1e-1")` with `Decimal(1e-1)`. They are not the same! `Decimal` offers a way to represent decimal numbers in an exact way, something that floating point representations do not support.
```py
>>>> Decimal("1e-1") == Decimal(1e-1)  # 1e-1 = 0.1 can not be represented exactly as a float
False
>>>> Decimal("5e-1") == Decimal(5e-1)  # Okay, 5e-1 = 1/2 can be represented exactly
True
```

I think we need to be similarly careful with large numbers, even if they are integers:
```py
>>>> Decimal("1e22") == Decimal(1e22)
True
>>>> Decimal("1e23") == Decimal(1e23)
False
```

---

_@sharkdp reviewed on 2024-11-05 08:11_

---

_Review comment by @sharkdp on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:129 on 2024-11-05 08:11_

I think they are:

https://github.com/python/cpython/blob/ac556a2ad1213b8bb81372fe6fb762f5fcb076de/Lib/_pydecimal.py#L463

```py
>>> Decimal("2e1_0") == Decimal(2e1_0)
True
```

---

_@MichaReiser reviewed on 2024-11-05 08:23_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:129 on 2024-11-05 08:23_

Understanding your own code is always the hardest. So what's not allowed is `Decimal(2._1)`

https://github.com/astral-sh/ruff/blob/4323512a6569eb1a820f91141f5798ba015628e6/crates/ruff_python_parser/src/lexer.rs#L1072-L1078

---

_@sharkdp reviewed on 2024-11-05 08:30_

---

_Review comment by @sharkdp on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:129 on 2024-11-05 08:30_

Yes. Leading/trailing underscores are also not supported in "plain" Python syntax: `_10`. But they are supported in `Decimal`s syntax.

But I don't think there is a problem in this PR? All underscores are replaced. So if someone really has `Decimal("_1_0_0__")` in their codebase, we would suggest replacing that with `Decimal(100)`, which is fine.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:86 on 2024-11-05 11:44_

I'm fine not using memchr, but I do think we need to use `replace` because underscores may appear anywhere in the string. For example- `Decimal("1_____00")` is valid.

Edit: whoops, didn't see that was already addressed below

---

_@dylwil3 reviewed on 2024-11-05 11:44_

---

_@dylwil3 reviewed on 2024-11-05 11:46_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:110 on 2024-11-05 11:46_

Just as @sharkdp points out, the omission of negative exponents was intentional. I didn't think about large exponents though, good point!

---

_@dylwil3 reviewed on 2024-11-05 12:07_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:119 on 2024-11-05 12:07_

I like `remove_matches` but isn't it nightly only? https://doc.rust-lang.org/std/string/struct.String.html#method.remove_matches

---

_@dylwil3 reviewed on 2024-11-05 12:18_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:118 on 2024-11-05 12:18_

You're absolutely right! Dunno why I thought it was...

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/refurb/FURB157.py`:18 on 2024-11-05 12:48_

Could we please also include cases for `Decimal("-inf")` and `Decimal("inf")` (both should error).

---

_@sbrugman reviewed on 2024-11-05 12:49_

---

_@dylwil3 reviewed on 2024-11-05 14:01_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:110 on 2024-11-05 14:01_

It's hard to figure out when this conversion is actually safe - parsing scientific notation is weird, and I can't seem to find a reference for the range where the representation is faithful. I think issues start to occur when numbers are larger than `1e16` but I'm having trouble finding a justification for that in code/documentation...

The smallest example I found so far was:

```bash
>>> Decimal(1801439850948199e1) == Decimal('1801439850948199e1')
False
```


---

_@dscorbett reviewed on 2024-11-05 14:20_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:110 on 2024-11-05 14:20_

FURB157 should not rewrite strings to float literals. Literals with exponents are float literals, so they should not be passed to the `Decimal` constructor, or they will trigger [`decimal-from-float-literal` (RUF032)](https://docs.astral.sh/ruff/rules/decimal-from-float-literal/).

```console
$ cat ruf032.py                                                   
from decimal import Decimal
Decimal(1e16)

$ ruff check --isolated --preview --select RUF032 ruf032.py 
ruf032.py:2:9: RUF032 `Decimal()` called with float literal argument
  |
1 | from decimal import Decimal
2 | Decimal(1e16)
  |         ^^^^ RUF032
  |
  = help: Use a string literal instead

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

---

_@dylwil3 reviewed on 2024-11-05 14:36_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:110 on 2024-11-05 14:36_

Well that makes life easier! Thank you!

---

_Comment by @dylwil3 on 2024-11-05 14:56_

Sorry for sending everyone down an exponential rabbit hole, and thanks @dscorbett for digging me out of it!

Exponent handling has been removed since it takes us out of the world of integer literals.

---

_@MichaReiser reviewed on 2024-11-05 18:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:119 on 2024-11-05 18:06_

Ah dang. I guess replace is still the best option 

---

_Comment by @MichaReiser on 2024-11-05 18:07_

@dylwil3 feel free to merge at your own discretion 

---

_@dylwil3 reviewed on 2024-11-05 19:13_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB157_FURB157.py.snap`:177 on 2024-11-05 19:13_

That's a good suggestion, and I think it would be a nice followup! I think one would want to trim leading/trailing underscores, and then replace every internal, contiguous sequence of underscores with a single underscore. At that point it may be worth a little refactoring to just walk through the string once and abort early as appropriate...

---

_Merged by @dylwil3 on 2024-11-05 19:33_

---

_Closed by @dylwil3 on 2024-11-05 19:33_

---

_Branch deleted on 2024-11-05 19:33_

---

_Label `bug` added by @dhruvmanila on 2024-11-08 10:45_

---
