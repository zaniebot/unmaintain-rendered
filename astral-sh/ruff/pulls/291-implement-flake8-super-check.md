```yaml
number: 291
title: "Implement `flake8-super` check"
type: pull_request
state: merged
author: sobolevn
labels: []
assignees: []
merged: true
base: main
head: issue-273
created_at: 2022-09-30T09:12:29Z
updated_at: 2022-09-30T15:28:30Z
url: https://github.com/astral-sh/ruff/pull/291
synced_at: 2026-01-12T15:55:04Z
```

# Implement `flake8-super` check

---

_@sobolevn_

Here's my attempt at implementing this rule.

Some feedback.

Right now implementing autofix seems very hard. Why?
Because `Location` does not have `.end()` method.

We need to fix:

```python
super(
   SuperClass,
   self
).method()
```

How can we do that?

1. Get the second argument, assume it is `self`, take `4` as length: this will backfire in cases when non-`self` name is used
2. Convert the last argument to string to get its length? No idea how can we do that. Python has `ast.unparse` or `astor`
3. Manually go through different types of nodes and find their length: seems to complex

It would also be rather hard to find these cases:

```python
super(
   SuperClass,
   self)

# vs

super(
   SuperClass,
   self
)
```

This is my code for the fix that I've decided not to include into this PR.

```rust
           if matches!(autofix, fixer::Mode::Generate | fixer::Mode::Apply) {
                // It looks like this:
                // super(some, args).some_attr
                // ^               ^
                // start           end
                // It can also be multiline.
                let last_arg = args.last().unwrap();
                check.amend(Fix {
                    content: "super()".to_string(),
                    start: Location::new(expr.location.row(), expr.location.column()),
                    end: Location::new(  // TODO: find end location
                        last_arg.location.row(),
                        last_arg.location.column() + 2,
                    ),
                    applied: false,
                });
            }
```

I propose to use libcst instead for fixes. It just works.

Refs #273 


---

_Marked ready for review by @sobolevn on 2022-09-30 09:12_

---

_Comment by @sobolevn on 2022-09-30 09:39_

Reference inplementation: https://github.com/asottile/pyupgrade/blob/7f79d5a5d08ed8873fd047eedbca0d3637b8d07e/pyupgrade/_plugins/legacy.py#L129

---

_Comment by @charliermarsh on 2022-09-30 13:03_

Agreed. The fix API is too manual right now. Are you interested in exploring the use of LibCST for autofix specifically? (E.g., given an autofix snippet, parse it with LibCST and generate the patch.)

FWIW, the way I'd implement this right now is by iterating over the token stream starting with the class definition, looking for a left paren and then a balanced right paren. We do a similar thing for `class Foo(object)`, though that case is more complex given that `object` can be preceded or followed by other symbols.


---

_@charliermarsh reviewed on 2022-09-30 13:05_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:700 on 2022-09-30 13:05_

Should we validate that the current scope stack is function, then class?

---

_Comment by @sobolevn on 2022-09-30 13:07_

@charliermarsh I can, but I don't have reliable access to `rust` binary right now.
Since I am traveling right now and only have a machine from work. I cannot install `rust` here.

So, probably only after a week :)

---

_Comment by @charliermarsh on 2022-09-30 13:08_

@sobolevn - No worries. I may find time between now and then, but have a few other things to explore. If you get to it, just let me know :)


---

_@charliermarsh reviewed on 2022-09-30 13:08_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:700 on 2022-09-30 13:08_

Eh, maybe non-critical.

---

_Review comment by @sobolevn on `src/check_ast.rs`:700 on 2022-09-30 13:10_

This is debatable. `flake8-super` does not that. But, `pyugrade` does.

In my opinion, it is not required. Because `super` cannot be redefined due to `flake8-builtins` rule. I also don't know any real-life use-cases where `super` is calledd somewhere outside of methods.

So, probably it is up to you.

---

_@sobolevn reviewed on 2022-09-30 13:10_

---

_Merged by @charliermarsh on 2022-09-30 13:12_

---

_Closed by @charliermarsh on 2022-09-30 13:12_

---

_Comment by @sobolevn on 2022-09-30 13:15_

🎉 Thanks for review and merge!

---

_Comment by @charliermarsh on 2022-09-30 13:16_

Thank _you_! Your contributions have been 💯 and really appreciate your perspective on API etc.

---

_Comment by @charliermarsh on 2022-09-30 15:28_

Also relevant: I've started working on AST-to-source code generation in #292. It's based on a mix of some code from RustPython and Astor.


---
