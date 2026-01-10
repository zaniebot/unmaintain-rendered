```yaml
number: 8745
title: Ecosystem reports can use a stale baseline
type: issue
state: open
author: zanieb
labels:
  - ci
assignees: []
created_at: 2023-11-17T18:18:18Z
updated_at: 2024-04-03T01:47:15Z
url: https://github.com/astral-sh/ruff/issues/8745
synced_at: 2026-01-10T11:09:51Z
```

# Ecosystem reports can use a stale baseline

---

_Issue opened by @zanieb on 2023-11-17 18:18_

Ecosystem reports on pull requests can include "false" changes from a recently committed change.

The way this works is:

- We download the baseline Ruff binary from the last CI build on `main`
- We use the pull request Ruff binary for comparison which is building on a temporary branch that is _merged_ with `main`
- If the last commit on `main` has not finished CI (i.e. a 20 minute gap or whatever) then the commit will be present in the comparison but not the baseline.

We could solve this by building the baseline from the base commit instead of downloading it from a finished CI run. This would significantly increase the runtime of ecosystem checks though. We may want to just leave it as-is.

---

_Label `ci` added by @zanieb on 2023-11-17 18:19_

---

_Comment by @zanieb on 2024-04-03 01:47_

I wrote this up a second time :) ref #10746 

The ecosystem checks perform a comparison between the branch `ruff` binary and a baseline `ruff` binary. We pull a baseline binary from the base branch of the pull request:

https://github.com/astral-sh/ruff/blob/814b26f82e1e6a678dc59ee4cd0fc1c277efb517/.github/workflows/ci.yaml#L248-L254

GitHub runs pull request CI against a "merge" commit of the pull request HEAD and the base branch. This commit is not guaranteed to be the same commit as we pulled a baseline for above. We see this manifest as incorrect ecosystem reports on pull requests — they include changes for "recently" merged pull requests.

I believe something like the following occurs:

- A commit is made to main and a baseline binary (1) is uploaded
- A commit is made to main and a baseline binary (2) build starts
- Open a pull request, a branch binary build starts — this includes the second commit made to main
- The pull request ecosystem checks start and pull latest baseline binary (1)
- Changes from (2) are incorrectly reported as coming from the pull request



---
