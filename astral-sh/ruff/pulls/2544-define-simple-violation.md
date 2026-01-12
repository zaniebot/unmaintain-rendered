```yaml
number: 2544
title: define_simple_violation
type: pull_request
state: closed
author: akx
labels: []
assignees: []
draft: true
base: main
head: define-simple-violation
created_at: 2023-02-03T14:46:53Z
updated_at: 2023-02-15T20:18:16Z
url: https://github.com/astral-sh/ruff/pull/2544
synced_at: 2026-01-12T15:55:08Z
```

# define_simple_violation

---

_@akx_

This PR defines a pair of new macros, `define_simple_violation!`, `define_simple_autofix_validation!` that define an unit struct and derive the `Violation`/`AlwaysAutofixableViolation` traits.

Since this hasn't been really discussed or anything, just a pattern that repeated throughout the Great Violation Migration, I'm totally fine with this being nixed, e.g. on the grounds of avoiding [Tim Toady](https://en.wikipedia.org/wiki/There%27s_more_than_one_way_to_do_it). (Even if the PR looks like it could've been a lot of work, it honestly wasn't – the majority of the half-hour here was spent tweaking the regexp I used to find the current simple-enough violations 😁 )


---

_Comment by @not-my-profile on 2023-02-03 14:55_

I don't think think we want to introduce such a declarative macro.

I think a procedural derive macro inspired by [thiserror](https://crates.io/crates/thiserror) would be much preferable, since it would also work with violations that have fields. E.g.

```rs
#[derive(Violation)]
#[message("Line too long ({length} > {limit} characters)")]
pub struct LineTooLong {
    pub length: usize,
    pub limit: usize,
}
```

---

_Comment by @charliermarsh on 2023-02-03 21:28_

That procedural macro API looks very nice...

---

_Comment by @akx on 2023-02-04 11:28_

I'll see what I can do with proc macros..!

---

_Converted to draft by @akx on 2023-02-04 11:29_

---

_Comment by @akx on 2023-02-15 20:17_

Ah, I'm not sure I have the bandwidth to look at proc macros right now, so I'll close this to keep it from littering the PR list :)

---

_Closed by @akx on 2023-02-15 20:17_

---

_Comment by @charliermarsh on 2023-02-15 20:18_

All good, thanks for following up :)

---
