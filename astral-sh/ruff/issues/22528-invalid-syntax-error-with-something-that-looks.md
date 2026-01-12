```yaml
number: 22528
title: "invalid syntax error with something that looks like a `match` statement"
type: issue
state: open
author: KotlinIsland
labels:
  - bug
  - parser
assignees: []
created_at: 2026-01-12T08:42:37Z
updated_at: 2026-01-12T21:25:52Z
url: https://github.com/astral-sh/ruff/issues/22528
synced_at: 2026-01-12T22:24:39Z
```

# invalid syntax error with something that looks like a `match` statement

---

_@KotlinIsland_

### Summary

```py
from re import *

def main(x, y, z):
    match [x, y, z]:                        {   #<--- load bearing brace
        case [0, 1, 2]: print("a"),
        case [3, 4, 5]: print("b"),
        case [_, _, _]: exit(1)
                                            }   #<--- load bearing brace
    print("all good")

main(4, 2, 0)
```
[playground](https://play.ruff.rs/516ccd06-b6b5-436a-8dd7-a86420a746c1)

`Expected newline, found `{` (invalid-syntax) [Ln 4, Col 45]`

finders credit to @decorator-factory 

<details><summary>Solution</summary>
<p>

```py
from re import match

match[0]: int  # Expected newline, found name(invalid-syntax)
```

</p>
</details> 

---

_Comment by @MichaReiser on 2026-01-12 08:50_

wow... 

In case anyone else wonders as what this is supposed to parse. It's an annotated assignment (the target is a subscript expression)

```
Module(
   body=[
      ImportFrom(
         module='re',
         names=[
            alias(name='*')],
         level=0),
      FunctionDef(
         name='main',
         args=arguments(
            args=[
               arg(arg='x'),
               arg(arg='y'),
               arg(arg='z')]),
         body=[
            AnnAssign(
               target=Subscript(
                  value=Name(id='match', ctx=Load()),
                  slice=Tuple(
                     elts=[
                        Name(id='x', ctx=Load()),
                        Name(id='y', ctx=Load()),
                        Name(id='z', ctx=Load())],
                     ctx=Load()),
                  ctx=Store()),
               annotation=Dict(
                  keys=[
                     Subscript(
                        value=Name(id='case', ctx=Load()),
                        slice=Tuple(
                           elts=[
                              Constant(value=0),
                              Constant(value=1),
                              Constant(value=2)],
                           ctx=Load()),
                        ctx=Load()),
                     Subscript(
                        value=Name(id='case', ctx=Load()),
                        slice=Tuple(
                           elts=[
                              Constant(value=3),
                              Constant(value=4),
                              Constant(value=5)],
                           ctx=Load()),
                        ctx=Load()),
                     Subscript(
                        value=Name(id='case', ctx=Load()),
                        slice=Tuple(
                           elts=[
                              Name(id='_', ctx=Load()),
                              Name(id='_', ctx=Load()),
                              Name(id='_', ctx=Load())],
                           ctx=Load()),
                        ctx=Load())],
                  values=[
                     Call(
                        func=Name(id='print', ctx=Load()),
                        args=[
                           Constant(value='a')]),
                     Call(
                        func=Name(id='print', ctx=Load()),
                        args=[
                           Constant(value='b')]),
                     Call(
                        func=Name(id='exit', ctx=Load()),
                        args=[
                           Constant(value=1)])]),
               simple=0),
            Expr(
               value=Call(
                  func=Name(id='print', ctx=Load()),
                  args=[
                     Constant(value='all good')]))]),
      Expr(
         value=Call(
            func=Name(id='main', ctx=Load()),
            args=[
               Constant(value=4),
               Constant(value=2),
               Constant(value=0)]))])

```

---

_Label `bug` added by @MichaReiser on 2026-01-12 08:51_

---

_Label `parser` added by @MichaReiser on 2026-01-12 08:51_

---

_Comment by @dylwil3 on 2026-01-12 20:58_

A minimal fix for just this example would be something like this:

```diff
diff --git i/crates/ruff_python_parser/src/parser/statement.rs w/crates/ruff_python_parser/src/parser/statement.rs
index 0a56768df5..ae334e8b1d 100644
--- i/crates/ruff_python_parser/src/parser/statement.rs
+++ w/crates/ruff_python_parser/src/parser/statement.rs
@@ -2449,9 +2449,14 @@ impl<'src> Parser<'src> {
 
         match self.current_token_kind() {
             TokenKind::Colon => {
-                // `match` is a keyword
+                // `match` is a keyword or annotated assignment
                 self.bump(TokenKind::Colon);
 
+                if self.current_token_kind() != TokenKind::Newline {
+                    self.rewind(checkpoint);
+                    return None;
+                }
+
                 let cases = self.parse_match_body();
 
                 Some(ast::StmtMatch {
```

But I think the correct thing to do is mimic the CPython behavior more precisely. The relevant code is here:

https://github.com/python/cpython/blob/66e1399311c17684c6e26f5d9d9603fbd0717d0d/Parser/parser.c#L7765-L7778

and I believe the logic is to revert to treating `match` as an identifier unless we "match" (pun intended) the pattern: `match`, followed by valid `subject`, followed by `:` and `Newline` and indent, followed by one or more valid `case`s, followed by a `dedent`.

---

_Comment by @MichaReiser on 2026-01-12 21:25_

> But I think the correct thing to do is mimic the CPython behavior more precisely. The relevant code is here:

I'm not sure that's trivial, because it seems they perform a lookahead over the entire `case` (which can be an arbitrary number of tokens).

Unlike Python, we also strive to still parse as much as possible even in the presence of an error. So we need to be careful not to be too strict (but obviously, also not deny valid syntax)



---
