```yaml
number: 22315
title: ANN201 Incorrect autofix return type for raise in loop
type: issue
state: open
author: spaceby
labels:
  - bug
assignees: []
created_at: 2025-12-31T10:47:23Z
updated_at: 2026-01-12T15:47:27Z
url: https://github.com/astral-sh/ruff/issues/22315
synced_at: 2026-01-12T15:54:58Z
```

# ANN201 Incorrect autofix return type for raise in loop

---

_@spaceby_

### Summary

https://play.ruff.rs/f2c7c8a3-1e38-425f-b1e5-0068e6f8b67b

```python
class Test:
    items_list = []

    def method(self):
        for item in self.items_list:
            raise Exception(item)


t = Test()
t.method()
```

This code returns None.
However, autofix substitutes in method NoReturn

```python
from typing import NoReturn
class Test:
    items_list = []

    def method(self) -> NoReturn:
        for item in self.items_list:
            raise Exception(item)


t = Test()
t.method()
```

### Version

0.14.10

---

_Renamed from "ANN201 Incorrect autofix" to "ANN201 Incorrect autofix return type for raise in loop" by @spaceby on 2025-12-31 10:49_

---

_Comment by @ntBre on 2025-12-31 15:27_

Thanks for the report!

This is related to the same code Micha mentions in https://github.com/astral-sh/ruff/issues/15975#issuecomment-2639121847. It looks like we treat loops as always being entered.

We do hope to have more general control-flow analysis at some point (https://github.com/astral-sh/ruff/issues/17065), but maybe there's something we can improve here in the meantime.

---

_Label `bug` added by @ntBre on 2025-12-31 15:27_

---

_Comment by @11happy on 2026-01-10 10:51_

Hii @ntBre , for the above issue as you mentioned loops are assumed to always execute atleast once, which causes for the above tests case to result in `Terminal::Raise` behaviour eventually returning `NoReturn` , for solution could we check if iterable in `for` & `while` stmt  is guranteed to be non-empty something along these lines: 
```
fn nonempty_tmep_check(iter: &Expr) -> bool {
    match iter {
        Expr::List(ast::ExprList { elts, .. }) => !elts.is_empty(),
        Expr::Tuple(ast::ExprTuple { elts, .. }) => !elts.is_empty(),
        Expr::Set(ast::ExprSet { elts, .. }) => !elts.is_empty(),
        Expr::Dict(ast::ExprDict { items, .. }) => !items.is_empty(),
        Expr::StringLiteral(ast::ExprStringLiteral { value, .. }) => !value.is_empty(),
        Expr::BytesLiteral(ast::ExprBytesLiteral { value, .. }) => !value.is_empty(),
        _ => false,
    }
}
```

& then in `Terminal::from_body` we can only apply terminal behaviour if we know the loops executes, however how do we make sure for function calls or other variables ? What do you think of this approach please let me know.

Also while exploring the codebase I noticed both `sometimes_break` & `always_break` have same doc comments, I think `always_break` should have correct doc comment.
```
/// Returns `true` if the body may break via a `break` statement.
fn sometimes_breaks(stmts: &[Stmt]) -> bool 

 /// Returns `true` if the body may break via a `break` statement.
fn always_breaks(stmts: &[Stmt]) -> bool

```



---

_Comment by @close2code-palm on 2026-01-11 11:18_

Hello. As I researched, we need to decide, if `test` in while \ for loop expression node are always falsy \ empty. One approach is to assume they are not excepted to be always truthy or non-empty, and this should solve the problem, but currently this is hard as this needs some kind of heavy analysis, and it still wont be correct sometimes, we'll just revert the situation. If you believe it’s better for fixes to miss some cases rather than produce incorrect results, we could consider modifying `Terminal::from_function` changes, the most simple to say is to take into account all parent nodes before to make a decision - we can return(upd - pass to and_then. By the way, "Terminal::Return" seems impossible by Rust code flow) just `Terminal::RaiseOrReturn` instead `Terminal::Raise` when there is at least one loop(later it will be loop on which we are not sure if it is iterated at least once) in ancestors.

---

_Comment by @ntBre on 2026-01-12 15:47_

Thank you both for looking into this! I would probably lean toward not implementing anything for this rule specifically and instead waiting for more general control-flow analysis in Ruff.

Checking the length/truthiness of the loop iterable does sound somewhat promising, but I think there will be tricky edge cases there too. The snippet in this issue is a good example of that because the `items_list` field could be modified outside the class (or even outside the file). So I think it's probably not worth any added complexity in the rule, especially since this is already an unsafe fix.

---
