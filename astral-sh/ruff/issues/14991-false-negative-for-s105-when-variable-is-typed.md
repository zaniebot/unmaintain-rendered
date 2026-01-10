```yaml
number: 14991
title: False negative for S105 when variable is typed
type: issue
state: closed
author: zxdavb
labels:
  - good first issue
  - rule
  - accepted
assignees: []
created_at: 2024-12-15T19:09:58Z
updated_at: 2024-12-20T08:12:31Z
url: https://github.com/astral-sh/ruff/issues/14991
synced_at: 2026-01-10T11:09:56Z
```

# False negative for S105 when variable is typed

---

_Issue opened by @zxdavb on 2024-12-15 19:09_

I note that `ruff check` detects the following:
```python
TEST_PASSWORD = "P@ssw0rd!!"  # Possible hardcoded password assigned to: "TEST_PASSWORD"
```

... except when the appropriate `noqa` directive is applied:
```python
TEST_PASSWORD = "P@ssw0rd!!"  # noqa: S105
```

However, these also pass a `ruff check` without complaint:
```python
TEST_PASSWORD: str = "P@ssw0rd!!"
TEST_PASSWORD: Final = "P@ssw0rd!!"
```

... and, IMO, they should not.

Ruff 0.8.3
Python 3.13.0
Ubuntu 24.04.1 LTS on WSL

---

_Comment by @InSyncWithFoo on 2024-12-15 21:44_

This should require only one edit to [`statement.rs`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/checkers/ast/analyze/statement.rs) to add another call to `flake8_bandit::rules::assign_hardcoded_password_string` at the end of the `AnnAssign` branch.

An excellent [good first issue](../labels/good%20first%20issue) candidate.

---

_Comment by @zxdavb on 2024-12-15 22:52_

A bit like this, I guess:

```rust
        Stmt::AnnAssign(
            assign_stmt @ ast::StmtAnnAssign {
                target,
                value,
                annotation,
                ..
            },
        ) => {
...

            if checker.enabled(Rule::HardcodedPasswordString) {
                flake8_bandit::rules::assign_hardcoded_password_string(checker, value, targets);
            }
        }
        Stmt::TypeAlias(ast::StmtTypeAlias { name, .. }) => {
```


---

_Comment by @InSyncWithFoo on 2024-12-15 23:24_

That's correct, though that scope doesn't have `targets`, only `target`.

If you are submitting a PR, please update the tests too. See [the contribution guide](https://github.com/astral-sh/ruff/blob/d848182340079299f71cfdfb647e1ae2d00835b6/CONTRIBUTING.md#the-basics), then look for the `S105.py` file.

---

_Label `good first issue` added by @dylwil3 on 2024-12-16 04:18_

---

_Label `rule` added by @dylwil3 on 2024-12-16 04:18_

---

_Label `accepted` added by @dylwil3 on 2024-12-16 04:18_

---

_Comment by @zxdavb on 2024-12-18 13:18_

> That's correct, though that scope doesn't have `targets`, only `target`.

Sigh - I am pretty good with Python... 

For the benefit of others, so far I have:
```rust
            if let Some(value) = value {
                if checker.enabled(Rule::HardcodedPasswordString) {
                    flake8_bandit::rules::assign_hardcoded_password_string(
                        checker,
                        value,
                        std::slice::from_ref(target), // Expects an array?
                    );
                }
            }
```

... but I can't get it to compile!

---

_Comment by @MichaReiser on 2024-12-18 13:28_

What you have seems almost correct and impressive that you found `std::slice::from_ref`. I remember that I only learned about that method after using Rust for several months:


```rust
            if checker.enabled(Rule::HardcodedPasswordString) {
                if let Some(value) = value.as_deref() {
                    flake8_bandit::rules::assign_hardcoded_password_string(
                        checker,
                        value,
                        std::slice::from_ref(target),
                    );
                }
            }
```

---

_Comment by @tarasmatsyk on 2024-12-19 12:02_

Hey guys, thank you for an amazing linter! 

I tried to absorb all advices here and came up with a PR:
https://github.com/astral-sh/ruff/pull/15059

@zxdavb my apologies if you wanted to tackle this, I was checking open issues and figured this one has the most potential for a test PR :) 

---

_Comment by @InSyncWithFoo on 2024-12-19 17:15_

I think this can be closed now, as it has been resolved by #15059.

---

_Closed by @MichaReiser on 2024-12-20 08:12_

---
