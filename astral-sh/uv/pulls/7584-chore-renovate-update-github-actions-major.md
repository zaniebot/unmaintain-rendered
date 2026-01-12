```yaml
number: 7584
title: "chore(renovate): update GitHub Actions major versions in docs"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: chore/update-github-actions-references-docs
created_at: 2024-09-20T12:11:21Z
updated_at: 2024-09-20T20:05:58Z
url: https://github.com/astral-sh/uv/pull/7584
synced_at: 2026-01-12T16:07:53Z
```

# chore(renovate): update GitHub Actions major versions in docs

---

_@mkniewallner_

## Summary

Originally wanted to update the reference to `astral-sh/setup-uv` in https://docs.astral.sh/uv/guides/integration/github/, but thought it could be nice to automate those updates through Renovate. The custom manager will look for any major version GitHub Action reference in any Markdown file in `docs` directory, and raise a PR to update it.

Possible improvements:
- We could separate those updates from updating the actions updates for uv's own GitHub Actions workflow, which would end up raising 2 different PRs instead of grouping them (example of the current behaviour without this improvement in https://github.com/mkniewallner/mkv-playground/pull/4 where we update the doc reference at the same time as a real dependency usage in a workflow).
- ~Should the PRs be raised immediately, to handle the update as soon as possible, instead of waiting for the regular weekly Monday schedule? This would ensure that `astral-sh/setup-uv` references are handled as early as possible.~ done in https://github.com/astral-sh/uv/pull/7584/commits/6af7f4575071091b337a9fd3f24bc4459923f2a2

## Test Plan

I've tested that with https://github.com/mkniewallner/mkv-playground/blob/00ddfb69003843ad57d05f9ef036e655aaf765d7/renovate.json5 and https://github.com/mkniewallner/mkv-playground/blob/00ddfb69003843ad57d05f9ef036e655aaf765d7/docs/integeration/foo.md, where Renovate raised 2 PRs:
- https://github.com/mkniewallner/mkv-playground/pull/13
- https://github.com/mkniewallner/mkv-playground/pull/4

---

_Marked ready for review by @mkniewallner on 2024-09-20 12:35_

---

_Comment by @zanieb on 2024-09-20 18:04_

Thank you! This is great.

I don't mind a weekly cadence.

Separate pull requests seems kind of nice? But maybe not necessary. Is it hard to do? I'm willing to wait and see if it's problematic.

---

_Assigned to @zanieb by @zanieb on 2024-09-20 18:05_

---

_Comment by @mkniewallner on 2024-09-20 18:24_

> Separate pull requests seems kind of nice? But maybe not necessary. Is it hard to do? I'm willing to wait and see if it's problematic.

I don't think it's hard to do, will play with that in my test repository then update this PR (or report back if this is in the end).

---

_Comment by @mkniewallner on 2024-09-20 19:12_

> > Separate pull requests seems kind of nice? But maybe not necessary. Is it hard to do? I'm willing to wait and see if it's problematic.
> 
> I don't think it's hard to do, will play with that in my test repository then update this PR (or report back if this is in the end).

Played with that in https://github.com/mkniewallner/mkv-playground/commit/4c3ea0d2c8b6d1fd6351c2a1b3dca90e53f84886 (inspired by an example [from Renovate itself](https://github.com/renovatebot/.github/blob/fa0121c7ff88926f196fdb22b5f00851cf08439d/default.json#L62-L77)), and it works well:
- real dependency usage: https://github.com/mkniewallner/mkv-playground/pull/4/files
- dependency reference in the documentation: https://github.com/mkniewallner/mkv-playground/pull/16/files

---

_Review comment by @zanieb on `.github/renovate.json5`:31 on 2024-09-20 19:15_

For clarity, can we mention the docs in the title?

```suggestion
      commitMessageTopic: "documentation references to {{{depName}}}",
```

---

_@zanieb reviewed on 2024-09-20 19:15_

---

_Label `documentation` added by @zanieb on 2024-09-20 19:15_

---

_Label `releases` added by @zanieb on 2024-09-20 19:15_

---

_Label `releases` removed by @zanieb on 2024-09-20 19:15_

---

_@zanieb approved on 2024-09-20 19:15_

---

_Merged by @zanieb on 2024-09-20 19:58_

---

_Closed by @zanieb on 2024-09-20 19:58_

---

_Branch deleted on 2024-09-20 20:05_

---
