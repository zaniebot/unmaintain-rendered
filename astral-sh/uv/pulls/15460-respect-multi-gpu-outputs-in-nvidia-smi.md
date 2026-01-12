```yaml
number: 15460
title: Respect multi-GPU outputs in nvidia-smi
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/multi
created_at: 2025-08-22T17:27:50Z
updated_at: 2025-11-02T21:21:45Z
url: https://github.com/astral-sh/uv/pull/15460
synced_at: 2026-01-12T16:11:45Z
```

# Respect multi-GPU outputs in nvidia-smi

---

_@charliermarsh_

## Summary

This initially included `NVIDIA_VISIBLE_DEVICES` masking, though it's now omitted for simplicity.

Closes https://github.com/astral-sh/uv/issues/14647.


---

_Review requested from @geofft by @charliermarsh on 2025-08-22 17:27_

---

_Label `bug` added by @charliermarsh on 2025-08-22 17:27_

---

_Marked ready for review by @charliermarsh on 2025-08-22 17:28_

---

_@geofft approved on 2025-10-09 19:37_

This seems fine, but some nitpicks:

1) I don't actually think there's a point to us parsing `NVIDIA_VISIBLE_DEVICES` here. This variable is specifically used by nvidia-container-toolkit to determine which devices ought to be exposed inside the container. It doesn't seem to be something that's used in non-container tools at all. I think the reference to it in #14647 was just mentioning the lack of ability to use this as a workaround for being unable to parse multiple lines, but if we handle multiple lines I'm not sure we need the workaround. It doesn't really matter what we do here since the driver version ought to be the same for all lines of output, but if we extend this code to be about compute capabilities etc., I think we should put a tad more thought into whether we want this to be the interface, since I think we would be novel in using this environment variable in a non-container tool (e.g. I don't think that nvidia-variant-provider uses it).
2) Somewhat weirdly the parsing for the variable in nvidia-container-toolkit appears to allow `all`/`none`/`void` to be _individual elements_ in the comma-separated list, as opposed to having to be the entire string, e.g., `NVIDIA_VISIBLE_DEVICES=2,all` is accepted and interpreted as `all`. See https://github.com/NVIDIA/nvidia-container-toolkit/blob/v1.17.8/internal/config/image/cuda_image.go#L123-L154 which splits on commas and passes a list to https://github.com/NVIDIA/nvidia-container-toolkit/blob/v1.17.8/internal/config/image/devices.go which loops through the list looking for these special keywords.

---

_Comment by @charliermarsh on 2025-11-02 21:05_

Okay, sounds good. I removed the `NVIDIA_VISIBLE_DEVICES` parsing for now.

---

_Merged by @charliermarsh on 2025-11-02 21:21_

---

_Closed by @charliermarsh on 2025-11-02 21:21_

---

_Branch deleted on 2025-11-02 21:21_

---
