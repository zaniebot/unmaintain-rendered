```yaml
number: 14538
title: TC003’s rewritten annotations have syntax errors and type-checking problems
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2024-11-22T19:12:27Z
updated_at: 2024-11-27T17:58:49Z
url: https://github.com/astral-sh/ruff/issues/14538
synced_at: 2026-01-12T15:54:53Z
```

# TC003’s rewritten annotations have syntax errors and type-checking problems

---

_@dscorbett_

The fix for TC003 sometimes makes Ruff fail or introduces type-checking problems when `lint.flake8-type-checking.quote-annotations = true` in Ruff 0.8.0.

Here are some examples of type-checking problems. The first two make the types invalid. The rest change the types’ metadata. Metadata can be any Python expression and can be interpreted by any arbitrary tool, so Ruff should not make any assumptions about it or modify it beyond simple syntactic transformations like switching quotation marks.
```console
$ cat tc003_1.py
from collections.abc import Sequence
from typing import Annotated
a: Sequence[tuple[()]]
b: Sequence[Annotated[int, ()]]
c: Sequence[Annotated[int, 1 + 3]]
d: Sequence[Annotated[int, ([0] + [1])[1]]]
e: Sequence[Annotated[int, list["int"]]]
f: Sequence[Annotated[int, Annotated["int", "int"]]]

$ ruff check --isolated --select TC003 --config 'lint.flake8-type-checking.quote-annotations = true' tc003_1.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat tc003_1.py
from typing import Annotated, TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Sequence
a: "Sequence[tuple[]]"
b: "Sequence[Annotated[int, ]]"
c: "Sequence[Annotated[int, 1 | 3]]"
d: "Sequence[Annotated[int, [0] + [1][1]]]"
e: "Sequence[Annotated[int, list[int]]]"
f: "Sequence[Annotated[int, Annotated[int, 'int']]]"

$ mypy tc003_1.py
tc003_1.py:5: error: Invalid type comment or annotation  [valid-type]
tc003_1.py:6: error: Annotated[...] must have exactly one type argument and at least one annotation  [valid-type]
Found 2 errors in 1 file (checked 1 source file)
```

Here is an example of a failure.
```console
$ cat tc003_2.py
from collections.abc import Sequence
from typing import Annotated
x: Sequence[Annotated[int, "x"[0]]]

$ ruff check --isolated --select TC003 --config 'lint.flake8-type-checking.quote-annotations = true' tc003_2.py --unsafe-fixes --diff

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `tc003_2.py`, the rule codes TC003, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```

---

_Label `bug` added by @AlexWaygood on 2024-11-22 21:54_

---

_Label `fixes` added by @AlexWaygood on 2024-11-22 21:54_

---

_Label `help wanted` added by @AlexWaygood on 2024-11-22 21:54_

---

_Comment by @Daverball on 2024-11-23 07:42_

This looks to be the culprit, instead of properly handling 1 and 0 element tuples, 0 element tuples get skipped by `QuoteAnnotator::visit_expr` and 1 element tuples will not receive a trailing comma:

https://github.com/astral-sh/ruff/blob/4ba847f250ea2022d93df8162f7e35aed6dd5381/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L397-L410

Should be a fairly easy fix, but I won't have time to get around to it today. So if someone else wants to write a couple of test cases and handling for those two cases, go right ahead.

For 0 element tuples you just have to `self.annotation.push("()")`, for the 1 element case you just want to make sure that a trailing comma gets emitted, because currently it will not be.

---

_Comment by @Daverball on 2024-11-23 14:01_

There are probably additional corner cases though, i.e. the ones where the tuple needs to be enclosed in parentheses in order to be syntactically distinguishable.

So something like `Annotated[str, foo((1,2), "bar")]` would likely be broken too by being translated into `"Annotated[str, foo(1, 2, 'bar')]"`.

However it's somewhat questionable to quote anything containing `Annotated` to begin with, since it will likely be used at runtime, so maybe we should just not emit a fix (or violation) when we see `typing.Annotated`. In that case we would almost certainly only get corner cases for things that aren't syntactically valid type annotations, after fixing the 0-element tuple and 1-element tuple.

But we could make certain we avoid bad cases by marking the fix invalid, as soon as we see an `ast.Tuple` with a parent node that isn't `ast.Subscript`.

---

_Comment by @Daverball on 2024-11-24 11:57_

I'm gonna take a stab at this. It shouldn't actually be too hard to fix the other corner-cases either, since most nodes aren't traversed manually and just rely on `Generator`, so I just need to make sure tuples are enclosed in parentheses unless the previous node was `Expr::Subscript`.

There's still a larger of issue of whether or not we should allow any of the references inside type definitions to be quoted, when that type definition contains an `Annotated`, since that is a clear signal that the annotation will be inspected. But that's a much more tricky thing to fix and also affects references to type aliases that need to be available at runtime. This may be the reason why the fix is currently marked unsafe.

---

_Comment by @Daverball on 2024-11-24 12:47_

The longer I look at this the more subtle problems stick out at me. The approach with a custom visitor that tries to get away with handling just a small subset of nodes is never going to be sound and by the time it is sound we may just have duplicated all the code of `Generator::unparse_expr` plus some additional branches.

We will either need to extend `Generator` with a flag to optionally parse the value of `LiteralString` if it's inside a type definition, so all the nested quotes get removed from the expression (but that would mean that state would now have to live on the `LiteralString` node rather than only on the semantic model during traversal). Or we need to generate that expanded expression ourselves and pass it to `Generator`.


---

_Comment by @Daverball on 2024-11-24 13:29_

@charliermarsh Would you be opposed to adding a boolean field to `ExprStringLiteral` to indicate whether or not that string literal is a typing forward reference? It's kind of piggy, since we're leaking state of the semantic model into the AST, so the value will be meaningless until the semantic model has already traversed that branch of the AST and populated the value.

But it also seems like the most efficient way to detect and expand forward references in `Generator`. Another potential way to do it is to give `Generator` a `Option<&SemanticModel>`, so it can optionally detect `typing.Literal` and `typing.Annotated` during `ast::Subscript` traversal.

It might also be possible to leverage the cache of parsed forward references to detect whether or not `ExprStringLiteral` is a forward reference, but then we would rely on those forward references already having been visited and parsed before, which is probably not true for nested cases.

Or my least favorite way: Copy `Generator::unparse_expr` and make the necessary changes for removing redundant nested quotes and risk the two methods going out of sync.

Or we just give up for now and only provide a naïve fix, that either doesn't expand nested quotes, that are safe to expand or only provides a fix in fewer cases where we don't have to worry about the complexity of nested quotes (which would essentially mean reverting to what we had before switching to this more powerful but broken version or something even more simple).

---

_Comment by @Daverball on 2024-11-24 14:21_

Actually, tagging wouldn't handle nested forward references properly either, unless the forward reference is already sitting parsed in the cache and has already been visited by the semantic model. So it's probably not worth adding this to the AST after all.

Maybe it would be better to add a separate opinionated rule about nested forward references and the fix for that would remove one level of nesting at a time. Then we could rely on repeated fixes eventually giving us the desired result.

---

_Comment by @MichaReiser on 2024-11-24 20:53_

> Actually, tagging wouldn't handle nested forward references properly either, unless the forward reference is already sitting parsed in the cache and has already been visited by the semantic model. So it's probably not worth adding this to the AST after all.

It might also not be possible because you would have to mutate the tree in the semantic model, or use a `Cell` which I rather avoid :)

> Maybe it would be better to add a separate opinionated rule about nested forward references and the fix for that would remove one level of nesting at a time. Then we could rely on repeated fixes eventually giving us the desired result.

The challenge is that there's no guarantee that this rule is enabled or that the rule is run and fixed first. 

I haven't studied the problem in detail but is the rewrite a common problem? Could we just not emit a fix in those cases?

---

_Comment by @Daverball on 2024-11-24 21:43_

> I haven't studied the problem in detail but is the rewrite a common problem? Could we just not emit a fix in those cases?

That's pretty much how it worked previously IIRC, if there were any quotes at all in the unparsed expression no fix would be emitted. But since `Literal` and type expressions that already contain some forward references are quite common, I assume there was a desire to improve the fix, hence the change to the current version that can handle some common cases, but is easily broken with more complex ones. I think I could easily improve the current fix to a degree where it would be correct in 99% of cases, but there would be no way to make it 100% robust. So if someone puts something unexpected inside a type definition, it will break, and it will probably do so quietly, which is the worst kind of breakage, since Ruff will now silently rewrite code and there would be no easy way to detect it.

I assume it would be easy to write a fix that always works, all you'd have to do is put the unparsed expression into a new `StringExpr` node and then unparse that node. That should take care of nested quotes by escaping them. You could even avoid introducing escaped quotes in the most common case by inverting the setting for the preferred quotes in the stylizer for the first unparse.

The major drawback to this version of the fix would be that you would introduce nested quotes for existing forward references, which usually is undesirable (I can imagine that there are some corner cases in runtime typing where nested forward references are the only way to make some code work, but that should be rare, I certainly have never seen it out in the wild). Nested quotes for `Literal` on the other hand are required. The same goes for any quotes within non-type expressions in `Annotated`, since those should be actual strings instead of forward references. If there was no mix then the fix would be easy too, you'd just remove all the quotes.

> The challenge is that there's no guarantee that this rule is enabled or that the rule is run and fixed first.

I can see how it would be a problem for a type expression that already contains nested forward references, but now needs to be quoted as a whole. In which case the two fixes would overlap. I don't really know how Ruff deals with overlapping edits.

But the other way around it seems unproblematic to me. If quoting the annotation introduces nested annotations, then if someone cares about getting rid of those they will be flagged by the other rule and subsequently fixed. The fixes should always stabilize this way.

---

_Comment by @Daverball on 2024-11-26 10:49_

@MichaReiser I guess the most sensible approach would be to extract `Generator::unparse_expr` and its dependencies into a trait with a few minor changes so implementations can decide what happens when visiting `ast::Subscript` and `ast::StringLiteral`, since those are the two points we need to be able to hook into in order to implement automatic expansion of forward references during node traversal. What do you think?

My only concern would be that the trait would look a bit arbitrary, since it would be tailored exactly for this use-case, when there may well be other `Generator` traversal operations people would like to be able to hook into. If we wanted to actually be more generic a `derive` macro may make more sense, but it doesn't seem worth the additional effort right now.

---

_Comment by @Daverball on 2024-11-26 11:00_

Actually, one common hook at the start of `unparse_expr` that can skip further traversal similar to `SourceOrderVisitor::enter_node` may be viable as well and would be a lot more flexible, although it would mean that we would potentially do the work of figuring out what kind of node we're visiting twice.

Edit: Looks like this doesn't quite work either, since we would also need a post-traversal hook, so we can pop the current annotation state off the stack. But those two hooks together should be able to do a lot of different things.

---

_Closed by @MichaReiser on 2024-11-27 17:58_

---
