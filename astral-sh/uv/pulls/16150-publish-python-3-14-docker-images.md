```yaml
number: 16150
title: Publish Python 3.14 Docker Images
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
assignees: []
merged: true
base: main
head: py314-docker
created_at: 2025-10-07T14:03:30Z
updated_at: 2025-10-07T23:10:11Z
url: https://github.com/astral-sh/uv/pull/16150
synced_at: 2026-01-12T16:12:08Z
```

# Publish Python 3.14 Docker Images

---

_@samypr100_

## Summary

Tentative PR to drop the `-rc` suffix from the published images.

Do not merge this yet until the formal release [upstream](https://hub.docker.com/_/python) :) 


---

_Renamed from "Use Python 3.14 Docker Images" to "Publish Python 3.14 Docker Images" by @samypr100 on 2025-10-07 14:14_

---

_@zanieb approved on 2025-10-07 14:49_

Thank you!

---

_Comment by @zanieb on 2025-10-07 14:49_

Oh, when will these images come through upstream?

---

_Label `enhancement` added by @konstin on 2025-10-07 14:50_

---

_Comment by @samypr100 on 2025-10-07 14:53_

> Oh, when will these images come through upstream?

Should be today, they're often release around the same time as the announcement. I'm keeping a look and will mark the PR as ready when they're live

---

_Comment by @zanieb on 2025-10-07 15:13_

Cool thanks, we're coordinating a 3.14 release today so hopefully this can merge before then

---

_Comment by @zanieb on 2025-10-07 16:11_

See https://github.com/docker-library/python/pull/1083

---

_Comment by @samypr100 on 2025-10-07 18:15_

> See [docker-library/python#1083](https://github.com/docker-library/python/pull/1083)

The update to https://github.com/docker-library/official-images is still pending. Now that the builds have finished, I think it should be a matter of time. https://github.com/docker-library/faq#an-images-source-changed-in-git-now-what

---

_Comment by @zanieb on 2025-10-07 18:32_

Thanks :D I just pushed optimistically

---

_Comment by @MatthijsKok on 2025-10-07 21:22_

@zanieb upstream tags are published! 🎉 

EDIT: sorry for getting your hopes up... seems like the tags are available, but the docker infra is still in the process of building the layer caches.  
See:
- https://doi-janky.infosiftr.net/job/meta/job/riscv64/job/build/15922/
- https://github.com/docker-library/faq?tab=readme-ov-file#an-images-source-changed-in-git-now-what

It's taking a bit because the infra job queue hasn't caught up on some more niche platforms  
https://doi-janky.infosiftr.net/computer/multiarch%2Ds390x/load-statistics

---

_Marked ready for review by @samypr100 on 2025-10-07 21:30_

---

_Merged by @zanieb on 2025-10-07 23:10_

---

_Closed by @zanieb on 2025-10-07 23:10_

---

_Branch deleted on 2025-10-07 23:10_

---
