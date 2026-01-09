---
number: 3538
title: Reconcile published and vendored pep508_rs crates
type: issue
state: closed
author: konstin
labels:
  - releases
assignees: []
created_at: 2024-05-13T08:06:02Z
updated_at: 2025-06-24T20:53:57Z
url: https://github.com/astral-sh/uv/issues/3538
synced_at: 2026-01-07T13:12:17-06:00
---

# Reconcile published and vendored pep508_rs crates

---

_Issue opened by @konstin on 2024-05-13 08:06_

There are two versions of the `pep508_rs` crate: The crates.io published one from https://github.com/konstin/pep508_rs, and the vendored one at https://github.com/astral-sh/uv/tree/main/crates/pep508-rs. We added the non-PEP 508 features `VerbatimUrl`, `UnnamedRequirement` and `RequirementOrigin` to the vendored version. We also partially added them to the published version. 

Given that we have users of both the published version and our vendored version (pixi) and that the published is lagging behind, we should split the current vendored pep508_rs crate into a merely spec-compliant core pep508_rs that is synced with the published repository, while moving `VerbatimUrl`, `UnnamedRequirement` and `RequirementOrigin` to `distribution-types` or a similar uv crate. The main challenge is that `UnnamedRequirement` uses the `Cursor` from `pep508_rs` that should not become part of the public api, while `UnnamedRequirement` should not be part of pep508_rs, given that it is not PEP 508 compliant but a pip addition.

We should also split the pyo3 binding off into a separate crate that only exists in https://github.com/konstin/pep508_rs. 

---

_Comment by @tdejager on 2024-05-13 08:07_

Would be great! Let me know if you need any help

---

_Label `release` added by @charliermarsh on 2024-05-13 14:16_

---

_Referenced in [prefix-dev/pixi#2002](../../prefix-dev/pixi/issues/2002.md) on 2024-09-07 14:18_

---

_Comment by @pnasrat on 2025-02-14 13:20_

I’d definitely find the type/metadata crates for peps useful in other rust/python tooling with the plan to create a standalone separate a build backend #3957 it might be an opportunity make those a more public facing api


---

_Comment by @ofek on 2025-02-17 00:25_

I would also be interested in using various parts in [PyApp](https://github.com/ofek/pyapp).

---

_Closed by @konstin on 2025-06-24 20:53_

---
