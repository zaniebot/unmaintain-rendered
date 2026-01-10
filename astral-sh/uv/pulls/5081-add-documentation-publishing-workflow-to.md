```yaml
number: 5081
title: Add documentation publishing workflow to standalone repo
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: charlie/docs-repo
created_at: 2024-07-15T20:31:31Z
updated_at: 2024-07-16T13:14:55Z
url: https://github.com/astral-sh/uv/pull/5081
synced_at: 2026-01-10T13:42:52Z
```

# Add documentation publishing workflow to standalone repo

---

_Pull request opened by @charliermarsh on 2024-07-15 20:31_

## Summary

This approach is based on https://github.com/PrefectHQ/docs. Rather than publishing docs in the uv repo, we push to an independent repo that's used solely to house the docs. In Prefect's case, this allows them to publish versioned documentation (we should do that too). For us, though, the benefit is that we can publish the Ruff and uv docs as a single site (docs.astral.sh).

Prefect clones the source repo and builds the documentation from the `docs` repo (i.e., the action runs in the `docs` repo). In our case, I've instead set it up such that the action runs in `uv` (and later in `ruff` too), clones the `docs` repo, and puts up a PR in that separate repo. Because of these requirements, we have to use a PAT rather than a deploy key (as PATs cannot do GitHub-specific things like create PRs -- they can only operate over the Git CLI).

See: https://github.com/astral-sh/docs/pull/2.


---

_Comment by @charliermarsh on 2024-07-15 20:31_

This will take me 1,000 attempts to get working.

---

_Review requested from @zanieb by @charliermarsh on 2024-07-15 21:44_

---

_Label `documentation` added by @charliermarsh on 2024-07-15 21:44_

---

_Label `internal` added by @charliermarsh on 2024-07-15 21:44_

---

_Marked ready for review by @charliermarsh on 2024-07-15 21:45_

---

_@zanieb reviewed on 2024-07-15 23:42_

---

_Review comment by @zanieb on `.github/workflows/publish-docs.yml`:101 on 2024-07-15 23:42_

Using the $GITHUB_ACTOR variables here feels cleaner e.g. https://github.com/astral-sh/packse/blob/main/.github/workflows/release.yaml#L71-L72

---

_@zanieb reviewed on 2024-07-15 23:44_

---

_Review comment by @zanieb on `.github/workflows/publish-docs.yml`:127 on 2024-07-15 23:44_

Why login instead of just putting the token as an environment variable? This will persist to disk. Seems like a larger attack surface.

---

_@zanieb reviewed on 2024-07-15 23:44_

---

_Review comment by @zanieb on `.github/workflows/publish-docs.yml`:42 on 2024-07-15 23:44_

May be mildly confusing if we're not versioning yet

---

_@zanieb reviewed on 2024-07-15 23:45_

---

_Review comment by @zanieb on `.github/workflows/publish-docs.yml`:25 on 2024-07-15 23:45_

Why do we need these permissions if we're writing to another repository with a dedicated secret token?

---

_@charliermarsh reviewed on 2024-07-15 23:52_

---

_Review comment by @charliermarsh on `.github/workflows/publish-docs.yml`:25 on 2024-07-15 23:52_

Sorry, we don't, this is copied over from Prefect.

---

_@charliermarsh reviewed on 2024-07-15 23:52_

---

_Review comment by @charliermarsh on `.github/workflows/publish-docs.yml`:101 on 2024-07-15 23:52_

Tell that to Prefect...

---

_@charliermarsh reviewed on 2024-07-15 23:54_

---

_Review comment by @charliermarsh on `.github/workflows/publish-docs.yml`:127 on 2024-07-15 23:54_

I seem to recall that it didn't work, but let me test again now that I removed the secondary SSH key -- that may have been the root cause.

---

_Review requested from @zanieb by @charliermarsh on 2024-07-15 23:59_

---

_Comment by @charliermarsh on 2024-07-16 00:07_

Thanks @zanieb!

---

_@zanieb approved on 2024-07-16 04:16_

Awesome :)

---

_Merged by @charliermarsh on 2024-07-16 13:14_

---

_Closed by @charliermarsh on 2024-07-16 13:14_

---

_Branch deleted on 2024-07-16 13:14_

---
