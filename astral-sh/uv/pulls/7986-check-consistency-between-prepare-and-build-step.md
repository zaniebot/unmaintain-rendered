```yaml
number: 7986
title: " Check consistency between prepare and build step "
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend9-check-prepare-build-match
created_at: 2024-10-07T18:29:26Z
updated_at: 2024-10-08T16:38:23Z
url: https://github.com/astral-sh/uv/pull/7986
synced_at: 2026-01-12T16:08:07Z
```

#  Check consistency between prepare and build step 

---

_@konstin_

PEP 517 mandates that the metadata must be consistent between `prepare_metadata_for_build_wheel` and `build_wheel` by passing the directory written in the prepare step to the build step (https://peps.python.org/pep-0517/#build-wheel). There is no reason why we would violate this guarantee, but we are prudent and check that `METADATA` and `entry_points.txt` (the main metadata files) actually match.

---

_Label `preview` added by @konstin on 2024-10-07 18:29_

---

_Review requested from @BurntSushi by @konstin on 2024-10-07 18:29_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:368 on 2024-10-08 14:21_

Nit: I'd put the LHS in a local variable since this is a mouthful to fit in an `if`. And then tweak the names, e.g., `pyproject_metadata` and `direct_metadata` or some such.

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:391 on 2024-10-08 14:22_

If we expect this to actually happen, it would be more useful for the error to indicate where the mismatch is. But I'm fine with merging as-is and only adding that if it proves useful. (Since it's probably somewhat annoying to implement.)

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:409 on 2024-10-08 14:26_

How come there's asymmetry  between the `entry_points.txt` checks and `METADATA`? For `METADATA`, you check for content equivalence, and error if the file doesn't exist. But here, an error is returned if the file does exist.

OK, maybe writing that out helped me understand. It looks like creating entry points from `pyproject_toml` might return `None`, but that case doesn't exist for `to_metadata`?

Apologies for the meandering comment. :)

---

_@BurntSushi approved on 2024-10-08 14:26_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:409 on 2024-10-08 15:20_

I've added inline comments: METADATA is mandatory and always written, `entry_points.txt` is not written if it would be empty.

---

_@konstin reviewed on 2024-10-08 15:20_

---

_@konstin reviewed on 2024-10-08 15:22_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:391 on 2024-10-08 15:22_

I see these more as `debug_assert!` (i don't see a way how we could violate this invariant), so I've kept the reporting minimal.

---

_@BurntSushi reviewed on 2024-10-08 15:26_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:391 on 2024-10-08 15:26_

Yeah that makes sense!

---

_@BurntSushi reviewed on 2024-10-08 15:26_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:409 on 2024-10-08 15:26_

Thanks!

---

_Merged by @konstin on 2024-10-08 16:38_

---

_Closed by @konstin on 2024-10-08 16:38_

---

_Branch deleted on 2024-10-08 16:38_

---
