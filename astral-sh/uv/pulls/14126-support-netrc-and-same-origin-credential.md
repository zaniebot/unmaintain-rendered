```yaml
number: 14126
title: Support netrc and same-origin credential propagation on index redirects
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: feature/redirect-authentication
created_at: 2025-06-18T12:15:23Z
updated_at: 2025-06-20T07:21:34Z
url: https://github.com/astral-sh/uv/pull/14126
synced_at: 2026-01-12T16:11:02Z
```

# Support netrc and same-origin credential propagation on index redirects

---

_@jtfmumm_

This PR is a combination of #12920 and #13754. Prior to these changes, following a redirect when searching indexes would bypass our authentication middleware. This PR updates uv to support propagating credentials through our middleware on same-origin redirects and to support netrc credentials for both same- and cross-origin redirects. It does not handle the case described in #11097 where the redirect location itself includes credentials (e.g., `https://user:pass@redirect-location.com`). That will be addressed in follow-up work.

This includes unit tests for the new redirect logic and integration tests for credential propagation. The automated external registries test is also passing for AWS CodeArtifact, Azure Artifacts, GCP Artifact Registry, JFrog Artifactory, GitLab, Cloudsmith, and Gemfury.

---

_Label `bug` added by @jtfmumm on 2025-06-18 12:15_

---

_Merged by @jtfmumm on 2025-06-20 07:21_

---

_Closed by @jtfmumm on 2025-06-20 07:21_

---

_Branch deleted on 2025-06-20 07:21_

---
