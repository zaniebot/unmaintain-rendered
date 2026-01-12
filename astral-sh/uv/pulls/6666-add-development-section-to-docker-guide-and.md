```yaml
number: 6666
title: Add development section to Docker guide and reference new example project
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/docker-docs-new
created_at: 2024-08-27T01:42:51Z
updated_at: 2024-08-27T11:41:46Z
url: https://github.com/astral-sh/uv/pull/6666
synced_at: 2026-01-12T16:07:28Z
```

# Add development section to Docker guide and reference new example project

---

_@zanieb_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-08-27 01:42_

---

_Renamed from "Add volume mount content to Docker guide and improve layout" to "Add development section to Docker guide and reference new example project" by @zanieb on 2024-08-27 01:43_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-27 01:45_

---

_Review requested from @konstin by @zanieb on 2024-08-27 01:45_

---

_Review comment by @konstin on `docs/guides/integration/docker.md`:170 on 2024-08-27 07:31_

https://docs.docker.com/compose/compose-application-model/#the-compose-file:

> The default path for a Compose file is compose.yaml (preferred) or compose.yml that is placed in the working directory. Compose also supports docker-compose.yaml and docker-compose.yml for backwards compatibility of earlier versions. If both files exist, Compose prefers the canonical compose.yaml.

huh TIL; i've never seen anything but `docker-compose.yml` being used.

---

_Review comment by @konstin on `docs/guides/integration/docker.md`:186 on 2024-08-27 08:57_

We should add a note that watch is a very recent feature, i had to switch from the default ubuntu 24.04 installation to the PPA to be able to run `docker compose up --watch`

---

_@konstin approved on 2024-08-27 10:11_

---

_@zanieb reviewed on 2024-08-27 11:06_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:170 on 2024-08-27 11:06_

(I'll switch to `yaml`)

---

_@zanieb reviewed on 2024-08-27 11:07_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:186 on 2024-08-27 11:07_

October 2023 :D yeah makes sense https://www.docker.com/blog/announcing-docker-compose-watch-ga-release/

---

_Merged by @zanieb on 2024-08-27 11:41_

---

_Closed by @zanieb on 2024-08-27 11:41_

---

_Branch deleted on 2024-08-27 11:41_

---
