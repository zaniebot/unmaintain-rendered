```yaml
number: 9359
title: Generate deterministic ids when formatting notebooks
type: pull_request
state: merged
author: zanieb
labels:
  - formatter
assignees: []
merged: true
base: main
head: zb/notebook-id-fmt
created_at: 2024-01-02T16:11:17Z
updated_at: 2024-01-04T15:19:02Z
url: https://github.com/astral-sh/ruff/pull/9359
synced_at: 2026-01-10T23:07:18Z
```

# Generate deterministic ids when formatting notebooks

---

_Pull request opened by @zanieb on 2024-01-02 16:11_

When formatting notebooks, we populate the `id` field for cells that do not have one. Previously, we generated a UUID v4 which resulted in non-deterministic formatting. Here, we generate the UUID from a seeded random number generator instead of using true randomness. For example, here are the first five ids it would generate:

```
7fb27b94-1602-401d-9154-2211134fc71a
acae54e3-7e7d-407b-bb7b-55eff062a284
9a63283c-baf0-4dbc-ab1f-6479b197f3a8
8dd0d809-2fe7-4a7c-9628-1538738b07e2
72eea511-9410-473a-a328-ad9291626812
```

We also add a check that an id is not present in another cell to prevent accidental introduction of duplicate ids.

The specification is lax, and we could just use incrementing integers e.g. `0`, `1`, ... but I have a minor preference for retaining the UUID format. Some discussion [here](https://github.com/astral-sh/ruff/pull/9359#discussion_r1439607121) — I'm happy to go either way though.

Discovered via #9293 

---

_@zanieb reviewed on 2024-01-02 16:12_

---

_Review comment by @zanieb on `crates/ruff_notebook/src/notebook.rs`:150 on 2024-01-02 16:12_

We should collect all of the ids beforehand, otherwise we could generate an id that is present in a later cell.

---

_@zanieb reviewed on 2024-01-02 16:12_

---

_Review comment by @zanieb on `crates/ruff_notebook/src/notebook.rs`:160 on 2024-01-02 16:12_

I'm not sure about using `from_u128` with an integer index vs seeding a random number generator and using `from_random_bytes`.

---

_@konstin reviewed on 2024-01-02 16:20_

---

_Review comment by @konstin on `crates/ruff_notebook/src/notebook.rs`:160 on 2024-01-02 16:20_

Why not use `id_index` directly? It's allowed in the schema

---

_Comment by @github-actions[bot] on 2024-01-02 16:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@zanieb reviewed on 2024-01-02 16:24_

---

_Review comment by @zanieb on `crates/ruff_notebook/src/notebook.rs`:160 on 2024-01-02 16:24_

Definitely an option. I guess it's a question of matching user expectations vs just being schema complaint. 

---

_@zanieb reviewed on 2024-01-02 17:49_

---

_Review comment by @zanieb on `crates/ruff_notebook/src/notebook.rs`:160 on 2024-01-02 17:49_

These generate UUIDs like

```
00000000-0000-0000-0000-000000000001
00000000-0000-0000-0000-000000000002
00000000-0000-0000-0000-000000000003
00000000-0000-0000-0000-000000000004
00000000-0000-0000-0000-000000000005
```

which I think is basically worthless — we might as well use the integer directly in that case.

Another option is to do use a mock random number generator e.g. `rand::rngs::mock::StepRng::new(0, 1)` which yields

```
00010203-0405-4607-8809-0a0b0c0d0e0f
10111213-1415-4617-9819-1a1b1c1d1e1f
20212223-2425-4627-a829-2a2b2c2d2e2f
30313233-3435-4637-b839-3a3b3c3d3e3f
40414243-4445-4647-8849-4a4b4c4d4e4f
```

---

_Marked ready for review by @zanieb on 2024-01-03 03:38_

---

_Review requested from @konstin by @zanieb on 2024-01-03 16:40_

---

_Review requested from @dhruvmanila by @zanieb on 2024-01-03 16:40_

---

_@charliermarsh reviewed on 2024-01-03 16:53_

---

_Review comment by @charliermarsh on `crates/ruff_notebook/src/notebook.rs`:152 on 2024-01-03 16:53_

Could we use a seeded number generator rather than the `StepRng` one, so that the IDs are deterministic but look random rather than structured as they do now? Or was this the only option for deterministic UUIDs?

---

_Review comment by @zanieb on `crates/ruff_notebook/src/notebook.rs`:152 on 2024-01-03 16:57_

I'm sure there's another option for a seeded number generator, this one was just the most obvious way I saw.

---

_@zanieb reviewed on 2024-01-03 16:57_

---

_@charliermarsh approved on 2024-01-03 17:00_

---

_Review comment by @zanieb on `crates/ruff_notebook/src/notebook.rs`:152 on 2024-01-03 17:02_

Sure we can use the `StdRng` seeded with `0`

```
7fb27b94-1602-401d-9154-2211134fc71a
acae54e3-7e7d-407b-bb7b-55eff062a284
9a63283c-baf0-4dbc-ab1f-6479b197f3a8
8dd0d809-2fe7-4a7c-9628-1538738b07e2
72eea511-9410-473a-a328-ad9291626812
```

---

_@zanieb reviewed on 2024-01-03 17:02_

---

_Label `formatter` added by @zanieb on 2024-01-03 17:11_

---

_@konstin approved on 2024-01-04 12:24_

I'd go with natural numbers for simplicity but it doesn't matter much since the users shouldn't see that id anyway.

---

_Merged by @zanieb on 2024-01-04 15:19_

---

_Closed by @zanieb on 2024-01-04 15:19_

---

_Branch deleted on 2024-01-04 15:19_

---
