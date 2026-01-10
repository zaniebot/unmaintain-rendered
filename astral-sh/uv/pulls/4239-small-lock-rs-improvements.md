```yaml
number: 4239
title: "Small `lock.rs` improvements"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/lock-rs-improvements
created_at: 2024-06-11T17:42:19Z
updated_at: 2024-06-25T22:19:01Z
url: https://github.com/astral-sh/uv/pull/4239
synced_at: 2026-01-10T13:48:28Z
```

# Small `lock.rs` improvements

---

_Pull request opened by @konstin on 2024-06-11 17:42_

Small improvements i made reading through `lock.rs`.


---

_Review requested from @BurntSushi by @konstin on 2024-06-11 17:42_

---

_@charliermarsh reviewed on 2024-06-11 18:39_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:858 on 2024-06-11 18:39_

I don't know that ID is right here, I defer to @BurntSushi.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:858 on 2024-06-12 11:25_

Yeah I'm not sure that `SourceId` is right here either. If it's an ID, then what is it identifying? For `DistributionId`, it's identifying a `Distribution`. A `Source` isn't really identifying anything, it is the source itself.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:830 on 2024-06-12 11:26_

:+1: 

---

_@BurntSushi reviewed on 2024-06-12 11:26_

Everything looks good here to me except for the `Source -> SourceId` rename. Can you say more about that?

---

_@konstin reviewed on 2024-06-12 12:33_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:858 on 2024-06-12 12:33_

I'm looking for different because `Source` makes it sound like we're reading the origin of a distribution from it, but (if i'm reading the code correctly) this field merely exists to match distributions with different versions for the same package in `DistributionId`, it's more like a subdivider.

---

_@charliermarsh reviewed on 2024-06-25 12:30_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:858 on 2024-06-25 12:30_

Should we just leave it as `Source` right now so that we can merge without resolving?

---

_@konstin reviewed on 2024-06-25 22:09_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:858 on 2024-06-25 22:09_

Done

---

_Merged by @konstin on 2024-06-25 22:19_

---

_Closed by @konstin on 2024-06-25 22:19_

---

_Branch deleted on 2024-06-25 22:19_

---
