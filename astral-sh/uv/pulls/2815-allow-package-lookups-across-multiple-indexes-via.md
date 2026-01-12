```yaml
number: 2815
title: Allow package lookups across multiple indexes via explicit opt-in
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/multi-index
created_at: 2024-04-03T19:43:50Z
updated_at: 2024-04-08T13:24:11Z
url: https://github.com/astral-sh/uv/pull/2815
synced_at: 2026-01-12T16:05:14Z
```

# Allow package lookups across multiple indexes via explicit opt-in

---

_@charliermarsh_

## Summary

This partially revives https://github.com/astral-sh/uv/pull/2135 (with some modifications) to enable users to opt-in to looking for packages across multiple indexes.

The behavior is such that, in version selection, we take _any_ compatible version from a "higher-priority" index over the compatible versions of a "lower-priority" index, even if that means we might accept an "older" version.

Closes https://github.com/astral-sh/uv/issues/2775.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-03 19:51_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-03 19:51_

---

_Marked ready for review by @charliermarsh on 2024-04-03 19:51_

---

_Label `enhancement` added by @charliermarsh on 2024-04-03 19:51_

---

_Label `compatibility` added by @charliermarsh on 2024-04-03 19:51_

---

_Comment by @atti92 on 2024-04-03 19:57_

It'd be nice to also have an env variable option like: `UV_INDEX_STRATEGY=first-match/unsafe-first-satisfy/unsafe-latest/etc...`

---

_Review comment by @zanieb on `crates/uv-types/src/build_options.rs`:232 on 2024-04-03 19:58_

Nit: "Flatten" sounds like we would merge the versions into a single ordered flat map instead of iterating over each index until exhausted.

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:395 on 2024-04-03 20:00_

Should we use a enum instead e.g. `--unsafe-index-strategy flatten` or `--index-strategy unsafe-merge`? so this is future-proof?

---

_@zanieb approved on 2024-04-03 20:01_

Nice work! I like it.

---

_@charliermarsh reviewed on 2024-04-03 20:16_

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:395 on 2024-04-03 20:16_

Yeah good call.

---

_@charliermarsh reviewed on 2024-04-03 20:16_

---

_Review comment by @charliermarsh on `crates/uv-types/src/build_options.rs`:232 on 2024-04-03 20:16_

Originally I had `AllMatches`

---

_@zanieb reviewed on 2024-04-03 20:26_

---

_Review comment by @zanieb on `crates/uv-types/src/build_options.rs`:222 on 2024-04-03 20:26_

Is a match just "package name" or "package name and version range"?

---

_@charliermarsh reviewed on 2024-04-03 20:29_

---

_Review comment by @charliermarsh on `crates/uv-types/src/build_options.rs`:222 on 2024-04-03 20:29_

Package name.

---

_@atti92 reviewed on 2024-04-03 20:36_

---

_Review comment by @atti92 on `crates/uv-types/src/build_options.rs`:232 on 2024-04-03 20:36_

doesn't `FirstCompatible` more match the behavior?

---

_@zanieb reviewed on 2024-04-03 20:54_

---

_Review comment by @zanieb on `crates/uv-types/src/build_options.rs`:232 on 2024-04-03 20:54_

Maybe

- `FirstName`
- `FirstCompatible`

What would we call an option that truly merged them then?

- `AllCompatible`?

Maybe

- `First`
- `Concatenated`
- `Merged` (not implemented)

idk

---

_@atti92 reviewed on 2024-04-03 21:01_

---

_Review comment by @atti92 on `crates/uv-types/src/build_options.rs`:232 on 2024-04-03 21:01_

> What would we call an option that truly merged them then?
- `Latest`
- `AllLatest`
- `MergedLatest`
I think merge only makes sense if you truly want the `latest` out of all indexes.

---

_@charliermarsh reviewed on 2024-04-03 22:12_

---

_Review comment by @charliermarsh on `crates/uv-types/src/build_options.rs`:232 on 2024-04-03 22:12_

I think `FirstCompatible` is good here.

---

_Merged by @charliermarsh on 2024-04-03 23:23_

---

_Closed by @charliermarsh on 2024-04-03 23:23_

---

_Branch deleted on 2024-04-03 23:23_

---

_Review comment by @BurntSushi on `crates/uv-types/src/build_options.rs`:231 on 2024-04-08 13:23_

It might be worth ~one sentence mentioning that this risks dependency confusion attacks so that the "unsafe" in the name is justified here. (I know the PEP link gives the full context, but I think a one sentence call out here is important.)

(I'm assuming this is user facing docs. If not, the PEP link is enough IMO.)

---

_@BurntSushi reviewed on 2024-04-08 13:24_

Love it! Nice work. :-)

---
