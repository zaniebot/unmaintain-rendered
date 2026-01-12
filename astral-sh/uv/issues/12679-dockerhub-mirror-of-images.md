```yaml
number: 12679
title: Dockerhub mirror of images
type: issue
state: open
author: shughes-uk
labels:
  - enhancement
assignees: []
created_at: 2025-04-04T19:04:01Z
updated_at: 2025-04-07T14:17:17Z
url: https://github.com/astral-sh/uv/issues/12679
synced_at: 2026-01-12T16:01:10Z
```

# Dockerhub mirror of images

---

_@shughes-uk_

### Summary

Is it possible astral could mirror the docker images for uv onto dockerhub?

This seems silly but we only have access to dockerhub through the corporate artifactory mirror. 

I suspect this is the case at a few F500 places where the github docker repo is still winding through years long approval processes.



### Example

_No response_

---

_Label `enhancement` added by @shughes-uk on 2025-04-04 19:04_

---

_Comment by @zanieb on 2025-04-04 23:04_

I could have sworn someone had asked for this before, but I can't find it.

I'm not particularly excited about mirroring the images, but if there are other requests where this is blocking usage I will consider it.

---

_Comment by @FishAlchemist on 2025-04-05 04:01_

@zanieb Is this it?
* https://github.com/astral-sh/uv/issues/8699

---

_Comment by @zanieb on 2025-04-05 14:35_

Ah classic — the space broke my GitHub search. Thanks!

---

_Comment by @shughes-uk on 2025-04-05 14:46_

Mine too it seems 😅

---

_Comment by @FishAlchemist on 2025-04-05 15:06_

Information like this that's been around for a while should theoretically be indexed by search engines, so I just used an AI with web search to find it.

The following image shows the results of ChatGPT's o3-mini using web search and reasoning.

![Image](https://github.com/user-attachments/assets/cfb1ac75-9368-46a7-9d7d-f95c1e1d2a86)

---

_Comment by @antdking on 2025-04-07 14:17_

For usage with Gitlab's built-in [Dependency Proxy](https://docs.gitlab.com/user/packages/dependency_proxy/), images currently need to be on dockerhub.

[Trivy opted](https://github.com/aquasecurity/trivy-db/issues/441#issuecomment-2406710186) to start publishing there after Github started enforcing some heavy rate limiting on container image pulls; though we've seen less issues with Github since moving to self-hosted Gitlab Runners.


---
