```yaml
number: 10406
title: Add an example with Lambda Layers to AWS Lambda docs
type: issue
state: closed
author: charliermarsh
labels:
  - documentation
assignees: []
created_at: 2025-01-08T19:21:39Z
updated_at: 2025-01-08T22:12:54Z
url: https://github.com/astral-sh/uv/issues/10406
synced_at: 2026-01-12T16:00:13Z
```

# Add an example with Lambda Layers to AWS Lambda docs

---

_@charliermarsh_

See: https://x.com/sertherk/status/1877071200322556292

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-08 19:21_

---

_Label `documentation` added by @charliermarsh on 2025-01-08 19:21_

---

_Comment by @charliermarsh on 2025-01-08 19:24_

I think this will require the use of `--no-installer-metadata` to ensure that the dependency layer isn't invalidated, but I need to try it in practice.

---

_Comment by @sarflux on 2025-01-08 19:28_

I am not sure how much of this can be helped by uv but adding this as a reference https://github.com/aws/aws-cdk/discussions/19706

---

_Comment by @sarflux on 2025-01-08 19:49_

I think using the zip based approach would solve this. I'm trying it out now. if it works I can add some relevant snippets

---

_Comment by @charliermarsh on 2025-01-08 21:06_

I think I have this figured out, will have a PR later today hopefully.

---

_Closed by @charliermarsh on 2025-01-08 22:12_

---

_Closed by @charliermarsh on 2025-01-08 22:12_

---
