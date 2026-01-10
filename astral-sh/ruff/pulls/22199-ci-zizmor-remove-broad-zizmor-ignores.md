```yaml
number: 22199
title: "ci(zizmor): remove broad zizmor ignores"
type: pull_request
state: merged
author: samypr100
labels:
  - ci
assignees: []
merged: true
base: main
head: remove-zizmor-broad-ignores
created_at: 2025-12-25T23:35:16Z
updated_at: 2025-12-29T13:45:51Z
url: https://github.com/astral-sh/ruff/pull/22199
synced_at: 2026-01-10T16:36:18Z
```

# ci(zizmor): remove broad zizmor ignores

---

_Pull request opened by @samypr100 on 2025-12-25 23:35_

## Summary

Follow up to https://github.com/astral-sh/ty/pull/2217

Encountered that ty doesn't have a zizmor.yml with broad ignores. Dropped the file in favor of inline ignores.

1. build-docker.yml could be fixed by re-defining the permissions with an explicit `zizmor: ignore[excessive-permissions]`. This aligns with what ty does.
2. publish-docs.yml seems to only need `content: read` which fixes the errors.

Separately, I didn't get errors about cache-poisoning, secrets-inherit, or template-injection so the file has been removed.

## Test Plan

Published a release from my fork with the changes.

* [Release](https://github.com/samypr100/ruff/actions/runs/20540204455)
* [Packages](https://github.com/samypr100/ruff/pkgs/container/ruff/versions?filters%5Bversion_type%5D=tagged)


---

_Label `ci` added by @AlexWaygood on 2025-12-25 23:49_

---

_Marked ready for review by @samypr100 on 2025-12-27 15:04_

---

_@woodruffw reviewed on 2025-12-28 21:44_

---

_Review comment by @woodruffw on `.github/workflows/publish-docs.yml`:21 on 2025-12-28 21:44_

Nit: this could be `permissions: {}` and then `contents: read` on the specific job that actually needs it, but that's pretty minor.

---

_@woodruffw approved on 2025-12-28 21:45_

LGTM! Another option here would be to remove the inline ignores entirely, and allow these findings to percolate into the advanced security dashboard to be triaged later. I think that'd be fine, although we might need to tweak the CI rulesets to allow the zizmor workflow to produce findings without failing the entire CI.

---

_@samypr100 reviewed on 2025-12-28 22:36_

---

_Review comment by @samypr100 on `.github/workflows/publish-docs.yml`:21 on 2025-12-28 22:36_

I deliberately kept it as [ty](https://github.com/astral-sh/ty/blob/95761966c231e40fc615368ebdbef09ebe8158d0/.github/workflows/publish-docs.yml#L20-L21) to minimize divergence. From what I see in the workflow though, I agree that `{}` should suffice. I think `contents: read` is not even needed.

---

_Comment by @MichaReiser on 2025-12-29 09:52_

Thank you! I'll merge this as is and suggest that we remove the inline ignores for both ty and ruff in separate PRs.

---

_Merged by @MichaReiser on 2025-12-29 09:52_

---

_Closed by @MichaReiser on 2025-12-29 09:52_

---

_Branch deleted on 2025-12-29 13:45_

---
