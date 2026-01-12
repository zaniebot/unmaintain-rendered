```yaml
number: 3227
title: "[pylint] W0603: global-statement"
type: pull_request
state: merged
author: igozali
labels:
  - rule
assignees: []
merged: true
base: main
head: plw0603-global-statement
created_at: 2023-02-25T22:13:24Z
updated_at: 2023-02-27T15:58:43Z
url: https://github.com/astral-sh/ruff/pull/3227
synced_at: 2026-01-12T15:55:12Z
```

# [pylint] W0603: global-statement

---

_@igozali_

Implements pylint rule [W0603: global-statement](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/global-statement.html).

Currently checks for global statement usage in a few StmtKinds (as tested in the `pylint` `global-statement` test case [here](https://github.com/PyCQA/pylint/blob/b70d2abd7fabe9bfd735a30b593b9cd5eaa36194/tests/functional/g/globals.py)):

* Assign
* AugAssign
* ClassDef
* FunctionDef | AsyncFunctionDef
* Import
* ImportFrom
* Delete

Tracking issue is located here: https://github.com/charliermarsh/ruff/issues/970

Output comparison:

**pylint**
```
[11:53:57 | last: 1s] (  4) | ~/src/ruff
igozali@hostname $ pylint --version
pylint 2.16.2
astroid 2.14.2
Python 3.9.9 (main, Dec  9 2022, 10:29:03)
[Clang 14.0.0 (clang-1400.0.29.202)]

[11:54:20 | last: 0s] (  0) | ~/src/ruff
igozali@hostname $ pylint -d R,C,W,E --enable W0603 --score=n crates/ruff/resources/test/fixtures/pylint/global_statement.py
************* Module global_statement
crates/ruff/resources/test/fixtures/pylint/global_statement.py:14:4: W0603: Using the global statement (global-statement)
crates/ruff/resources/test/fixtures/pylint/global_statement.py:21:4: W0603: Using the global statement (global-statement)
crates/ruff/resources/test/fixtures/pylint/global_statement.py:27:4: W0603: Using the global statement (global-statement)
crates/ruff/resources/test/fixtures/pylint/global_statement.py:33:4: W0603: Using the global statement (global-statement)
crates/ruff/resources/test/fixtures/pylint/global_statement.py:40:4: W0603: Using the global statement (global-statement)
crates/ruff/resources/test/fixtures/pylint/global_statement.py:47:4: W0603: Using the global statement (global-statement)
crates/ruff/resources/test/fixtures/pylint/global_statement.py:57:4: W0603: Using the global statement (global-statement)
crates/ruff/resources/test/fixtures/pylint/global_statement.py:67:4: W0603: Using the global statement (global-statement)
```

**ruff**
```
[11:55:00 | last: 21s] (  2) | ~/src/ruff
igozali@hostname $ cargo run -p ruff_cli -- check crates/ruff/resources/test/fixtures/pylint/global_statement.py --no-cache --select PLW0603
    Finished dev [unoptimized + debuginfo] target(s) in 0.29s
     Running `target/debug/ruff check crates/ruff/resources/test/fixtures/pylint/global_statement.py --no-cache --select PLW0603`
crates/ruff/resources/test/fixtures/pylint/global_statement.py:14:5: PLW0603 Using the global statement to update `CONSTANT` is discouraged
crates/ruff/resources/test/fixtures/pylint/global_statement.py:21:5: PLW0603 Using the global statement to update `sys` is discouraged
crates/ruff/resources/test/fixtures/pylint/global_statement.py:27:5: PLW0603 Using the global statement to update `namedtuple` is discouraged
crates/ruff/resources/test/fixtures/pylint/global_statement.py:33:5: PLW0603 Using the global statement to update `CONSTANT` is discouraged
crates/ruff/resources/test/fixtures/pylint/global_statement.py:40:5: PLW0603 Using the global statement to update `CONSTANT` is discouraged
crates/ruff/resources/test/fixtures/pylint/global_statement.py:47:5: PLW0603 Using the global statement to update `CONSTANT` is discouraged
crates/ruff/resources/test/fixtures/pylint/global_statement.py:57:5: PLW0603 Using the global statement to update `FUNC` is discouraged
crates/ruff/resources/test/fixtures/pylint/global_statement.py:67:5: PLW0603 Using the global statement to update `CLASS` is discouraged
Found 8 errors.
```

TODOs:

* [x] base impl
* [x] handle all cases in `pylint`
* [x] add doc

---

_Comment by @igozali on 2023-02-25 22:17_

Wanted to get early feedback if possible 😄, but I think I'm still missing some violations in the original pylint test cases: https://github.com/PyCQA/pylint/blob/b04e690a429adb5a1b0f17b909d8a98149f22fea/tests/functional/g/globals.py

If the direction looks good, I'll do the import and del as well?

---

_@charliermarsh reviewed on 2023-02-25 22:43_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/global_statement.rs`:53 on 2023-02-25 22:43_

I think this can be a bit simpler. Above, I'd call `pylint::rules::global_statement(self, target, id)`, then make this `pub fn global_statement(checker: &mut Checker, expr: &Expr, name: &str)`, and then the body can just be:

```rust
checker.diagnostics.push(Diagnostic::new(
    GlobalStatement {
        name: name.to_string(),
    },
    Range::from_located(expr)
));
```

---

_@charliermarsh reviewed on 2023-02-25 22:45_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:1889 on 2023-02-25 22:45_

I think, instead, we should put this in the `StmtKind::Global { names } => { ... }` branch around line 341. That is: we should just trigger this rule when we declare the global.

---

_@igozali reviewed on 2023-02-25 23:34_

---

_Review comment by @igozali on `crates/ruff/src/checkers/ast.rs`:1889 on 2023-02-25 23:34_

If I'm understanding correctly, what you want is for the behavior to be "warn whenever any global variable is declared", which is different from pylint's [original definition of the W0603 rule](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/global-statement.html): 

> Used when you use the "global" statement to update a global variable

Want to confirm if that's ok?

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:1889 on 2023-02-26 03:32_

Ohh interesting, I totally missed that. Then I think you're on the right track here. The rule is maybe a little strange, since if you're not using `global` to update or otherwise mutate a variable, then the `global` is effectively useless anyway, right? Like why else define a `global`? But I generally prefer to maintain parity with upstream rules unless we have good reasons not to.

---

_@charliermarsh reviewed on 2023-02-26 03:32_

---

_@igozali reviewed on 2023-02-26 17:05_

---

_Review comment by @igozali on `crates/ruff/src/checkers/ast.rs`:1889 on 2023-02-26 17:05_

Haha yeah I agree (would've loved to use your approach too if possible since it's way simpler), the original pylint rule feels a bit weird already, also there are 3 separate warnings that the user can configure but regardless, the range reported in all of them seems to always point to the global statement 😕 

```
[09:01:35 | last: 0s] (  0) | /tmp
igozali@hostname $ bat main.py
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: main.py
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ def do_something():
   2   │     global a
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

[09:01:39 | last: 0s] (  0) | /tmp
igozali@hostname $ pylint -d C --score=n main.py
************* Module main
main.py:2:4: W0602: Using global for 'a' but no assignment is done (global-variable-not-assigned)
```

```
[09:01:54 | last: 3s] (  0) | /tmp
igozali@hostname $ bat main.py
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: main.py
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ def do_something():
   2   │     global a
   3   │     a = 5
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

[09:01:56 | last: 0s] (  0) | /tmp
igozali@hostname $ pylint -d C --score=n main.py
************* Module main
main.py:2:4: W0601: Global variable 'a' undefined at the module level (global-variable-undefined)
```

```
[09:02:20 | last: 3s] (  0) | /tmp
igozali@hostname $ bat main.py
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: main.py
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ a = 0
   2   │
   3   │ def do_something():
   4   │     global a
   5   │     a = 5
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

[09:02:22 | last: 0s] (  0) | /tmp
igozali@hostname $ pylint -d C --score=n main.py
************* Module main
main.py:4:4: W0603: Using the global statement (global-statement)
```

---

_Comment by @igozali on 2023-02-26 19:08_

Should be good for rereview now whenever convenient! 

EDIT: Also added output comparison in PR description

---

_Comment by @charliermarsh on 2023-02-26 19:10_

Great, thanks! Will try to get to this today.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-02-26 19:10_

---

_Label `rule` added by @charliermarsh on 2023-02-26 23:37_

---

_Merged by @charliermarsh on 2023-02-26 23:40_

---

_Closed by @charliermarsh on 2023-02-26 23:40_

---

_Comment by @charliermarsh on 2023-02-26 23:40_

Thank you, nice PR!

---

_Comment by @igozali on 2023-02-27 15:58_

Thanks for the guidance and maintaining an excellent linter!

---

_Branch deleted on 2023-02-27 15:58_

---
