```yaml
number: 21757
title: Use our org-wide Renovate preset
type: pull_request
state: closed
author: woodruffw
labels:
  - internal
  - ci
assignees: []
base: main
head: ww/renovate-org-preset
created_at: 2025-12-02T17:19:05Z
updated_at: 2025-12-02T17:57:36Z
url: https://github.com/astral-sh/ruff/pull/21757
synced_at: 2026-01-10T16:48:02Z
```

# Use our org-wide Renovate preset

---

_Pull request opened by @woodruffw on 2025-12-02 17:19_

## Summary

This switches the preset to https://github.com/astral-sh/renovate-config, which is a superset of the `config:recommended` preset (with cooldowns added).

## Test Plan

No functional changes.

---

_Assigned to @woodruffw by @woodruffw on 2025-12-02 17:19_

---

_Label `internal` added by @woodruffw on 2025-12-02 17:19_

---

_Label `ci` added by @woodruffw on 2025-12-02 17:19_

---

_@ntBre approved on 2025-12-02 17:27_

---

_Comment by @woodruffw on 2025-12-02 17:27_

Note: I manually validated this (from the repo root) with:

```
docker run --rm -it -v $(pwd):/workspace -w /workspace renovate/renovate renovate-config-validator ".github/renovate.json5"
```

...however that doesn't seem to have recursively validated the preset. I'm going to confirm that it does (or figure out how to) before merging here.

Edit: well... https://github.com/renovatebot/renovate/issues/30968

I validated the preset itself in its own repo, so by composition the two should be okay.

---

_Comment by @ntBre on 2025-12-02 17:30_

Does this special branch name [validation](https://docs.renovatebot.com/config-validation/#validation-of-renovate-config-change-prs) work on GitHub? (I defer to you, just noticed this when skimming the docs)

---

_Comment by @woodruffw on 2025-12-02 17:32_

> Does this special branch name [validation](https://docs.renovatebot.com/config-validation/#validation-of-renovate-config-change-prs) work on GitHub? (I defer to you, just noticed this when skimming the docs)

Oh, nice! Yeah, I bet so, let me re-PR this with that branch name.

---

_Comment by @woodruffw on 2025-12-02 17:34_

https://github.com/astral-sh/ruff/pull/21759

---

_Closed by @woodruffw on 2025-12-02 17:34_

---

_Branch deleted on 2025-12-02 17:57_

---
