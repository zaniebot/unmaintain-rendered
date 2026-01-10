```yaml
number: 16523
title: "[syntax-errors] Parenthesized context managers before Python 3.9"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-parenthesized-context-managers
created_at: 2025-03-05T17:48:47Z
updated_at: 2025-03-17T12:54:56Z
url: https://github.com/astral-sh/ruff/pull/16523
synced_at: 2026-01-10T19:49:01Z
```

# [syntax-errors] Parenthesized context managers before Python 3.9

---

_Pull request opened by @ntBre on 2025-03-05 17:48_

Summary
--

I thought this was very complicated based on the comment here: https://github.com/astral-sh/ruff/pull/16106#issuecomment-2653505671 and on some of the discussion in the CPython issue here: https://github.com/python/cpython/issues/56991. However, after a little bit of experimentation, I think it boils down to this example:

```python
with (x as y): ...
```

The issue is parentheses around a `with` item with an `optional_var`, as we (and [Python](https://docs.python.org/3/library/ast.html#ast.withitem)) call the trailing variable name (`y` in this case). It's not actually about line breaks after all, except that line breaks are allowed in parenthesized expressions, which explains the validity of cases like 


```pycon
>>> with (
...     x,
...     y
... ) as foo:
...     pass
... 
```

even on Python 3.8.

I followed [pyright]'s example again here on the diagnostic range (just the opening paren) and the wording of the error.


Test Plan
--
Inline tests

[pyright]: https://pyright-play.net/?pythonVersion=3.7&strict=true&code=FAdwlgLgFgBAFAewA4FMB2cBEAzBCB0EAHhJgJQwCGAzjLgmQFwz6tA


---

_Label `parser` added by @ntBre on 2025-03-05 17:48_

---

_Label `preview` added by @ntBre on 2025-03-05 17:48_

---

_Comment by @github-actions[bot] on 2025-03-05 17:57_

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

_Marked ready for review by @ntBre on 2025-03-05 18:45_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-05 18:45_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-05 18:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2114 on 2025-03-06 07:28_

Do we also need to emit the same diagnostic on line 2144, e.g. for `with (foo, bar):` 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:702 on 2025-03-06 07:28_

It would be nice to add documentation, similar to what you did for other variants.

---

_@MichaReiser reviewed on 2025-03-06 07:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@parenthesized_context_manager_py38.py.snap`:179 on 2025-03-06 07:29_

@BurntSushi's new diagnostic system will allow us to mark both parentheses.

---

_@dhruvmanila reviewed on 2025-03-06 11:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2114 on 2025-03-06 11:42_

Should we move this check down below where we're sure that the parenthesis belongs to the with items and not the expression? i.e., on line 2162
```rs
        if with_item_kind.is_parenthesized() {
```

---

_@dhruvmanila reviewed on 2025-03-06 11:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2114 on 2025-03-06 11:44_

Or, in `parse_with_items` like:
```rs
        if self.at(TokenKind::Lpar) {
			let lpar_range = self.current_token_range();
            if let Some(items) = self.try_parse_parenthesized_with_items() {
				// check target version and raise error
                self.expect(TokenKind::Rpar);
                items
            } else {
```

---

_@ntBre reviewed on 2025-03-06 14:30_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:2114 on 2025-03-06 14:30_

No, that syntax is actually okay. I think `(foo, bar)` is just a tuple, so it falls under the expression grammar. The tuple doesn't have an `__enter__` method, so it causes a runtime error, but it does parse and even compile I guess.

As far as I can tell, the problem with parentheses is specific to the `as` case since that's part of the `with` statement grammar.

---

_@ntBre reviewed on 2025-03-06 14:31_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:2114 on 2025-03-06 14:31_

```pycon
Python 3.7.9 (default, Aug 23 2020, 00:57:53)
[Clang 10.0.1 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import ast
>>> ast.parse("with (foo, bar): ...")
<_ast.Module object at 0x70849fd20c10>
>>> ast.dump(_)
"Module(body=[With(items=[withitem(context_expr=Tuple(elts=[Name(id='foo', ctx=Load()), Name(id='bar', ctx=Load())], ctx=Load()), optional_vars=None)], body=[Expr(value=Ellipsis())])])"
```

---

_@ntBre reviewed on 2025-03-06 15:13_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:2114 on 2025-03-06 15:13_

Oh nice, I moved it to `parse_with_items`. I just had to add a separate check for `optional_vars` otherwise we get a false positive on the case Micha mentioned above. I added that as a `test_ok` case to be sure.

---

_Comment by @ntBre on 2025-03-06 15:29_

Thanks for the reviews! I 
* relocated the check to `parse_with_items`
* added variant docs
* expanded the `test_ok` cases with Micha's suggestion and some of the other cases I put in the docs

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2114 on 2025-03-07 08:20_

Ah right. I only remembered that we can't parenthesize multiple context managers in the formatter but that is because it would change the semantics (it would make it a tuple).

So yes, the problem is that not a single context manager is allowed to use `as`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2069 on 2025-03-07 08:21_

Let's add an example where only some context managers use `as`
```suggestion
                    // with (foo as x, bar as y): ...
                    // with (foo, bar as y): ...
                    // with (foo as x, bar): ...
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2046 on 2025-03-07 08:34_

It is not directly related to your PR, but it is somewhat related that the parser now supports a `target_version` option.

If given:

```py
with (CtxManager1(), CtxManager2()):
    ...
```


* Pre 3.9: The context manager expression is a tuple which is always a runtime error (I think)
* 3.9 or newer: This parses as a multiple-item context manager.


There are two concerns here:

1. Always parsing the above as a context manager with multiple with items affects downstream tools. E.g., a type checker will error when parsed as a tuple because the tuple doesn't implement the context manager protocol. However, the code might be valid when parsed as a with statement with multiple context expressions. 
2. We'll miss errors that are technically not syntax errors, but they are most certainly not what the user wanted. 

I think we have two options here:

1. We make the backwards compatibility stricter than it has to be and flag any parenthesized context manager with more than one expression or with a trailing comma as an error because it is parsed as a tuple on Python 3.8 or older
2. We leave it as is and consider this a concern of a lint rule to catch tuples in context manager positions.

I'm not sure if we should change the parser to correctly parse this as a tuple instead of a statement with multiple context managers. I think it's fine to leave it as is for 1, but we should definitely change the parser if we go for 2.

I just tested how pyright handles this and pyright always flags:

```
with (CtxManager1(), CtxManager2()):
    ...
```

While this is not a 100% correct, I think it's accurate enough and could be a simplified fix for 1 and 2, but I'm interested in more opinions.


---

_@MichaReiser reviewed on 2025-03-07 08:37_

Okay, this is a bit more subtle. I wrote a longer inline comment about the problem and listed a few options

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2046 on 2025-03-07 11:00_

Hmm, that's a good point.

> * Pre 3.9: The context manager expression is a tuple which is always a runtime error (I think)

Yes, it's a runtime error. I quickly tested it out:

<details><summary>Context managers as tuple test code</summary>
<p>

```py
from contextlib import contextmanager


@contextmanager
def context1():
    yield 1


@contextmanager
def context2():
    yield 2


with (context1(), context2()):
    ...
```

</p>
</details> 

My initial thought was to change the parser to emit the correct AST as per the target version but that is something that we only need to question for this specific change as it seems to be the only change which not only changes the grammar but that in turn changes the parsed AST depending on the target version. But, I'm not sure if that'd actually provide any advantages.

I think my preference would be to go for (1) given that (a) it's already done by Pyright and I don't see any issues that users have brought up, (b) mypy doesn't flag this case on 3.8 either ([mypy playground](https://mypy-play.net/?mypy=latest&python=3.8&gist=d1986f8345de346c91188f530b71a3e9&flags=show-traceback,strict)), and (c) [Python 3.8 is has reached EOL](https://devguide.python.org/versions/).

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:2046 on 2025-03-07 14:59_

Oh that's a very good point, thanks for catching that.

Option (1) sounds like the easier of the two, but I see some appeal in having it as a lint rule too. I guess it's a bit weird to flag this as a `SyntaxError`, but like you said, I don't think there's any way for this not to cause an error at runtime. It also seems like a pain to change the parser for such an old Python version.

[pyright] is actually a little touchy here, I think. It flags the case with multiple elements as a parenthesized context manager, but for a single-element tuple, you get a type error about the tuple not implementing `__enter__` and `__exit__`.

[pyright]: https://pyright-play.net/?pythonVersion=3.8&strict=true&code=O4SwLgFgBAFA9gBwKYDsYCIBmc4DowAeY6AlADRSKoYBGAhgE75GkkBcUuXAUN6JLCposOZsXLtOXIA

---

_@ntBre reviewed on 2025-03-07 14:59_

---

_@dhruvmanila reviewed on 2025-03-07 18:18_

(A bit embarrassing but I just found out that my comment was still pending, submitting it now)

---

_Comment by @ntBre on 2025-03-14 19:54_

I implemented option (1) as discussed above (always flagging cases that we parse as parenthesized context managers).

The only remaining `ok` case is `with (foo, bar, baz) as tup: ...`. This could be a good candidate for a lint rule because we parse this accurately as a parenthesized expression within the `with` statement, making it actually accessible to ruff or red-knot as a tuple without having to modify the parser. This matches pyright too, which raises a type error about a tuple not implementing `__enter__` or `__exit__`.

---

_@MichaReiser approved on 2025-03-17 08:22_

---

_Merged by @ntBre on 2025-03-17 12:54_

---

_Closed by @ntBre on 2025-03-17 12:54_

---

_Branch deleted on 2025-03-17 12:54_

---
