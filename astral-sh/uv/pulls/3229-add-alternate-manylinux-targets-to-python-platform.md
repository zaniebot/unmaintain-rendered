```yaml
number: 3229
title: "Add alternate manylinux targets to `--python-platform`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/many
created_at: 2024-04-24T00:12:51Z
updated_at: 2024-04-24T11:53:26Z
url: https://github.com/astral-sh/uv/pull/3229
synced_at: 2026-01-12T16:05:30Z
```

# Add alternate manylinux targets to `--python-platform`

---

_@charliermarsh_

## Summary

I initially implemented this by allowing arbitrary `x86_64-manylinux_x_y`, but it makes the Clap, Serde, and Schemars definitions more complicated, _and_ makes it harder for the user. Ultimately, manylinux itself only provides images for 2_17 and 2_28, so this seems like it should be sufficient.

Closes https://github.com/astral-sh/uv/issues/3222.


---

_Label `enhancement` added by @charliermarsh on 2024-04-24 00:12_

---

_Review requested from @konstin by @charliermarsh on 2024-04-24 00:13_

---

_Marked ready for review by @charliermarsh on 2024-04-24 00:13_

---

_Renamed from "Add alternate manylinux targets to --python-platform" to "Add alternate manylinux targets to `--python-platform`" by @charliermarsh on 2024-04-24 00:13_

---

_@charliermarsh reviewed on 2024-04-24 00:34_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/target_triple.rs`:32 on 2024-04-24 00:34_

We could consider removing these and _only_ supporting the manylinux versions? And change `x86_64-unknown-linux-musl` to `x86_64-musllinx_1_2`?

---

_Comment by @messense on 2024-04-24 09:29_

> Ultimately, manylinux itself only provides images for 2_17 and 2_28, so this seems like it should be sufficient.

They used to also provide docker images for 2_24, while it's deprecated now they are still some packages targeting it.

BTW, some manylinux policies do not have official docker images probably because the version is new enough to just build on a recent Ubuntu system. See also

* https://github.com/pypa/auditwheel/blob/main/src/auditwheel/policy/policy-schema.json
* https://github.com/mayeut/pep600_compliance?tab=readme-ov-file#acceptable-distros-to-build-wheels

---

_@konstin approved on 2024-04-24 11:38_

---

_Comment by @charliermarsh on 2024-04-24 11:53_

I saw at least one package that publishes at 2_27, but even in that case it seems reasonable to just expose 2_28, ie, have some coarseness in the setting. We can always expand support in the future.

---

_Merged by @charliermarsh on 2024-04-24 11:53_

---

_Closed by @charliermarsh on 2024-04-24 11:53_

---

_Branch deleted on 2024-04-24 11:53_

---
