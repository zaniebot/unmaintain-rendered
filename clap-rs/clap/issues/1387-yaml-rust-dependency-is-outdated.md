```yaml
number: 1387
title: yaml-rust dependency is outdated
type: issue
state: closed
author: kpcyrd
labels: []
assignees: []
created_at: 2018-11-23T11:22:44Z
updated_at: 2020-02-02T02:12:10Z
url: https://github.com/clap-rs/clap/issues/1387
synced_at: 2026-01-12T16:14:10Z
```

# yaml-rust dependency is outdated

---

_@kpcyrd_

### Affected Version of clap
```
2.32.0
```

### Bug or Feature Request Summary

The latest version of yaml-rust is currently:
```
yaml-rust = "0.4.2"
```

The version clap is depending on is:
```
yaml-rust = { version = "0.3.5", optional = true }
```

An update would be appreciated!



---

_Comment by @hashmap on 2018-12-18 11:08_

```
$cargo audit
    Fetching advisory database from `https://github.com/RustSec/advisory-db.git`
      Loaded 17 security advisories (from /home/ubuntu/.cargo/advisory-db)
    Scanning Cargo.lock for vulnerabilities (311 crate dependencies)
error: Vulnerable crates found!

ID:      RUSTSEC-2018-0006
Crate:   yaml-rust
Version: 0.3.5
Date:    2018-09-17
URL:     https://github.com/chyh1990/yaml-rust/pull/109
Title:   Uncontrolled recursion leads to abort in deserialization
Solution: upgrade to: >= 0.4.1

error: 1 vulnerability found!
```


---

_Referenced in [clap-rs/clap#1396](../../clap-rs/clap/pulls/1396.md) on 2018-12-18 11:10_

---

_Referenced in [clap-rs/clap#1415](../../clap-rs/clap/pulls/1415.md) on 2019-02-17 00:43_

---

_Closed by @spacekookie on 2019-03-26 08:54_

---

_Comment by @ErichDonGubler on 2019-03-27 15:40_

Wait, why did this get reverted? :( Should the issue be reopened?

---

_Reopened by @spacekookie on 2019-03-28 10:21_

---

_Comment by @spacekookie on 2019-03-28 15:59_

Just to bring everybody up to speed. Yes, the PR that fixed this issue was reverted. The description explains in detail why (https://github.com/clap-rs/clap/pull/1439)

I have opened https://github.com/chyh1990/yaml-rust/issues/126 which would allow me to merge a backported patch that mitigates this vulnerability to `0.3.x`. Then they could publish a new patch version that we pin to :tada: 

---

_Comment by @ErichDonGubler on 2019-03-28 16:16_

Wonderful explanation! Thank you. :)

---

_Comment by @gedigi on 2019-05-07 07:51_

@spacekookie yaml-rust doesn't seem actively maintained, or at least I don't see your request getting any attention. Alternatives?

---

_Comment by @spacekookie on 2019-05-07 08:16_

I'd suggest we fork 0.3 and upload it to crates.io ourselves. I'll probably do that this afternoon.

Thanks for the reminder on this btw. I've been pretty swamped at work the last 2 months :weary: 

---

_Comment by @gedigi on 2019-05-07 15:51_

Thanks a lot. Is there anything I can do to help? I don’t have much experience dealing with crates.io.

---

_Referenced in [paritytech/substrate#2559](../../paritytech/substrate/issues/2559.md) on 2019-05-14 09:47_

---

_Comment by @CreepySkeleton on 2020-02-01 15:26_

Closing due to inactivity. Honestly, I don't think we should fix it unless we have the patch backported to `rust-yaml 0.3`

---

_Closed by @CreepySkeleton on 2020-02-01 15:26_

---

_Comment by @kpcyrd on 2020-02-01 15:50_

If yaml support is unmaintained, can you please drop the dependency as a whole?

---

_Comment by @CreepySkeleton on 2020-02-01 15:52_

How would you expect us to drop it from `2.33`? That would be a breaking change.

Also, [like I said](https://github.com/clap-rs/clap/issues/1569#issuecomment-540630305), there's no much point in this fix for `clap` anyway.

---

_Comment by @kpcyrd on 2020-02-01 16:14_

I didn't file the issue due to security issues but because we have to carry a patch for clap downstream: https://salsa.debian.org/rust-team/debcargo-conf/blob/master/src/clap/debian/patches/relax-dep-versions.patch

Closing the issue doesn't make the dependency less outdated.

---

_Label `T: duplicate` added by @pksunkara on 2020-02-01 18:32_

---

_Comment by @pksunkara on 2020-02-01 18:34_

We have a duplicate at #1569. Please follow there.

---

_Comment by @CreepySkeleton on 2020-02-02 02:12_

@kpcyrd for clarity, the reason we can't bump rust-yaml to 0.4 is because some types from this crate are part of clap's API. If we actually do the bump, that would be s breaking change for users relying on them.

We did bump in the new-coming clap 3.0 but clap 2.x is going to have to stay on rust-yaml 0.3.x

---
