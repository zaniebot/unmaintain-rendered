```yaml
number: 1710
title: Automatically remove duplicate dictionary keys
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/duplicate-keys
created_at: 2023-01-07T05:14:35Z
updated_at: 2023-01-07T21:16:43Z
url: https://github.com/astral-sh/ruff/pull/1710
synced_at: 2026-01-12T05:36:32Z
```

# Automatically remove duplicate dictionary keys

---

_Pull request opened by @charliermarsh on 2023-01-07 05:14_

For now, to be safe, we're only removing keys with duplicate _values_.

See: #1647.


---

_Comment by @andersk on 2023-01-07 05:18_

This is incorrect:

```diff
-d = {"b": 2, "a": 1, "b": 2}
+d = {"b": 2, "a": 2}
```

---

_@charliermarsh reviewed on 2023-01-07 05:29_

---

_Review comment by @charliermarsh on `src/pyflakes/plugins/repeated_keys.rs`:55 on 2023-01-07 05:29_

I think this can maybe be reduced to a single case? But it's 12:30 so I'm gonna revisit in the morning.

---

_@andersk reviewed on 2023-01-07 05:32_

---

_Review comment by @andersk on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 05:32_

This quadratic loop could be slow on a large dictionary. A `HashMap` would avoid that problem.

---

_@charliermarsh reviewed on 2023-01-07 05:36_

---

_Review comment by @charliermarsh on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 05:36_

Nice idea.

---

_@charliermarsh reviewed on 2023-01-07 05:37_

---

_Review comment by @charliermarsh on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 05:37_

Literally a leetcode problem and I failed it!

---

_Review comment by @charliermarsh on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 16:47_

@andersk - Do you think it's important to avoid removing duplicate keys with non-identical values?

---

_@charliermarsh reviewed on 2023-01-07 16:47_

---

_@charliermarsh reviewed on 2023-01-07 17:17_

---

_Review comment by @charliermarsh on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 17:17_

If we do, it's hard to avoid quadratic time in pathological cases because values aren't hashable. As an alternative, I could stringify them.

---

_Review comment by @andersk on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 19:44_

We can make a wrapper type `struct ComparableExpr<'a>(&'a Expr)` that has custom `impl`s of `Eq` and `Hashable`.

---

_@andersk reviewed on 2023-01-07 19:44_

---

_Review comment by @charliermarsh on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 20:08_

I did start on that, but `f64` itself isn't hashable.

---

_@charliermarsh reviewed on 2023-01-07 20:08_

---

_@andersk reviewed on 2023-01-07 20:13_

---

_Review comment by @andersk on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 20:13_

You can hash via [`f64::to_bits`](https://doc.rust-lang.org/std/primitive.f64.html#method.to_bits).

---

_@charliermarsh reviewed on 2023-01-07 20:14_

---

_Review comment by @charliermarsh on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 20:14_

Mmm ok, cool. I'll try that out. (Above, you're suggesting that `ComparableExpr` compares by ignoring the `Location` fields, right?)

---

_@andersk reviewed on 2023-01-07 20:16_

---

_Review comment by @andersk on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 20:16_

Right.

---

_Review comment by @charliermarsh on `src/pyflakes/plugins/repeated_keys.rs`:33 on 2023-01-07 21:15_

This will come in a separate PR.

---

_@charliermarsh reviewed on 2023-01-07 21:15_

---

_Merged by @charliermarsh on 2023-01-07 21:16_

---

_Closed by @charliermarsh on 2023-01-07 21:16_

---

_Branch deleted on 2023-01-07 21:16_

---
