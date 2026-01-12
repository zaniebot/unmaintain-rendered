```yaml
number: 6217
title: Update Docker guide for projects
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/docker-project
created_at: 2024-08-19T17:31:56Z
updated_at: 2024-08-19T22:53:24Z
url: https://github.com/astral-sh/uv/pull/6217
synced_at: 2026-01-12T16:07:16Z
```

# Update Docker guide for projects

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/5371

---

_Label `documentation` added by @zanieb on 2024-08-19 17:31_

---

_Label `preview` added by @zanieb on 2024-08-19 17:31_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-19 17:32_

---

_Review requested from @konstin by @zanieb on 2024-08-19 17:32_

---

_Review comment by @mkniewallner on `docs/guides/integration/docker.md`:58 on 2024-08-19 22:31_

```suggestion
```dockerfile title="Dockerfile"
```

---

_Review comment by @mkniewallner on `docs/guides/integration/docker.md`:64 on 2024-08-19 22:31_

```suggestion
```dockerfile title="Dockerfile"
```

---

_Review comment by @mkniewallner on `docs/guides/integration/docker.md`:41 on 2024-08-19 22:32_

Maybe overkill, but if setting `WORKDIR` before `ADD`, we can copy in `./` without repeating `/app`:
```suggestion
# Set the current working directory to `/app`
WORKDIR /app

# Copy the project into the image current working directory
ADD . ./
```

---

_Review comment by @mkniewallner on `docs/guides/integration/docker.md`:59 on 2024-08-19 22:33_

Knowing that we set `WORKDIR` above, this could be simplified to:
```suggestion
RUN uv run some_script.py
```

---

_Merged by @zanieb on 2024-08-19 22:47_

---

_Closed by @zanieb on 2024-08-19 22:47_

---

_Branch deleted on 2024-08-19 22:47_

---

_@mkniewallner reviewed on 2024-08-19 22:48_

---

_Comment by @zanieb on 2024-08-19 22:50_

@mkniewallner thanks for the review!

---

_@zanieb reviewed on 2024-08-19 22:51_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:41 on 2024-08-19 22:51_

I think I'd write it the way I have now (personally) maybe it's a little clearer? 

---

_@mkniewallner reviewed on 2024-08-19 22:53_

---

_Review comment by @mkniewallner on `docs/guides/integration/docker.md`:41 on 2024-08-19 22:53_

Yep maybe better to be explicit here indeed.

---
