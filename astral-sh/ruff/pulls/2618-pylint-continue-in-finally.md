```yaml
number: 2618
title: "[`pylint`]: continue-in-finally"
type: pull_request
state: closed
author: colin99d
labels: []
assignees: []
base: main
head: continue-in-finally
created_at: 2023-02-07T02:29:26Z
updated_at: 2023-02-09T00:20:39Z
url: https://github.com/astral-sh/ruff/pull/2618
synced_at: 2026-01-12T04:52:00Z
```

# [`pylint`]: continue-in-finally

---

_Pull request opened by @colin99d on 2023-02-07 02:29_

Ref: #970

Charlie, one thing I could use help with. I would like the error line to be the line where continue is, and not the first line of the finalbody. I tried using the start of the token, but that results in incorrect values. Any advice? The lint still works if not, just might not highlight the line the user actually needs to fix.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/continue_in_finally.rs`:31 on 2023-02-07 02:33_

I think the way to get the right location would be to use `lexer::make_tokenizer_located(contents, range.location)`, and then use the `start` and `end` of `(start, tok, end`).

---

_@charliermarsh reviewed on 2023-02-07 02:33_

---

_@charliermarsh reviewed on 2023-02-07 02:34_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/continue_in_finally.rs`:31 on 2023-02-07 02:34_

That being said: I think we should instead do this with a visitor and the AST, rather than re-tokenzing. Take a look at how `ReturnStatementVisitor` works. We should try to find a `StmtKind::Continue` in `finalbody`.

---

_Renamed from "['pylint']: continue-in-finally" to "[`pylint`]: continue-in-finally" by @colin99d on 2023-02-07 02:53_

---

_Converted to draft by @colin99d on 2023-02-07 03:12_

---

_@colin99d reviewed on 2023-02-08 00:12_

---

_Review comment by @colin99d on `crates/ruff/src/rules/pylint/rules/continue_in_finally.rs`:31 on 2023-02-08 00:12_

I got this solution implemented!

---

_Comment by @ngnpope on 2023-02-08 22:25_

Out of interest, this rule is only really applicable to Python < 3.8.

See [here](https://docs.python.org/dev/whatsnew/3.8.html#other-language-changes) which has:

> - A [continue](https://docs.python.org/dev/reference/simple_stmts.html#continue) statement was illegal in the [finally](https://docs.python.org/dev/reference/compound_stmts.html#finally) clause due to a problem with the implementation. In Python 3.8 this restriction was lifted. (Contributed by Serhiy Storchaka in [bpo-32489](https://bugs.python.org/issue?@action=redirect&bpo=32489).)

With Python 3.7 reaching end-of-life June 2023, this is almost not worth implementing. But if we do it should be restricted to Python < 3.8.

---

_Comment by @charliermarsh on 2023-02-08 22:26_

I should've noticed this earlier, but is this the same as `B012`?

---

_Comment by @ngnpope on 2023-02-08 22:35_

> I should've noticed this earlier, but is this the same as `B012`?

Not really, no. The reasons are different.

For `flake8-bugbear` we have:

> **B012:** Use of `break`, `continue` or `return` inside `finally` blocks will silence exceptions or override return values from the `try` or `except` blocks. To silence an exception, do it explicitly in the `except` block. To properly use a `break`, `continue` or `return` refactor your code so these statements are not in the `finally` block.

For `pylint` we have:

> Emitted when the `continue` keyword is found inside a `finally` clause, which is a `SyntaxError`.

So it's just about whether it's worth supporting this rule for the next ~5 months as per [my comment](https://github.com/charliermarsh/ruff/pull/2618#issuecomment-1423320025).

---

_Marked ready for review by @colin99d on 2023-02-08 23:59_

---

_Comment by @colin99d on 2023-02-09 00:00_

@charliermarsh let me know if you want me to add a pyversion38 checker? Also if you want to close it, that is totally fine as well.

---

_Comment by @charliermarsh on 2023-02-09 00:20_

Lets nix for now just because it's confusingly similar to the other rule, even if it exists for a separate reason.

---

_Closed by @charliermarsh on 2023-02-09 00:20_

---
