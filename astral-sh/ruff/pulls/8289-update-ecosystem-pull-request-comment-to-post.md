```yaml
number: 8289
title: Update ecosystem pull request comment to post when unrelated jobs fail
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/ci-fix-pr-comment
created_at: 2023-10-27T21:07:48Z
updated_at: 2023-10-27T21:21:10Z
url: https://github.com/astral-sh/ruff/pull/8289
synced_at: 2026-01-12T02:32:42Z
```

# Update ecosystem pull request comment to post when unrelated jobs fail

---

_Pull request opened by @zanieb on 2023-10-27 21:07_

While we run the `pr-comment` workflow on `completed` workflows (i.e. instead of `success`, it can have failed) we use the default artifact download behavior which requires `success` workflows (https://github.com/dawidd6/action-download-artifact) which means that, if any jobs failed, the ecosystem checks would not be reported.

For example, a successful ecosystem run at https://github.com/astral-sh/ruff/actions/runs/6672033727/job/18135290880 was not posted at https://github.com/astral-sh/ruff/actions/runs/6672153598/job/18135534008 because no artifacts met the success criterion.

You can see this is "valid" with a manual dispatch at https://github.com/astral-sh/ruff/actions/runs/6672278055/job/18135883849 but it pulls from the latest commit rather than the one with the failed one mentioned above so you can't see verification it'll work for failed jobs. Another manual dispatch at https://github.com/astral-sh/ruff/actions/runs/6672349316/job/18136082917 shows it works great for successful jobs still.

---

_Label `internal` added by @zanieb on 2023-10-27 21:10_

---

_@charliermarsh approved on 2023-10-27 21:13_

---

_Merged by @zanieb on 2023-10-27 21:20_

---

_Closed by @zanieb on 2023-10-27 21:20_

---

_Branch deleted on 2023-10-27 21:20_

---
