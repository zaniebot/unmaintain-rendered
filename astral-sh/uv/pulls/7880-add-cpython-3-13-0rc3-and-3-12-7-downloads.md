```yaml
number: 7880
title: Add CPython 3.13.0rc3 and 3.12.7 downloads
type: pull_request
state: merged
author: github-actions
labels:
  - enhancement
assignees: []
merged: true
base: main
head: sync-python-releases
created_at: 2024-10-02T21:57:18Z
updated_at: 2024-10-07T16:28:23Z
url: https://github.com/astral-sh/uv/pull/7880
synced_at: 2026-01-12T16:08:03Z
```

# Add CPython 3.13.0rc3 and 3.12.7 downloads

---

_@github-actions_

Automated update for Python releases.

---

_Label `enhancement` added by @zanieb on 2024-10-02 22:04_

---

_Renamed from "Sync latest Python releases" to "Add CPython 3.13.0rc3 and 3.12.7" by @zanieb on 2024-10-02 22:04_

---

_Renamed from "Add CPython 3.13.0rc3 and 3.12.7" to "Add CPython 3.13.0rc3 and 3.12.7 downloads" by @zanieb on 2024-10-02 22:09_

---

_Closed by @zanieb on 2024-10-02 22:09_

---

_Reopened by @zanieb on 2024-10-02 22:09_

---

_Comment by @zanieb on 2024-10-02 22:09_

So annoying the jobs don't start when the PR is opened.

---

_Merged by @zanieb on 2024-10-02 22:20_

---

_Closed by @zanieb on 2024-10-02 22:20_

---

_Branch deleted on 2024-10-02 22:20_

---

_Comment by @edmorley on 2024-10-03 06:40_

> So annoying the jobs don't start when the PR is opened.

@zanieb Hope you don't mind the drive-by and apologies if you've already figured this out, but this is due to the recursive job protection feature of using `GITHUB_TOKEN`:
https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow

This used to drive us up the wall too for all of our automation - we eventually switched to an internal GitHub App along with the [actions/create-github-app-token](https://github.com/actions/create-github-app-token) to generate one-off tokens on the fly. 

As an added bonus this approach also allows for cross-repo access (such as the workflow on one repo directly opening a PR against another; [example](https://github.com/heroku/cnb-builder-images/pull/584)), and the custom App also provides a bit nicer branding than the `github-actions [bot]` user (eg for the author of the GitHub releases; [example](https://github.com/heroku/buildpacks-python/releases)).

See:
https://github.com/actions/create-github-app-token?tab=readme-ov-file#usage

---

_Comment by @zanieb on 2024-10-07 16:28_

Thanks @edmorley! I knew why but wasn't really sure how fix it, so I appreciate the additional context.

---
