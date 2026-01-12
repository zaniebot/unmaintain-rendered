```yaml
number: 12885
title: make distability opt-in
type: pull_request
state: merged
author: Gankra
labels:
  - internal
assignees: []
merged: true
base: main
head: gankra/uv-build-dist
created_at: 2025-04-14T18:35:12Z
updated_at: 2025-04-14T18:49:30Z
url: https://github.com/astral-sh/uv/pull/12885
synced_at: 2026-01-12T16:10:26Z
```

# make distability opt-in

---

_@Gankra_

We have been claiming in our releases that we provide archives/installers for uv-build, but we only upload it as a wheel to pypi. This is because cargo-dist tries to be helpful and find all your apps, but this scales poorly to large workspaces like ours, as stuff like this slips in. So invert the default and make uv the only package dist will see until we say otherwise.

See e.g. https://github.com/astral-sh/uv/releases/tag/0.6.14

Fixes #12883

---

_Label `internal` added by @Gankra on 2025-04-14 18:35_

---

_Merged by @Gankra on 2025-04-14 18:49_

---

_Closed by @Gankra on 2025-04-14 18:49_

---

_Branch deleted on 2025-04-14 18:49_

---
