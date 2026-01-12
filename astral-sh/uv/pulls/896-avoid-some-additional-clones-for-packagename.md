```yaml
number: 896
title: "Avoid some additional clones for `PackageName`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/small
created_at: 2024-01-12T04:54:13Z
updated_at: 2024-01-12T17:54:42Z
url: https://github.com/astral-sh/uv/pull/896
synced_at: 2026-01-12T16:04:16Z
```

# Avoid some additional clones for `PackageName`

---

_@charliermarsh_

_No description provided._

---

_Review comment by @charliermarsh on `crates/puffin-normalize/src/package_name.rs`:50 on 2024-01-12 04:54_

We can avoid allocating if it doesn't contain `-`.

---

_@charliermarsh reviewed on 2024-01-12 04:54_

---

_@charliermarsh reviewed on 2024-01-12 04:55_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/traits.rs`:38 on 2024-01-12 04:55_

I think we can use _any_ separator here that doesn't appear in versions? As in, I assume the reason for using `as_dist_info_name` is that we want to avoid ambiguity because otherwise dashes can appear in versions.

---

_Review requested from @konstin by @charliermarsh on 2024-01-12 04:55_

---

_Label `internal` added by @charliermarsh on 2024-01-12 04:55_

---

_@konstin reviewed on 2024-01-12 09:10_

---

_Review comment by @konstin on `crates/distribution-types/src/traits.rs`:38 on 2024-01-12 09:10_

Dashes can't appear in normalized versions; i picked that format because it's the same normalization as in wheel filenames.

---

_@konstin approved on 2024-01-12 09:14_

Did you see `PackageName` in the profiles? I never bothered because in the cases i looked at `Version` was so dominant.

---

_Review comment by @MichaReiser on `crates/puffin-normalize/src/package_name.rs`:50 on 2024-01-12 09:22_

Nit: Slightly fewer branching

```suggestion
        if let Some(dash_position) = self.0.find('-') {
            // Initialize `replaced` with the start of the string up to the current character.
            let mut owned_string = String::with_capacity(self.0.len());
            owned_string.push_str(&self.0[..dash_position]);
            owned_string.push('_');

            // Iterate over the rest of the string.
            owned_string.extend(self.0[dash_position + 1..].chars().map(|character| {
                if character == '-' {
                    '_'
                } else {
                    character
                }
            }));
            

            Cow::Owned(owned_string)
        } else {
            Cow::Borrowed(self.0.as_str())
        }
```

---

_@MichaReiser reviewed on 2024-01-12 09:22_

---

_@konstin reviewed on 2024-01-12 09:28_

---

_Review comment by @konstin on `crates/puffin-normalize/src/package_name.rs`:50 on 2024-01-12 09:28_

I thought about suggesting the `if memchr { Owned(str.replace) } else { Borrowed(original) }` version but then i thought it was intentionally avoided to not parse the string twice.

---

_@charliermarsh reviewed on 2024-01-12 14:39_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/traits.rs`:38 on 2024-01-12 14:39_

Oh I got confused because of pre-releases, but I guess those are stripped from the normalized version?

---

_@konstin reviewed on 2024-01-12 16:19_

---

_Review comment by @konstin on `crates/distribution-types/src/traits.rs`:38 on 2024-01-12 16:19_

The are normalized to `1.1a1`, `1.1b2`, and `1.1rc3` (https://peps.python.org/pep-0440/#pre-release-spelling)

---

_Merged by @charliermarsh on 2024-01-12 17:54_

---

_Closed by @charliermarsh on 2024-01-12 17:54_

---

_Branch deleted on 2024-01-12 17:54_

---
