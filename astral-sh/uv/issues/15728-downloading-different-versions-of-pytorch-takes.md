```yaml
number: 15728
title: Downloading different versions of PyTorch takes varying amounts of time.
type: issue
state: closed
author: wz1114841863
labels:
  - question
assignees: []
created_at: 2025-09-08T03:44:00Z
updated_at: 2025-09-08T07:44:37Z
url: https://github.com/astral-sh/uv/issues/15728
synced_at: 2026-01-12T16:02:16Z
```

# Downloading different versions of PyTorch takes varying amounts of time.

---

_@wz1114841863_

### Question

When I specify a particular version of PyTorch (prior to 2.0), downloading with `uv` is very fast. However, when I use `uv pip install torch` to install the latest PyTorch (2.7.0), it takes an extremely long time to download and even encounters IO timeouts. I have to repeat the operation several times before it succeeds. Why is this happening? Is it due to my own network issues, or does the latest PyTorch include too many related libraries, or is there another problem? The error message is as follows: “ × Failed to download `nvidia-cudnn-cu12==9.10.2.21`
  ├─▶ Failed to extract archive: nvidia_cudnn_cu12-9.10.2.21-py3-none-manylinux_2_27_x86_64.whl
  ├─▶ I/O operation failed during extraction
  ╰─▶ Failed to download distribution due to network timeout. Try increasing UV_HTTP_TIMEOUT (current value: 30s).
  help: `nvidia-cudnn-cu12` (v9.10.2.21) was included because `torch` (v2.8.0) depends on `nvidia-cudnn-cu12`”，However, when running multiple times, even when only the torch (2.7.0) package remains, it still takes a very long time to download, with the download speed being quite slow.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @wz1114841863 on 2025-09-08 03:44_

---

_Comment by @konstin on 2025-09-08 06:52_

Are you using any specific index settings? Otherwise there should be no major different between the torch versions, it does sound like a network problem.

---

_Comment by @wz1114841863 on 2025-09-08 07:44_

Thanks. If no one else has encountered the same issue, it might indeed be a network problem. When I install torch's dependencies (such as nvidia_cudnn_cu12-9.10.2.21) independently, the download speed remains fast. However, when I attempt to install torch and its dependencies again directly, the network speed slows down significantly. This is genuinely perplexing.

---

_Closed by @wz1114841863 on 2025-09-08 07:44_

---
