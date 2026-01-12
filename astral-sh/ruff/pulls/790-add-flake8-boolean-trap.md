```yaml
number: 790
title: Add flake8-boolean-trap
type: pull_request
state: merged
author: pwoolvett
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2022-11-17T16:28:51Z
updated_at: 2022-11-18T17:30:08Z
url: https://github.com/astral-sh/ruff/pull/790
synced_at: 2026-01-12T15:55:05Z
```

# Add flake8-boolean-trap

---

_@pwoolvett_

Disclaimer: I have no rust experience whatsoever.


---

_Comment by @charliermarsh on 2022-11-17 17:10_

Nice! Impressive work especially if you don't have any Rust experience. Will give this a full review shortly.


---

_Review comment by @Stranger6667 on `src/flake8_boolean_trap/plugins.rs`:44 on 2022-11-17 22:05_

Would `.contains(&hint)` work here instead?

---

_Review comment by @Stranger6667 on `src/flake8_boolean_trap/plugins.rs`:44 on 2022-11-17 22:06_

Though, this would be faster

```rust
&hint == "bool" || &hint == "'bool'"
```

---

_Review comment by @Stranger6667 on `src/flake8_boolean_trap/plugins.rs`:44 on 2022-11-17 22:11_

See the comment below - maybe this approach is not needed

---

_Review comment by @Stranger6667 on `src/flake8_boolean_trap/plugins.rs`:34 on 2022-11-17 22:13_

maybe `to_string` could be avoided altogether. E.g. `hint` could be a boolean that stores whether the annotation is `bool` / `"bool"`.

Here we can compare `&id` to `"bool"` and in the "Constant" branch we can match on `Constant::Str` similarly.

---

_@Stranger6667 reviewed on 2022-11-17 22:13_

---

_Review comment by @charliermarsh on `src/flake8_boolean_trap/plugins.rs`:8 on 2022-11-17 22:20_

Nit: Should be able to use `arg: &Expr` here (and elsewhere in this file).

---

_Review comment by @charliermarsh on `src/flake8_boolean_trap/plugins.rs`:54 on 2022-11-17 22:21_

Do you need to check `kw_defaults` here too?

---

_Review comment by @charliermarsh on `src/flake8_boolean_trap/plugins.rs`:34 on 2022-11-17 22:24_

Yeah that'd be preferable, I think.

Like:
```rust
let hint = match &expr.node {
  ExprKind::Name { id, .. } => id == "bool",
  ExprKind::Constant { value, .. } => ..
  _ => false,
};
```

---

_@charliermarsh reviewed on 2022-11-17 22:24_

Generally looks good! A couple small comments.

---

_@pwoolvett reviewed on 2022-11-18 12:24_

---

_Review comment by @pwoolvett on `src/flake8_boolean_trap/plugins.rs`:54 on 2022-11-18 12:24_

kw_defaults are fine as booleans (in fact that would be a valid ast fix in an upcoming release), as they do not allow positional calls. MWE:

```py
def fun(a, *, b=True):
  ...
fun(4, True) # raises TypeError
```

---

_@pwoolvett reviewed on 2022-11-18 13:22_

---

_Review comment by @pwoolvett on `src/flake8_boolean_trap/plugins.rs`:34 on 2022-11-18 13:22_

corrected, please advise (tests passing, no lint suggestions)

---

_@pwoolvett reviewed on 2022-11-18 13:23_

---

_Review comment by @pwoolvett on `src/flake8_boolean_trap/plugins.rs`:44 on 2022-11-18 13:23_

adressed with charlie's suggestion

---

_@Stranger6667 reviewed on 2022-11-18 13:39_

---

_Review comment by @Stranger6667 on `src/flake8_boolean_trap/plugins.rs`:26 on 2022-11-18 13:39_

This line seems outdated

---

_Review comment by @pwoolvett on `src/flake8_boolean_trap/plugins.rs`:26 on 2022-11-18 13:41_

you mean just the comment, right?

---

_@pwoolvett reviewed on 2022-11-18 13:41_

---

_@Stranger6667 reviewed on 2022-11-18 13:42_

---

_Review comment by @Stranger6667 on `src/flake8_boolean_trap/plugins.rs`:26 on 2022-11-18 13:42_

Yes, I guess it could be removed :)

---

_@pwoolvett reviewed on 2022-11-18 13:44_

---

_Review comment by @pwoolvett on `src/flake8_boolean_trap/plugins.rs`:26 on 2022-11-18 13:44_

done


---

_Comment by @charliermarsh on 2022-11-18 15:11_

Looks great! I'll merge this later today.

---

_Merged by @charliermarsh on 2022-11-18 17:30_

---

_Closed by @charliermarsh on 2022-11-18 17:30_

---
