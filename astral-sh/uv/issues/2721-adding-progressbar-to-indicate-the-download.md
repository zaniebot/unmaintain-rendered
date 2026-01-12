```yaml
number: 2721
title: "Adding progressbar to indicate the download status and ETA especially for packages with >=10MB"
type: issue
state: closed
author: detrin
labels:
  - duplicate
assignees: []
created_at: 2024-03-28T21:38:22Z
updated_at: 2024-03-28T22:15:50Z
url: https://github.com/astral-sh/uv/issues/2721
synced_at: 2026-01-12T15:58:40Z
```

# Adding progressbar to indicate the download status and ETA especially for packages with >=10MB

---

_@detrin_

I started using uv as it is neat and faster than pip. One feature that I miss from pip is the progressbar to indicate what part of the wheel was downloaded and what is the ETA. The progressbar has little to no value added except it is nice to watch of course. The ETA is useful, consider packages such as polars, or torch that have rather huge wheels andit can take a while to download. Sometimes my connection is poor (down to like 300 KB/s) and while those packages are downloading I use that time for something else, so it is nice to have some ETA on that. 

Would you consider adding such a feature into uv?


---

_Comment by @zanieb on 2024-03-28 22:15_

Hi! This is a duplicate of #1209 

We're interested but it's non-trivial to show the progress bars when we do concurrent operations. I'm going to close this in favor of the original issue.

---

_Closed by @zanieb on 2024-03-28 22:15_

---

_Label `duplicate` added by @zanieb on 2024-03-28 22:15_

---
