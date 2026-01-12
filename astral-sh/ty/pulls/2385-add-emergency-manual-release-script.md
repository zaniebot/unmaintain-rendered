```yaml
number: 2385
title: add emergency manual release script
type: pull_request
state: open
author: Gankra
labels:
  - release
assignees: []
base: main
head: gankra/man-rel
created_at: 2026-01-07T22:58:22Z
updated_at: 2026-01-08T08:56:41Z
url: https://github.com/astral-sh/ty/pull/2385
synced_at: 2026-01-12T15:54:28Z
```

# add emergency manual release script

---

_@Gankra_

Same one in uv but with the prerelease line delete because @zanieb noted it was broken

---

_Label `release` added by @Gankra on 2026-01-07 22:58_

---

_@zanieb reviewed on 2026-01-07 23:08_

---

_Review comment by @zanieb on `scripts/manual-github-release.sh`:38 on 2026-01-07 23:08_

This might be a problematic line too? I'm not sure we should omit it unless we reproduce the problem though. Also, I think no part of this script is repo specific, so we should probably just put it in it's own repo or a gist and link to it in CONTRIBUTING?

---

_@Gankra reviewed on 2026-01-07 23:29_

---

_Review comment by @Gankra on `scripts/manual-github-release.sh`:38 on 2026-01-07 23:29_

I *think* it was just the later line with the echo that was messed up

---

_Review comment by @MichaReiser on `scripts/manual-github-release.sh`:38 on 2026-01-08 08:41_

Regardless of where we add it. Can we add a mention to `CONTRIBUTING.md` that this script exist and when/how to use it

---

_@MichaReiser reviewed on 2026-01-08 08:41_

---

_Comment by @AlexWaygood on 2026-01-08 08:56_

We have the shellcheck hook enabled in Ruff's pre-commit config but not in this repo — it might be an idea to add it here too if we're adding more shell scripts. It's really good at checking for syntax issues and common mistakes in shell scripts. https://github.com/astral-sh/ruff/blob/7319c37f4eb063e9590e1f09c8e92d7dabc63403/.pre-commit-config.yaml#L134

---
