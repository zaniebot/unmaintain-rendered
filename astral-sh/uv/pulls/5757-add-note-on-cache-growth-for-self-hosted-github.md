```yaml
number: 5757
title: Add note on cache growth for self-hosted GitHub runners
type: pull_request
state: merged
author: samypr100
labels:
  - documentation
assignees: []
merged: true
base: main
head: caching-hosted-runner
created_at: 2024-08-04T04:53:02Z
updated_at: 2024-09-18T22:25:16Z
url: https://github.com/astral-sh/uv/pull/5757
synced_at: 2026-01-12T16:06:59Z
```

# Add note on cache growth for self-hosted GitHub runners

---

_@samypr100_

## Summary

Related discussion: #5731

This adds a warning section for caching on non-ephemeral (e.g. ec2) self hosted github runners.

## Test Plan

Prettier was ran on the file.

---

_Marked ready for review by @samypr100 on 2024-08-04 05:09_

---

_Assigned to @zanieb by @zanieb on 2024-08-05 13:54_

---

_Review requested from @zanieb by @konstin on 2024-08-06 08:17_

---

_Comment by @samypr100 on 2024-08-23 12:07_

Rendered

<img width="381" alt="self_hosted_runners" src="https://github.com/user-attachments/assets/166cf7fa-a8f3-4051-bd56-defb5668a93c">


---

_@zanieb reviewed on 2024-09-17 04:09_

---

_Review comment by @zanieb on `docs/guides/integration/github.md`:289 on 2024-09-17 04:09_

```suggestion
    ```sh title="clean-workspace.sh"
```

---

_@zanieb reviewed on 2024-09-17 04:10_

---

_Review comment by @zanieb on `docs/guides/integration/github.md`:283 on 2024-09-17 04:10_

What is being referred to here?

---

_@zanieb reviewed on 2024-09-17 04:12_

---

_Review comment by @zanieb on `docs/guides/integration/github.md`:275 on 2024-09-17 04:12_

```suggestion
    When using non-ephemeral self-hosted runners and the default cache directory, the cache can grow over time 
    and become problematic. In such cases, it's recommend to move the cache to inside the
```
```

---

_@samypr100 reviewed on 2024-09-17 12:03_

---

_Review comment by @samypr100 on `docs/guides/integration/github.md`:283 on 2024-09-17 12:03_

Good question, it was a callout to the previous section https://docs.astral.sh/uv/guides/integration/github/#caching

---

_@zanieb reviewed on 2024-09-17 13:46_

---

_Review comment by @zanieb on `docs/guides/integration/github.md`:283 on 2024-09-17 13:46_

But would you actually restore the cache in this situation? My understanding is the cache is deleted at the end of each job so what is there to persist and restore? (This confusion has been why this pull request has been lingering, sorry!)

---

_@samypr100 reviewed on 2024-09-17 16:00_

---

_Review comment by @samypr100 on `docs/guides/integration/github.md`:283 on 2024-09-17 16:00_

Ah I see. That was meant to be optional step (if desired to use github actions cache for a pre-warmed cache). We can easily remove it if its confusing.

---

_@zanieb reviewed on 2024-09-17 16:31_

---

_Review comment by @zanieb on `docs/guides/integration/github.md`:283 on 2024-09-17 16:31_

```suggestion
```

---

_Label `documentation` added by @zanieb on 2024-09-18 13:33_

---

_Renamed from "docs: add warning note on self-hosted github runners" to "Add note on cache growth for self-hosted runners" by @zanieb on 2024-09-18 13:33_

---

_Renamed from "Add note on cache growth for self-hosted runners" to "Add note on cache growth for self-hosted GitHub runners" by @zanieb on 2024-09-18 13:34_

---

_@zanieb approved on 2024-09-18 13:34_

---

_Merged by @zanieb on 2024-09-18 13:34_

---

_Closed by @zanieb on 2024-09-18 13:34_

---

_@samypr100 reviewed on 2024-09-18 18:51_

---

_Review comment by @samypr100 on `docs/guides/integration/github.md`:291 on 2024-09-18 18:51_

@zanieb I noticed this was changed 😅  This happens at the runner level and not at the job level, so uv will not be at the path (unless installed globally and not via setup-uv) nor the job env vars 😁
Hence why it was `rm -rf $GITHUB_WORKSPACE/*` before

---

_Branch deleted on 2024-09-18 22:25_

---
