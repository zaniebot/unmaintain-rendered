```yaml
number: 3345
title: require serde and rkyv everywhere; remove optional serde and rkyv features
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - rustlib
assignees: []
merged: true
base: main
head: ag/require-serde
created_at: 2024-05-03T13:26:10Z
updated_at: 2024-05-03T14:23:16Z
url: https://github.com/astral-sh/uv/pull/3345
synced_at: 2026-01-10T14:37:54Z
```

# require serde and rkyv everywhere; remove optional serde and rkyv features

---

_Pull request opened by @BurntSushi on 2024-05-03 13:26_

In *some* places in our crates, `serde` (and `rkyv`) are optional
dependencies. I believe this was done out of reasons of "good sense,"
that is, it follows a Rust ecosystem pattern where serde integration
tends to be an opt-in crate feature. (And similarly for `rkyv`.)

However, ultimately, `uv` itself requires `serde` and `rkyv` to
function. Since our crates are strictly internal, there are limited
consumers for our crates without `serde` (and `rkyv`) enabled. I think
one possibility is that optional `serde` (and `rkyv`) integration means
that someone can do this:

    cargo test -p pep440_rs

And this will run tests _without_ `serde` or `rkyv` enabled. That in
turn could lead to faster iteration time by reducing compile times. But,
I'm not sure this is worth supporting. The iterative compilation times of
individual crates are probably fast enough in debug mode, even with
`serde` and `rkyv` enabled. Namely, `serde` and `rkyv` themselves
shouldn't need to be re-compiled in most cases. On `main`:

```
from-scratch: `cargo test -p pep440_rs --lib` 0.685
incremental: `cargo test -p pep440_rs --lib` 0.278s
from-scratch: `cargo test -p pep440_rs --features serde,rkyv --lib` 3.948s
incremental: `cargo test -p pep440_rs --features serde,rkyv --lib` 0.321s
```

So while a from-scratch build does take significantly longer, an
incremental build is about the same.

The benefit of doing this change is two-fold:

1. It brings out crates into alignment with "reality." In particular,
   some crates were _implicitly_ relying on `serde` being enabled
   without explicitly declaring it. This technically means that our
   `Cargo.toml`s were wrong in some cases, but it is hard to observe it
   because of feature unification in a Cargo workspace.
2. We no longer need to deal with the cognitive burden of writing
   `#[cfg_attr(feature = "serde", ...)]` everywhere.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-05-03 13:26_

---

_Review requested from @konstin by @BurntSushi on 2024-05-03 13:26_

---

_Label `internal` added by @BurntSushi on 2024-05-03 13:29_

---

_@zanieb approved on 2024-05-03 13:31_

Yeah, this seems like the correct trade-off for us.

---

_@konstin approved on 2024-05-03 13:38_

---

_Label `rustlib` added by @zanieb on 2024-05-03 13:39_

---

_Comment by @zanieb on 2024-05-03 13:40_

This may affect people using our crates (like pixi) right?

---

_Comment by @BurntSushi on 2024-05-03 13:42_

> This may affect people using our crates (like pixi) right?

It could. But this doesn't remove anything. In the worst case, they will wind up compiling `serde` even if they don't need it. But I don't have a ton of insight there...

---

_Comment by @zanieb on 2024-05-03 13:44_

@BurntSushi won't they get an error that there is no feature "serde"? I've seen this when removing the optional in the past.

---

_Comment by @BurntSushi on 2024-05-03 13:48_

> @BurntSushi won't they get an error that there is no feature "serde"? I've seen this when removing the optional in the past.

Oh sure, if they are opting into serde, yes. But we aren't doing semver on these crates. And their fix is simple: just remove the opt-in.

---

_@charliermarsh approved on 2024-05-03 13:52_

I think if it were _just_ me I would leave it as-is -- it doesn't cause me much trouble, it's well-principled, and we may have to undo this in the future. But I understand the desire to remove, and "we may have to undo this in the future" is always a YAGNI red flag.

But please only merge if it's net-helpful for you, and not because I mentioned it in Discord :joy:

---

_Merged by @BurntSushi on 2024-05-03 14:21_

---

_Closed by @BurntSushi on 2024-05-03 14:21_

---

_Branch deleted on 2024-05-03 14:21_

---

_Comment by @BurntSushi on 2024-05-03 14:22_

> But please only merge if it's net-helpful for you, and not because I mentioned it in Discord 😂

Hah, yeah, it has been bothering me for a while.

I agree that it's possible we might have to undo this. But I think that will be part of publishing crates, right? (Maybe there's another use case I'm missing.) If so, I think there will be a fair bit of other work to do anyway in that case. An ecosystem crate and an internal crate are two different beasts.

---

_Comment by @charliermarsh on 2024-05-03 14:23_

Yup all good, I support it!

---
