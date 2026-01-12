```yaml
number: 13721
title: "Only show the first match per platform (and architecture) by default in `uv python list` "
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/filter-smarter
created_at: 2025-05-29T20:29:26Z
updated_at: 2025-05-29T21:38:46Z
url: https://github.com/astral-sh/uv/pull/13721
synced_at: 2026-01-12T16:10:49Z
```

# Only show the first match per platform (and architecture) by default in `uv python list` 

---

_@Gankra_

This is an alternative to #13701 

---

_Comment by @zanieb on 2025-05-29 20:39_

Hm... this presumes that all the install problems were fixed by https://github.com/astral-sh/uv/pull/13709 ?

I don't think this removes the need for #13701 because it won't solve the microarchitecture case?

---

_Comment by @Gankra on 2025-05-29 20:41_

Confirmed this fixes #13475 if i extend it to also do "x86_32 windows on arm64 windows"

(although in that case the snapshot churns in an annoying way)

```
    3       │-cpython-3.10.17-[PLATFORM]    <download available>
    4       │-pypy-3.10.16-[PLATFORM]       <download available>
    5       │-graalpy-3.10.0-[PLATFORM]     <download available>
          3 │+cpython-3.10.17-[PLATFORM]      <download available>
          4 │+pypy-3.10.16-[PLATFORM]      <download available>
          5 │+graalpy-3.10.0-[PLATFORM]    <download available>
 ```

---

_Comment by @Gankra on 2025-05-29 20:42_

Do you think it would be reasonable to add an --all-microarches flag? Or hide microarches behind --all-arches?

---

_Comment by @zanieb on 2025-05-29 20:49_

We'll need more sophisticated flags for sure, like `--all-arches` also doesn't cover the "I want to see all _usable_ architectures" use-case.

For microarchitectures, I think we still want to list the best one by default, the thing there will be that we'll want to change the ordering of microarchitectures based on some preference flag.

---

_Renamed from "only conditionally consider os/arch when filtering downloads" to "Only show the first match per platform (and architecture) by default in `uv python list` " by @zanieb on 2025-05-29 20:51_

---

_Comment by @Gankra on 2025-05-29 20:52_

> For microarchitectures, I think we still want to list the best one by default, the thing there will be that we'll want to change the ordering of microarchitectures based on some preference flag.

Yeah I think that's the crux of it -- we should regard all these download-preference issues as sorting problems, with the `install` mode selecting the first item in the list that matches the requirement, and `list` applying some "don't show more than one" logic.

And then whatever preferences the user applies to prefer v1 are settings that go into the sorter?

---

_Comment by @zanieb on 2025-05-29 20:54_

I find that fairly compelling.

---

_@zanieb approved on 2025-05-29 20:54_

---

_Merged by @Gankra on 2025-05-29 21:38_

---

_Closed by @Gankra on 2025-05-29 21:38_

---

_Branch deleted on 2025-05-29 21:38_

---
