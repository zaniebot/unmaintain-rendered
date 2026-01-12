```yaml
number: 1607
title: Add isort.order-by-type boolean setting
type: pull_request
state: merged
author: mattoberle
labels: []
assignees: []
merged: true
base: main
head: isort/order-by-type
created_at: 2023-01-03T22:08:39Z
updated_at: 2023-01-03T23:08:03Z
url: https://github.com/astral-sh/ruff/pull/1607
synced_at: 2026-01-12T05:36:32Z
```

# Add isort.order-by-type boolean setting

---

_Pull request opened by @mattoberle on 2023-01-03 22:08_

The default `isort` behavior is `order-by-type = true`. When imports are ordered by type:

1. `CONSTANT_VARIABLES` are first.
2. `CamelCaseClasses` are second.
3. `everything_else` is third.

- https://pycqa.github.io/isort/docs/configuration/options.html#order-by-type

When `order-by-type = false` imports are ordered alphabetically (case-insensitive).

eg.

`order-by-type = false`

```py
import BAR
import bar
import FOO
import foo
import StringIO
from module import Apple, BASIC, Class, CONSTANT, function
```

`order-by-type = true`

```py
import BAR
import bar
import FOO
import foo
import StringIO
from module import BASIC, CONSTANT, Apple, Class, function
```

---

This PR is what I could hobble together with my limited `rust`.
In particular, I couldn't get a default option value of `true` without `impl Default for Settings` and wasn't sure the best way to get `member_key` aware of the setting without quite a bit of argument passing.

Open to feedback on anything in the PR and if this is an `isort` feature that would have a home in `ruff`!

---

_Review comment by @charliermarsh on `src/isort/settings.rs`:114 on 2023-01-03 22:11_

I think this should be `options.order_by_type.unwrap_or(true)`. As-is, it will default to `false`.

---

_Review comment by @charliermarsh on `src/isort/sorting.rs`:22 on 2023-01-03 22:12_

Hmm, maybe return `(Option<Prefix>, ...)`, and then `if !order_by_type { None } else if name.len() > 1 && string::is_upper(name) { Some(Prefix::Constants) } else if ...`?

---

_@charliermarsh reviewed on 2023-01-03 22:12_

Seems reasonable to me! You'll need to run `cargo +nightly dev generate-all` to update the JSON schema, README, etc.

Thanks for getting involved!

---

_@mattoberle reviewed on 2023-01-03 22:26_

---

_Review comment by @mattoberle on `src/isort/sorting.rs`:22 on 2023-01-03 22:26_

Oops, it looks like there my branch was out-of-date, sorry about that!
Just rebased- `member_key` is gone and the responsibilities are split between `cmp_members` and `prefix`.

---

_@mattoberle reviewed on 2023-01-03 22:39_

---

_Review comment by @mattoberle on `src/isort/sorting.rs`:47 on 2023-01-03 22:39_

Maybe a bit weird that `order_by_type` essentially turns this into a different existing function.
Is it better to select which function is used somewhere in `mod.rs`?

---

_Review comment by @charliermarsh on `src/isort/sorting.rs`:47 on 2023-01-03 23:02_

I think this is ok. The current setup is a bit confusing as-is anyway (mostly just the nomenclature isn't that clear), but I like that this at least encapsulates the member sorting in one place.

---

_@charliermarsh reviewed on 2023-01-03 23:02_

---

_@charliermarsh reviewed on 2023-01-03 23:03_

---

_Review comment by @charliermarsh on `src/isort/sorting.rs`:22 on 2023-01-03 23:03_

All good, I should've noticed too, I wrote that newer code 😂 

---

_Merged by @charliermarsh on 2023-01-03 23:07_

---

_Closed by @charliermarsh on 2023-01-03 23:07_

---

_Comment by @charliermarsh on 2023-01-03 23:08_

Thanks for this! It's much appreciated. Hope you'll consider sticking around :)

---
