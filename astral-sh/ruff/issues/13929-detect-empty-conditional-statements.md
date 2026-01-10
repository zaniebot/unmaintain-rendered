```yaml
number: 13929
title: Detect empty conditional statements
type: issue
state: open
author: gpauloski
labels:
  - rule
assignees: []
created_at: 2024-10-25T16:11:02Z
updated_at: 2024-12-16T13:05:21Z
url: https://github.com/astral-sh/ruff/issues/13929
synced_at: 2026-01-10T11:09:55Z
```

# Detect empty conditional statements

---

_Issue opened by @gpauloski on 2024-10-25 16:11_

## Problem
Code deletion rules in ruff can sometimes leave unnecessary conditional blocks. Consider this common conditional import logic.
```python
import sys

if sys.version_info >= (3, 11):
    from typing import Self
else:
    from typing_extensions import Self
```
If the file is refactored such that `Self` is no longer used, `ruff check --fix` will remove the unused imports leaving an empty if-else statement:
```python
import sys

if sys.version_info >= (3, 11):
    pass
else:
    pass
```
This kind of statement is useless and could easily be overlooked when introduced by auto-formatting.

## Related

This problem was originally raised in #9472, but I'm opening this issue to discuss the scope of a new rule in more detail.

There are a couple related rules that don't quite cover this problem:
* [`RUF034`](https://docs.astral.sh/ruff/rules/useless-if-else/) detects useless if-else expressions, but not statements (e.g, `foo = x if y else x`).
* [`TCH005`](https://docs.astral.sh/ruff/rules/empty-type-checking-block/#empty-type-checking-block-tch005) can only detect empty `TYPE_CHECKING` blocks.

## Proposal

A new `empty-if-statement` rule that detects if/if-else/if-elif-else statements where all arms are empty. This can have an autofix, but it would be an unsafe autofix since the test in the conditional could have a side-effect even if the body is empty.

As @MichaReiser brought up in #9472, there are some questions about scope:
* Should an empty `else` in a `for`/`try` statement be detected?
* What about an empty `elif` between a non-empty `if` and `else`?
* Potential nuance with typing stubs.

If we are unsure about the first two, they could be ignored for now and revisited in a followup.

---

_Comment by @MichaReiser on 2024-10-26 08:31_

Thanks for writing this up. Such a rule overall seems useful to me. 

> If we are unsure about the first two, they could be ignored for now and revisited in a followup.

I could see us start with if-statements. The only risk is that adding new control flow statements could require renaming the rule. 

> What about an empty elif between a non-empty if and else?

I could see us detecting this but not fixing (we can try by inverting the condition). 

CC: @AlexWaygood 

---

_Comment by @gpauloski on 2024-10-29 14:31_

> The only risk is that adding new control flow statements could require renaming the rule.

Makes sense. Is there a case for more granular rules? I.e., the following could be added more incrementally: `empty-if-else-statement`, `empty-for-else-statement`, `empty-try-else-statement`.

---

_Comment by @MichaReiser on 2024-10-29 15:17_

> Makes sense. Is there a case for more granular rules? I.e., the following could be added more incrementally: empty-if-else-statement, empty-for-else-statement, empty-try-else-statement.

We have done that in the past if there are reason why a user would only want to enable one, but not the other. I'm otherwise leaning towards fewer rules because rules have a certain cognitive overhead. 

---

_Comment by @gpauloski on 2024-10-29 20:58_

I don't see an obvious reason why one would only enable the check for one of the conditional types, so a single rule makes most sense to me.

How about changing the name to `empty-conditional-statement` which checks the following cases:
1. Empty `if` statement where *all* arms are empty (unsafe autofix since the test could have side-effects).
2. Empty `else` clause in `if` statement (safe autofix).
3. Empty `else` clause in `for` statement (safe autofix).
4. Empty `else` clause in `try-except` statement (safe autofix).

Does it make sense to not autofix if there's a comment? E.g., detect, but not fix, scenarios like:
```python
if condition:
    # Comment ...
    pass

try:
    foo()
except FooException:
    bar()
else:
    # Comment ...
    pass
```

> What about an empty elif between a non-empty if and else?
> I could see us detecting this but not fixing (we can try by inverting the condition).

I lean towards not checking this case. The more general name of `empty-conditional-statement` would support this addition better in the future if it was wanted, but I've also seen empty `elif` branches used to explicitly indicate a case is being ignored.

---

_Comment by @gpauloski on 2024-10-29 21:45_

I was looking at related rules to see how they handle comments. I don't see a clear standard but each of these comes from a different tool anyways.
- [`empty-type-checking-block`](https://docs.astral.sh/ruff/rules/empty-type-checking-block/) deletes comments in an otherwise empty if statement. E.g.,
  ```python
  from typing import TYPE_CHECKING
  
  if TYPE_CHECKING:
      # Comment ...
      pass
  ```
- [`useless-try-except`](https://docs.astral.sh/ruff/rules/useless-try-except/) has no autofix so won't delete comments.
- [`useless-else-on-loop`](https://docs.astral.sh/ruff/rules/useless-else-on-loop/) de-indents the comment.

---

_Label `rule` added by @MichaReiser on 2024-10-30 07:45_

---

_Comment by @MichaReiser on 2024-10-30 07:49_

This sounds good to me. The name probably requires some more bikeshedding but it shouldn't prevent us from start working towards this rule (awaiting @AlexWaygood input)

---

_Comment by @MichaReiser on 2024-12-16 11:10_

After some internal discussion, we agree that this should be multiple separate rules. I suggest that we scope them similar to clippy:

* [`needless_if`](https://rust-lang.github.io/rust-clippy/master/index.html#needless_if): Detects empty `if` statements, excluding if statements where the body contains comment
* [`needless_else`](https://rust-lang.github.io/rust-clippy/master/index.html#needless_else) Detects empty `else` branches (excluding bodies that contain comments)
* [`if_not_else`](https://rust-lang.github.io/rust-clippy/master/index.html#/if_not_else): I don't think we should add this rule just yet but we could consider adding it in the future to "clean up" `if a: pass else: code` after ruff fixes 
 
 
The `needless_if` rule should also help with https://github.com/astral-sh/ruff/issues/9472

---

_Comment by @InSyncWithFoo on 2024-12-16 12:22_

@MichaReiser Does `needless_else` apply to non-`if` `else` clauses too? What about `for`, `while` and `try`/`finally`?

---

_Comment by @MichaReiser on 2024-12-16 12:23_

> [<img alt="" width="16" height="16" src="https://avatars.githubusercontent.com/u/1203881?s=64&amp;u=a4198e6ec750b50d8f708f7b226cc6fec288b993&amp;v=4">@MichaReiser](https://github.com/MichaReiser?rgh-link-date=2024-12-16T12%3A22%3A08.000Z) Does `needless_else` apply to non-`if` `else` clauses too? What about `for`, `while` and `try`/`finally`?

Yes, I think it should. But maybe I'm overlooking something? Are there cases where you think it should not?

---

_Comment by @InSyncWithFoo on 2024-12-16 12:42_

The three functions implemented in #14763 are statement-wise (i.e., one for `if`/`elif`/`else`, one for `for`/`else`, one for `try`/`else`/`finally`). Each function can thus be neatly placed in a branch (`Stmt::If`, `Stmt::For`, `Stmt::Try`).

If `needless-else` applied to all kinds of `else`, it would need as many implementations as there are such statements (currently 4), since the range of `else` cannot be determined without looking at the parent statement.

Rust does not have exotic `else`s, so one `needless-else` is all Clippy needs. We, on the other hand, do have those, so it might not be a good idea to just copy Clippy in this specific case.

---

_Comment by @MichaReiser on 2024-12-16 13:05_

> If needless-else applied to all kinds of else, it would need as many implementations as there are such statements (currently 4), since the range of else cannot be determined without looking at the parent statement.

I think that's fine because it's just an implementation detail. I'm mainly concerned about why a user would want the rule to apply only to one kind of node rather than all of them. I currently can't think of any because an empty else block is always useless, regardless of its parent node.

---
