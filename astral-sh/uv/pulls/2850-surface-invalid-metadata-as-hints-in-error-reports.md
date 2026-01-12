```yaml
number: 2850
title: Surface invalid metadata as hints in error reports
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/extra
created_at: 2024-04-06T01:11:12Z
updated_at: 2024-04-10T03:12:11Z
url: https://github.com/astral-sh/uv/pull/2850
synced_at: 2026-01-12T16:05:15Z
```

# Surface invalid metadata as hints in error reports

---

_@charliermarsh_

## Summary

Closes #2847.


---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:4561 on 2024-04-06 01:12_

I thought we turned off the wrapping here.

---

_@charliermarsh reviewed on 2024-04-06 01:12_

---

_Label `error messages` added by @charliermarsh on 2024-04-06 01:12_

---

_Marked ready for review by @charliermarsh on 2024-04-06 14:58_

---

_Renamed from "Surface invalid metadata as hints in erro reports" to "Surface invalid metadata as hints in error reports" by @charliermarsh on 2024-04-06 14:58_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-06 14:58_

---

_@zanieb reviewed on 2024-04-06 15:00_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:4561 on 2024-04-06 15:00_

It's just off in the scenarios afaik

---

_@zanieb reviewed on 2024-04-06 15:01_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:4561 on 2024-04-06 15:01_

Oh I see interesting... is `textwrap::indent` doing this?

---

_@charliermarsh reviewed on 2024-04-06 15:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:4561 on 2024-04-06 15:27_

Oh maybe. I could disable it manually by reading the env var... We use it in a few places.

---

_@charliermarsh reviewed on 2024-04-06 15:29_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:635 on 2024-04-06 15:29_

It's confusing but for URLs, we don't have a version... so both `UnavailablePackage` and `IncompletePackage` have an `InvalidStructure` and `InvalidMetadata` variant.

---

_@zanieb reviewed on 2024-04-06 15:55_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:4561 on 2024-04-06 15:55_

We should probably implement https://github.com/zkat/miette/pull/328

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:4561 on 2024-04-06 17:49_

Oh actually, I don't think `textwrap::indent` does this. We're just missing `UV_NO_WRAP=1`. I'll put up a separate PR.

---

_@charliermarsh reviewed on 2024-04-06 17:49_

---

_@zanieb reviewed on 2024-04-06 19:36_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:4561 on 2024-04-06 19:36_

Ah yeah miette used `textwrap::fill` instead of `indent`.

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/report.rs`:586 on 2024-04-08 11:17_

Nit: Can we move the `hint:` outside the match?

---

_@konstin approved on 2024-04-08 11:19_

---

_Review comment by @konstin on `scripts/packages/dependent_editables/second_editable/build/lib/second_editable/__init__.py`:1 on 2024-04-08 12:16_

I don't understand how this file shows up regularly in git, we have `build/` in the gitignore. `git rm -rf --cached . && git add .` also did nothing.

---

_@konstin reviewed on 2024-04-08 12:16_

---

_@charliermarsh reviewed on 2024-04-08 13:43_

---

_Review comment by @charliermarsh on `scripts/packages/dependent_editables/second_editable/build/lib/second_editable/__init__.py`:1 on 2024-04-08 13:43_

I added an ignore for this in a separate PR.

---

_@konstin reviewed on 2024-04-08 14:17_

---

_Review comment by @konstin on `scripts/packages/dependent_editables/second_editable/build/lib/second_editable/__init__.py`:1 on 2024-04-08 14:17_

ah i see, thanks

---

_Merged by @charliermarsh on 2024-04-10 03:12_

---

_Closed by @charliermarsh on 2024-04-10 03:12_

---

_Branch deleted on 2024-04-10 03:12_

---
