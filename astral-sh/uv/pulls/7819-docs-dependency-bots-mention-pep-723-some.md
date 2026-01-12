```yaml
number: 7819
title: "docs(dependency-bots): mention PEP 723 + some improvements"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/update-dependency-bots
created_at: 2024-09-30T20:53:32Z
updated_at: 2024-10-10T22:21:29Z
url: https://github.com/astral-sh/uv/pull/7819
synced_at: 2026-01-12T16:08:01Z
```

# docs(dependency-bots): mention PEP 723 + some improvements

---

_@mkniewallner_

## Summary

Renovate recently gained support for updating dependencies defined using PEP 723 (https://github.com/renovatebot/renovate/pull/31266). Since uv supports this format, I thought it could be nice to mention support for it in the integrations documentation as well. I took the occasion to make the page a bit more structured as well.

## Test Plan

Ran Renovate on https://github.com/mkniewallner/renovate-pep723, which created https://github.com/mkniewallner/renovate-pep723/pull/2 that updates a dependency defined using PEP 723. But I'll re-run some tests again once the changes are released on Renovate cloud GitHub app just in case.

---

_@zanieb approved on 2024-09-30 21:08_

---

_Label `documentation` added by @zanieb on 2024-09-30 21:08_

---

_Comment by @zanieb on 2024-09-30 21:08_

Thanks!

---

_Marked ready for review by @mkniewallner on 2024-10-09 16:21_

---

_Comment by @mkniewallner on 2024-10-09 16:22_

Renovate version has been updated in the cloud app today. Just in case, tested the feature on the app and it works as expected: https://github.com/mkniewallner/renovate-pep723/pull/4, so this should be ready to merge.

---

_Merged by @zanieb on 2024-10-09 16:26_

---

_Closed by @zanieb on 2024-10-09 16:26_

---

_Branch deleted on 2024-10-09 16:26_

---

_@konstin reviewed on 2024-10-09 16:48_

---

_Review comment by @konstin on `docs/guides/integration/dependency-bots.md`:32 on 2024-10-09 16:48_

Why the change to jsx here?

---

_@mkniewallner reviewed on 2024-10-09 17:00_

---

_Review comment by @mkniewallner on `docs/guides/integration/dependency-bots.md`:32 on 2024-10-09 17:00_

`json5` did not actually highlight the code as JSON5 in mkdocs, but `jsx` did when testing locally. Not in front of a computer right now so I can't send a screenshot unfortunately, but it should be testable locally.

---

_@mkniewallner reviewed on 2024-10-09 19:12_

---

_Review comment by @mkniewallner on `docs/guides/integration/dependency-bots.md`:32 on 2024-10-09 19:12_

- `json5`:

![Screenshot From 2024-10-09 21-10-28](https://github.com/user-attachments/assets/e6f24ef0-dca3-4d9f-98b0-8b5cdf22ddd1)

- `jsx`:

![Screenshot From 2024-10-09 21-10-36](https://github.com/user-attachments/assets/57213a24-46b0-42bc-85a4-7d2e355f3e10)

---

_@konstin reviewed on 2024-10-10 22:21_

---

_Review comment by @konstin on `docs/guides/integration/dependency-bots.md`:32 on 2024-10-10 22:21_

Interesting catch!

---
