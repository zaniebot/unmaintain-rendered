```yaml
number: 686
title: "feat: no unnecessary encode utf8"
type: pull_request
state: merged
author: martinlehoux
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2022-11-11T18:45:56Z
updated_at: 2022-11-12T16:54:56Z
url: https://github.com/astral-sh/ruff/pull/686
synced_at: 2026-01-12T05:48:45Z
```

# feat: no unnecessary encode utf8

---

_Pull request opened by @martinlehoux on 2022-11-11 18:45_

- Mentions https://github.com/charliermarsh/ruff/issues/641
- I have a doubt about replacing multiline strings like in the test



---

_Review comment by @andersk on `resources/test/fixtures/U012.py`:6 on 2022-11-11 18:55_

pyupgrade does not rewrite this: [rationale](https://github.com/asottile/pyupgrade/issues/138#issuecomment-496044965), [source](https://github.com/asottile/pyupgrade/blob/v3.2.0/pyupgrade/_plugins/default_encoding.py), [tests](https://github.com/asottile/pyupgrade/blob/v3.2.0/tests/features/default_encoding_test.py).

---

_Review comment by @andersk on `resources/test/fixtures/U012.py`:13 on 2022-11-11 18:56_

pyupgrade would preserve the triple quotes with `b"""…"""`, which seems nicer.

---

_Review comment by @andersk on `resources/test/fixtures/U012.py`:18 on 2022-11-11 18:57_

pyupgrade would preserve the raw string with `br"fo\o"`, which seems nicer.

---

_@andersk reviewed on 2022-11-11 18:57_

---

_Review comment by @martinlehoux on `resources/test/fixtures/U012.py`:6 on 2022-11-11 19:54_

ok i misunderstood this one

---

_@martinlehoux reviewed on 2022-11-11 19:54_

---

_@martinlehoux reviewed on 2022-11-11 20:19_

---

_Review comment by @martinlehoux on `resources/test/fixtures/U012.py`:18 on 2022-11-11 20:19_

I don't really know how to achieve this, because what the AST gives me is already a "bytes" version of the string.
e.g., if I run this code on a `r"fo\o"`, which should replace it by itself
```rust
generator.unparse_expr(constant, 0)
if let Ok(content) = generator.generate() {
    check.amend(Fix::replacement(
        content,
        expr.location,
        expr.end_location.unwrap(),
    ))
}
```
I get `'fo\\o'`

---

_@charliermarsh reviewed on 2022-11-11 20:45_

---

_Review comment by @charliermarsh on `resources/test/fixtures/U012.py`:18 on 2022-11-11 20:45_

We may have to use LibCST to achieve that, since it looks like RustPython evaluates the raw string.

---

_@andersk reviewed on 2022-11-11 20:45_

---

_Review comment by @andersk on `resources/test/fixtures/U012.py`:18 on 2022-11-11 20:45_

I think this can be achieved with a similar method as W605?

https://github.com/charliermarsh/ruff/blob/bf7bf7aa17b7c864268d8ada1559f0ffe297427c/src/pycodestyle/checks.rs#L266-L272

Edit: Yeah, or LibCST.

---

_@charliermarsh reviewed on 2022-11-11 20:47_

---

_Review comment by @charliermarsh on `resources/test/fixtures/U012.py`:18 on 2022-11-11 20:47_

Oh yeah, that could work too, and might be easier?


---

_@martinlehoux reviewed on 2022-11-11 21:13_

---

_Review comment by @martinlehoux on `resources/test/fixtures/U012.py`:18 on 2022-11-11 21:13_

Yeah it works great, thanks!

---

_@martinlehoux reviewed on 2022-11-11 21:14_

---

_Review comment by @martinlehoux on `resources/test/fixtures/U012.py`:13 on 2022-11-11 21:14_

Fixed with below comment

---

_Comment by @martinlehoux on 2022-11-11 21:43_

@charliermarsh do you have any comment on the code, name or message of the error ?
I also had to trouble to understand what "regenerating files" meant

---

_Comment by @charliermarsh on 2022-11-11 21:50_

@martinlehoux - I think the name and error message etc. all look good! Code looks good too! Gonna spend a bit of time with it before merging to make sure I understand all the cases.

By "regenerating files", I mean running these two commands from the root directory:

- `cargo dev generate-rules-table`
- `cargo dev generate-check-code-prefix`

...and then checking in the changed files.


---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_encode_utf8.rs`:84 on 2022-11-11 21:54_

Can you add a comment to each of these branches to make it clear which cases they're handling? E.g.:

```
// Ex) `"foo".encode("...")`
if literal.is_ascii() {
  ...
```

...or whatever's appropriate.

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1055 on 2022-11-11 21:56_

I think this is missing an argument (`locator`), but maybe it should just be omitted, and then you can access `checker.locator` in `unnecessary_encode_utf8`?

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_encode_utf8.rs`:56 on 2022-11-11 21:57_

Is there any case where we'd have a `u`, but it's not the first character? Like `ru"""abc"""`? (Separately, isn't `U` also a valid prefix?)

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_encode_utf8.rs`:83 on 2022-11-11 21:58_

Should we be verifying that `args.len() == 1`, not just that it has at least one argument?

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_encode_utf8.rs`:11 on 2022-11-11 21:58_

Can you run Clippy, and fix any warnings?

E.g.:

```
 cargo +nightly clippy --all
    Checking ruff v0.0.108 (/Users/crmarsh/workspace/ruff)
warning: constants have by default a `'static` lifetime
  --> src/pyupgrade/plugins/unnecessary_encode_utf8.rs:11:23
   |
11 | const UTF8_LITERALS: &'static [&'static str] = &["utf-8", "utf8", "utf_8", "u8", "utf", "cp65001"];
   |                      -^^^^^^^--------------- help: consider removing `'static`: `&[&'static str]`
   |
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#redundant_static_lifetimes
   = note: `#[warn(clippy::redundant_static_lifetimes)]` on by default
```

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_encode_utf8.rs`:13 on 2022-11-11 21:59_

I think this can just be `Option<&'a Expr>`.

---

_@charliermarsh reviewed on 2022-11-11 21:59_

---

_Comment by @charliermarsh on 2022-11-11 22:00_

Thanks for this! Let me know what you're up for addressing, or if any of the comments feel "out of scope".

---

_Review comment by @martinlehoux on `src/check_ast.rs`:1055 on 2022-11-12 01:48_

yep i forgot to commit this part alongside

---

_@martinlehoux reviewed on 2022-11-12 01:48_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1055 on 2022-11-12 01:55_

All good!

---

_@charliermarsh reviewed on 2022-11-12 01:55_

---

_Review comment by @martinlehoux on `src/pyupgrade/plugins/unnecessary_encode_utf8.rs`:56 on 2022-11-12 09:41_

I don't know for sure, but it doesn't seem so. Is there an easy ay to be sure about it

---

_@martinlehoux reviewed on 2022-11-12 09:41_

---

_Comment by @martinlehoux on 2022-11-12 14:14_

@charliermarsh Thanks for the review, I fixed whatever I could and tried to make the code clearer

Don't hesitate if you need me to do something else

---

_Comment by @charliermarsh on 2022-11-12 14:17_

@martinlehoux - Awesome, thank you! I'll try to merge today :)

---

_Merged by @charliermarsh on 2022-11-12 16:54_

---

_Closed by @charliermarsh on 2022-11-12 16:54_

---

_Comment by @charliermarsh on 2022-11-12 16:54_

Thanks @martinlehoux!

---
