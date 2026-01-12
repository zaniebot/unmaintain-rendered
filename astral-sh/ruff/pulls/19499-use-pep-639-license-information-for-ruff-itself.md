```yaml
number: 19499
title: Use PEP 639 license information for Ruff itself instead of classifier
type: pull_request
state: merged
author: DimitriPapadopoulos
labels: []
assignees: []
merged: true
base: main
head: PEP639
created_at: 2025-07-22T21:04:01Z
updated_at: 2025-07-31T09:11:02Z
url: https://github.com/astral-sh/ruff/pull/19499
synced_at: 2026-01-12T15:56:40Z
```

# Use PEP 639 license information for Ruff itself instead of classifier

---

_@DimitriPapadopoulos_

## Summary

Declare licenses using only these two fields, as per PEP 639:
* `license`:       SPDX license expression consisting of one or more license identifiers
* `license-files`: list of license file glob patterns

Supported by maturin ≥ 1.9.0:
https://www.maturin.rs/changelog.html

## Test Plan

N/A

---

_Review requested from @konstin by @charliermarsh on 2025-07-22 21:06_

---

_Marked ready for review by @DimitriPapadopoulos on 2025-07-22 21:07_

---

_Label `internal` added by @ntBre on 2025-07-22 21:31_

---

_@vivodi reviewed on 2025-07-24 10:07_

---

_Review comment by @vivodi on `pyproject.toml`:25 on 2025-07-24 10:07_

Why is this removed?

---

_@DimitriPapadopoulos reviewed on 2025-07-24 11:34_

---

_Review comment by @DimitriPapadopoulos on `pyproject.toml`:25 on 2025-07-24 11:34_

Because PEP639 expects only `licence` and `licence-files`.
* [PEP 639 – Improving License Clarity with Better Package Metadata](https://peps.python.org/pep-0639/)
* [Core metadata specifications](https://packaging.python.org/en/latest/specifications/core-metadata/)

It's redundant with:
```toml
license = "MIT"
```

---

_@vivodi reviewed on 2025-07-24 11:39_

---

_Review comment by @vivodi on `pyproject.toml`:25 on 2025-07-24 11:39_

Although PEP 639 expects only `license` and `license-files`, explicitly specifying license information in `classifiers` is still a widely adopted practice. Moreover, it does not conflict with PEP 639 — the PEP does not suggest removing license information from `classifiers`.


---

_@vivodi reviewed on 2025-07-24 11:47_

---

_Review comment by @vivodi on `pyproject.toml`:25 on 2025-07-24 11:47_

Sorry, that PEP **does indeed recommend removing the license classifier**: [https://peps.python.org/pep-0639/#deprecate-license-classifiers](https://peps.python.org/pep-0639/#deprecate-license-classifiers) — I hadn’t looked at it carefully before.


---

_@konstin reviewed on 2025-07-27 19:09_

I confirmed that after this change, there's
```
License-File: LICENSE
License-Expression: MIT
```
in the METADATA and a license file at `/ruff-0.12.4.dist-info/licenses/LICENSE`

---

_@konstin approved on 2025-07-27 19:09_

---

_Renamed from "PEP 639 compliance" to "Use PEP 639 license information for Ruff itself instead of classifier" by @konstin on 2025-07-27 19:11_

---

_Label `internal` removed by @konstin on 2025-07-27 19:11_

---

_Label `documentation` added by @konstin on 2025-07-27 19:11_

---

_Merged by @konstin on 2025-07-28 07:43_

---

_Closed by @konstin on 2025-07-28 07:43_

---

_Label `documentation` removed by @MichaReiser on 2025-07-28 07:48_

---

_Comment by @MichaReiser on 2025-07-28 07:48_

I removed the documentation label so that it appears under the "Other" section (we tend to remove the *Documentation* section in releases)

---

_Branch deleted on 2025-07-28 08:28_

---

_Comment by @vivodi on 2025-07-31 07:56_

I see this PR has been fully reverted:
  - https://github.com/astral-sh/ruff/pull/19599
  - https://github.com/astral-sh/ruff/pull/19624

Will you @DimitriPapadopoulos have time to look into it?

---

_Comment by @DimitriPapadopoulos on 2025-07-31 09:11_

Unfortunately #19624 lacks context. Is the problem that `uv build` does not support PEP 639 yet?

---
