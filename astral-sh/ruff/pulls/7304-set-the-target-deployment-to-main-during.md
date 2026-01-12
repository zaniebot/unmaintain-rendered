```yaml
number: 7304
title: "Set the target deployment to `main` during dispatched documentation deployments"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/ci-head-ref
created_at: 2023-09-12T14:56:08Z
updated_at: 2023-09-12T15:34:06Z
url: https://github.com/astral-sh/ruff/pull/7304
synced_at: 2026-01-12T02:45:39Z
```

# Set the target deployment to `main` during dispatched documentation deployments

---

_Pull request opened by @zanieb on 2023-09-12 14:56_

Closes #7276 by deploying to production when not triggered by a pull request.

---

_Comment by @zanieb on 2023-09-12 14:58_

Testing with dispatch at https://github.com/astral-sh/ruff/actions/runs/6161239564 (using workflow from this branch and target ref of `v0.0.288`)

---

_Comment by @zanieb on 2023-09-12 15:12_

Confirmed that this deployed to production successfully!

We got the confusing output

```
npx wrangler pages deploy site --project-name=ruff-docs --branch main --commit-hash ${GITHUB_SHA}
  Uploading... (728/730)
  Uploading... (729/730)
  Uploading... (730/730)
  ✨ Success! Uploaded 2 files (728 already uploaded) (1.39 sec)
  
  ✨ Deployment complete! Take a peek over at https://02459792.ruff-docs.pages.dev/
  
  Error: fatal: bad object 0181c74ed05496287668a263116ceef6a93f582a
```

but it worked 🤷‍♀️ 

Looks like a `git` error message?

---

_Comment by @zanieb on 2023-09-12 15:21_

Tracking at https://github.com/cloudflare/wrangler-action/issues/169

---

_Review requested from @charliermarsh by @zanieb on 2023-09-12 15:21_

---

_@charliermarsh approved on 2023-09-12 15:33_

Nice

---

_Merged by @zanieb on 2023-09-12 15:34_

---

_Closed by @zanieb on 2023-09-12 15:34_

---

_Branch deleted on 2023-09-12 15:34_

---
