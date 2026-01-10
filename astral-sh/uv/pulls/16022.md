```yaml
number: 16022
title: "feat:add version tag annotation (pin #15980)"
type: pull_request
state: merged
author: Arkenstone999
labels:
  - internal
assignees: []
merged: true
base: main
head: Arkenstone/tag-annotation-aws
created_at: 2025-09-25T09:34:33Z
updated_at: 2025-09-25T13:25:35Z
url: https://github.com/astral-sh/uv/pull/16022
synced_at: 2026-01-10T06:36:15Z
```

# feat:add version tag annotation (pin #15980)

---

_Pull request opened by @Arkenstone999 on 2025-09-25 09:34_

## Summary

Clarifies the inline annotation for the pinned `aws-actions/configure-aws-credentials` action. Minor changes, but it's a pleasure to contribute to a rust project !

## Test Plan

Minor doc change only. Verified by running:

cargo nextest run    
cargo run python install

---

_@zanieb reviewed on 2025-09-25 12:52_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1797 on 2025-09-25 12:52_

I don't think this is the right commit for v5.0.0 

https://github.com/aws-actions/configure-aws-credentials/commit/a03048d87541d1d9fcf2ecf528a4a65ba9bd7838

---

_@Arkenstone999 reviewed on 2025-09-25 13:17_

---

_Review comment by @Arkenstone999 on `.github/workflows/ci.yml`:1797 on 2025-09-25 13:17_

I updated the PR to use the correct commit SHA for v5.0.0 :)) sorry for this mistake

---

_@zanieb approved on 2025-09-25 13:25_

---

_Merged by @zanieb on 2025-09-25 13:25_

---

_Closed by @zanieb on 2025-09-25 13:25_

---

_Label `internal` added by @zanieb on 2025-09-25 13:25_

---

_Comment by @zanieb on 2025-09-25 13:25_

Thanks!

---
