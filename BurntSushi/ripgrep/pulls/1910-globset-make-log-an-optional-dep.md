```yaml
number: 1910
title: "globset: make 'log' an optional dep"
type: pull_request
state: merged
author: SergioBenitez
labels: []
assignees: []
merged: true
base: master
head: globset-optional-log
created_at: 2021-06-24T04:36:52Z
updated_at: 2022-06-10T20:00:21Z
url: https://github.com/BurntSushi/ripgrep/pull/1910
synced_at: 2026-01-12T18:23:14Z
```

# globset: make 'log' an optional dep

---

_@SergioBenitez_

As in the title. Would be nice to avoid the dependency (and the transitive dependency on `cfg-if`) when `log` isn't being used. Made it a default so logs remain when updating the crate.

As an aside, does the `simd-accel` feature do anything in `globset`?

---

_Renamed from "ignore: make 'log' an optional dep" to "globset: make 'log' an optional dep" by @SergioBenitez on 2021-06-24 09:23_

---

_Comment by @BurntSushi on 2021-06-24 11:56_

I'm not so sure about this. `globset` isn't exactly a dependency-slim crate itself. `log` is itself small and ubiquitous. Where is `globset` being used that `log` isn't?

As a downside, this infects every logging statement to the point of requiring conditional compilation. We could define our own logging macros to paper over this, but the juice really doesn't seem worth the squeeze given the number of other deps that `globset` already has.

> As an aside, does the `simd-accel` feature do anything in `globset`?

No. It used to. I probably should have added a note saying that it was left behind for backwards compatibility. `globset` hasn't had a semver bump in quite some time.

---

_Comment by @SergioBenitez on 2021-06-24 11:59_

> I'm not so sure about this. `globset` isn't exactly a dependency-slim crate itself. `log` is itself small and ubiquitous. Where is `globset` being used that `log` isn't?

In an application/library (the latter to resolve #1909) that I'm working on, of course.

> As a downside, this infects every logging statement to the point of requiring conditional compilation. We could define our own logging macros to paper over this, but the juice really doesn't seem worth the squeeze given the number of other deps that `globset` already has.

It's true, but it doesn't seem like log statements have seen much action for a while, so I can't imagine this will be much of a maintenance burden. Happy to add a macro that hides the conditional compilation, however.

---

_Comment by @SergioBenitez on 2022-06-05 01:22_

> Happy to add a macro that hides the conditional compilation, however.

Just did this. I really want to see this merged. I see zero downside with a 25% reduction in the number of deps.

---

_Review comment by @BurntSushi on `crates/globset/src/lib.rs`:129 on 2022-06-05 01:37_

Can we just call this `debug!`?

---

_@BurntSushi requested changes on 2022-06-05 01:37_

LGTM after a possible name change and a `rustfmt` run.

---

_Comment by @SergioBenitez on 2022-06-10 17:57_

Done!

---

_@BurntSushi approved on 2022-06-10 18:09_

---

_Merged by @BurntSushi on 2022-06-10 18:10_

---

_Closed by @BurntSushi on 2022-06-10 18:10_

---

_Comment by @BurntSushi on 2022-06-10 18:11_

This PR is in `globset 0.4.9` on crates.io.

---

_Comment by @SergioBenitez on 2022-06-10 20:00_

> This PR is in `globset 0.4.9` on crates.io.

Thank you!

---
