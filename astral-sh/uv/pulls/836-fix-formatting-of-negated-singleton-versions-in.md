```yaml
number: 836
title: Fix formatting of negated singleton versions in error messages
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/fix-terms
created_at: 2024-01-08T18:07:01Z
updated_at: 2024-01-08T18:37:33Z
url: https://github.com/astral-sh/uv/pull/836
synced_at: 2026-01-12T16:04:13Z
```

# Fix formatting of negated singleton versions in error messages

---

_@zanieb_

Closes #805 
Requires https://github.com/zanieb/pubgrub/pull/17

---

_@zanieb reviewed on 2024-01-08 18:10_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:265 on 2024-01-08 18:10_

Here's the targeted fix

---

_@zanieb reviewed on 2024-01-08 18:10_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:1469 on 2024-01-08 18:10_

We also remove the spaces between package and ranges — this is consistent with all other rendering.

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/pubgrub/report.rs`:130 on 2024-01-08 18:27_

`(*t).clone()` is somewhat odd in safe code, because auto-deref will usually make `t.clone()` work. I don't know the type of `t` though to say whether `t.clone()` ought to work though.

---

_@BurntSushi approved on 2024-01-08 18:27_

w00t! Looks much better.

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:130 on 2024-01-08 18:30_

It's `&&t` for some reason...

```
error[E0308]: mismatched types
   --> crates/puffin-resolver/src/pubgrub/report.rs:130:75
    |
130 |                     .map(|(p, t)| format!("{p}{}", PubGrubTerm::from_term(t.clone())))
    |                                                    ---------------------- ^^^^^^^^^ expected `Term<Range<PubGrubVersion>>`, found `&Term<Range<PubGrubVersion>>`
    |                                                    |
    |                                                    arguments to this function are incorrect
    |
    = note:   expected enum `Term<pubgrub::range::Range<PubGrubVersion>>`
            found reference `&Term<pubgrub::range::Range<PubGrubVersion>>`
note: associated function defined here
   --> crates/puffin-resolver/src/pubgrub/report.rs:260:8
    |
260 |     fn from_term(term: Term<Range<PubGrubVersion>>) -> PubGrubTerm {
    |        ^^^^^^^^^ ---------------------------------
help: consider using clone here
    |
130 |                     .map(|(p, t)| format!("{p}{}", PubGrubTerm::from_term(t.clone().clone())))
    |                                                                                    ++++++++
```

---

_@zanieb reviewed on 2024-01-08 18:30_

---

_Label `error messages` added by @zanieb on 2024-01-08 18:32_

---

_Merged by @zanieb on 2024-01-08 18:33_

---

_Closed by @zanieb on 2024-01-08 18:33_

---

_Branch deleted on 2024-01-08 18:33_

---

_@BurntSushi reviewed on 2024-01-08 18:34_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/pubgrub/report.rs`:130 on 2024-01-08 18:34_

Oh, right, if `t` is a `&T` then `.clone()` will copy the `&T`. Nevermind me. Sometimes another way of writing it is `t.to_owned()`, but I'm not sure if that works here.

---

_@zanieb reviewed on 2024-01-08 18:37_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:130 on 2024-01-08 18:37_

That also doesn't work 😢 

---
