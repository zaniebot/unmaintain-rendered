```yaml
number: 4123
title: "Track `Markers` via a PubGrub package variant"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/marker-pkg
created_at: 2024-06-07T02:13:48Z
updated_at: 2024-06-07T19:57:03Z
url: https://github.com/astral-sh/uv/pull/4123
synced_at: 2026-01-10T13:54:02Z
```

# Track `Markers` via a PubGrub package variant

---

_Pull request opened by @charliermarsh on 2024-06-07 02:13_

## Summary

This PR adds a lowering similar to that seen in https://github.com/astral-sh/uv/pull/3100, but this time, for markers. Like `PubGrubPackageInner::Extra`, we now have `PubGrubPackageInner::Marker`. The dependencies of the `Marker` are `PubGrubPackageInner::Package` with and without the marker.

As an example of why this is useful: assume we have `urllib3>=1.22.0` as a direct dependency. Later, we see `urllib3 ; python_version > '3.7'` as a transitive dependency. As-is, we might (for some reason) pick a very old version of `urllib3` to satisfy `urllib3 ; python_version > '3.7'`, then attempt to fetch its dependencies, which could even involve building a very old version of `urllib3 ; python_version > '3.7'`. Once we fetch the dependencies, we would see that `urllib3` at the same version is _also_ a dependency (because we tack it on). In the new scheme though, as soon as we "choose" the very old version of `urllib3 ; python_version > '3.7'`, we'd then see that `urllib3` (the base package) is also a dependency; so we see a conflict before we even fetch the dependencies of the old variant.

With this, I can successfully resolve the case in #4099.

Closes https://github.com/astral-sh/uv/issues/4099.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-07 02:13_

---

_Label `bug` added by @charliermarsh on 2024-06-07 02:13_

---

_Label `preview` added by @charliermarsh on 2024-06-07 02:13_

---

_@charliermarsh reviewed on 2024-06-07 02:14_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock_scenarios.rs`:730 on 2024-06-07 02:14_

There is still some weirdness here -- we might want to throw out the markers entirely when reporting? I know how to do that.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/package.rs`:193 on 2024-06-07 02:15_

There is admittedly some weirdness here. If we have both a marker and an extra, I'm using the `Extra` variant. The `Marker` variant is being used _only_ for bare packages with markers. In `get_dependencies`, if `Extra` has a marker field, we flatten it out to include the base package with and without markers.

---

_@charliermarsh reviewed on 2024-06-07 02:15_

---

_@charliermarsh reviewed on 2024-06-07 02:16_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1174 on 2024-06-07 02:16_

This is a bit strange. I _think_ it's necessary?

---

_Review requested from @konstin by @charliermarsh on 2024-06-07 02:25_

---

_@konstin reviewed on 2024-06-07 08:28_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1174 on 2024-06-07 08:28_

Would adding a dependency on the virtual marker package and having the virtual package then depend on the real package work to?

---

_@konstin approved on 2024-06-07 08:38_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/package.rs`:193 on 2024-06-07 18:01_

Hmmm yes that is odd. I think that was one of the reasons I ended up not going this route initially, because it seemed like we'd want an `ExtraMarker` variant too. But I guess the flattening in `get_dependencies` works around that?

I think I would find some comments about this, on the `PubGrabInner` variants, helpful.

---

_@BurntSushi approved on 2024-06-07 18:03_

Nice fix. Very subtle and clever.

. o O { I find myself wondering if we can somehow abstract the case analysis on `PubGrubPackageInner`. I find it somewhat difficult to keep it straight how to deal with and consider each case. }

---

_Comment by @charliermarsh on 2024-06-07 18:24_

@BurntSushi -- I agree. We have the same patterns reused in a few places too, around matching on package name and URL.

---

_@charliermarsh reviewed on 2024-06-07 18:33_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/package.rs`:193 on 2024-06-07 18:33_

Yeah, the flattening works around it.

---

_Merged by @charliermarsh on 2024-06-07 19:57_

---

_Closed by @charliermarsh on 2024-06-07 19:57_

---

_Branch deleted on 2024-06-07 19:57_

---
