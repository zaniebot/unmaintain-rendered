```yaml
number: 16432
title: Add example to docs for a release job within GitLab CI
type: pull_request
state: open
author: jkrauth
labels: []
assignees: []
base: main
head: docs-gitlab-ci-publish
created_at: 2025-10-24T07:51:50Z
updated_at: 2025-10-27T10:01:58Z
url: https://github.com/astral-sh/uv/pull/16432
synced_at: 2026-01-12T16:12:15Z
```

# Add example to docs for a release job within GitLab CI

---

_@jkrauth_

## Summary

This pull request adds an example of a GitLab release job to the integration docs.

How to publish to the GitLab registry with `uv publish` was discussed in issue [#9195](https://github.com/astral-sh/uv/issues/9195#issuecomment-2483840918), including a suggestion to show an example in the docs, which has not happened yet.

Furthermore, it took a bit of effort to combine the following for a uv workspace:

- Publish all packages to GitLab PyPI registry (as explained in the issue linked above)
- Create one release item (using the GitLab CI release block)
- Link wheel files in PyPI registry to the release using assets-links (`uv publish` knows, but does not provide those links)

An example in the docs might be very helpful for others who want to publish and use code from their GitLab registry.

---

_Renamed from "Add example for a release job within GitLab CI" to "Add example to docs for a release job within GitLab CI" by @jkrauth on 2025-10-24 07:55_

---
