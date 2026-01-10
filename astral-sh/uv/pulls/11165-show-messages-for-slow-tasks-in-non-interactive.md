```yaml
number: 11165
title: Show messages for slow tasks in non-interactive mode
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/non-interactive-progress
created_at: 2025-02-02T13:40:06Z
updated_at: 2025-02-06T02:28:59Z
url: https://github.com/astral-sh/uv/pull/11165
synced_at: 2026-01-10T11:10:34Z
```

# Show messages for slow tasks in non-interactive mode

---

_Pull request opened by @konstin on 2025-02-02 13:40_

When stderr is not a tty, we currently don't show any messages for build or large downloads, since indicatif is hidden. We can improve this by showing a message for:

* Starting and finishing a large download (>1MB)
* Starting and finishing a build

Downloads are limited to 1MB or unknown size to keep the logs concise and not scroll the entire terminal away for a download that finishes almost immediately.

These messages are not captured in the tests since their order is non-deterministic (downloads and builds race to finish).

There are no "tick" messages for large downloads yet, we could e.g. show an update on runnning downloads every n seconds.

Part of #11121

**Test Plan**

```
$ uv venv && FORCE_COLOR=1 cargo run -q pip install numpy --no-binary :all: --no-cache 2>&1 | tee a.txt
  Using CPython 3.13.0
  Creating virtual environment at: .venv
  Activate with: source .venv/bin/activate
  Resolved 1 package in 221ms
     Building numpy==2.2.2
        Built numpy==2.2.2
  Prepared 1 package in 2m 34s
  Installed 1 package in 6ms
   + numpy==2.2.2
```

![image](https://github.com/user-attachments/assets/f4b64313-afa7-449f-9e5b-2b1b7026bef3)


```
$ uv venv && FORCE_COLOR=1 cargo run -q pip install torch --no-cache 2>&1 | tee b.txt
  Using CPython 3.13.0
  Creating virtual environment at: .venv
  Activate with: source .venv/bin/activate
  Resolved 24 packages in 648ms
  Downloading setuptools (1.2MiB)
  Downloading nvidia-cuda-cupti-cu12 (13.2MiB)
  Downloading torch (731.1MiB)
  Downloading nvidia-nvjitlink-cu12 (20.1MiB)
  Downloading nvidia-cufft-cu12 (201.7MiB)
  Downloading nvidia-cuda-nvrtc-cu12 (23.5MiB)
  Downloading nvidia-curand-cu12 (53.7MiB)
  Downloading nvidia-nccl-cu12 (179.9MiB)
  Downloading nvidia-cudnn-cu12 (634.0MiB)
  Downloading nvidia-cublas-cu12 (346.6MiB)
  Downloading sympy (5.9MiB)
  Downloading nvidia-cusparse-cu12 (197.8MiB)
  Downloading nvidia-cusparselt-cu12 (143.1MiB)
  Downloading networkx (1.6MiB)
  Downloading nvidia-cusolver-cu12 (122.0MiB)
  Downloading triton (241.4MiB)
   Downloaded setuptools
   Downloaded networkx
   Downloaded sympy
   Downloaded nvidia-cuda-cupti-cu12
   Downloaded nvidia-nvjitlink-cu12
   Downloaded nvidia-cuda-nvrtc-cu12
   Downloaded nvidia-curand-cu12
[...]
```

![image](https://github.com/user-attachments/assets/71918d94-c5c0-44ce-bea8-aaba6cd80ef7)


---

_Label `enhancement` added by @konstin on 2025-02-02 13:40_

---

_@konstin reviewed on 2025-02-02 13:40_

---

_Review comment by @konstin on `crates/uv-git/src/source.rs`:123 on 2025-02-02 13:40_

This change avoids the start messages showing the long hash, while the finish message shows the short hash.

---

_@konstin reviewed on 2025-02-02 13:42_

---

_Review comment by @konstin on `crates/uv/src/commands/reporters.rs`:113 on 2025-02-02 13:42_

We could raise the errors here if we need.

---

_Renamed from "Show message in non-interactive mode" to "Show messages for slow tasks in non-interactive mode" by @konstin on 2025-02-02 16:10_

---

_@zanieb approved on 2025-02-05 23:37_

---

_Merged by @zanieb on 2025-02-05 23:38_

---

_Closed by @zanieb on 2025-02-05 23:38_

---

_Branch deleted on 2025-02-05 23:38_

---

_@charliermarsh reviewed on 2025-02-06 00:12_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/reporters.rs`:230 on 2025-02-06 00:12_

Should we not grab both of these with a single lock acquisition?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/reporters.rs`:180 on 2025-02-06 00:13_

Hmm, it's a bit strange that we hide these but _always_ show them if we don't have a size? I'd expect them to be always visible.

---

_@charliermarsh reviewed on 2025-02-06 00:13_

---

_Comment by @robinjhuang on 2025-02-06 01:38_

This is very helpful and adding tick messages would be even better! For large downloads like pytorch cuda, this would still seem like uv is stuck on downloading this step since it takes so long.

---

_@zanieb reviewed on 2025-02-06 02:18_

---

_Review comment by @zanieb on `crates/uv/src/commands/reporters.rs`:180 on 2025-02-06 02:18_

I don't quite follow your comment. You expect them not to be gated by size? (I sort of agree, since it's not in a tty anyway it seems fine to write them all) Or you expect them to not be gated by the multiprogress status?

---

_Comment by @zanieb on 2025-02-06 02:19_

> For large downloads like pytorch cuda, this would still seem like uv is stuck on downloading this step since it takes so long.

I will still insist a JSON output is the solution for this. I don't think we can deliver nice human readable tick messages — that's why we have progress bars in the interactive output.

---

_@charliermarsh reviewed on 2025-02-06 02:22_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/reporters.rs`:180 on 2025-02-06 02:22_

I expect them _not_ to be gated by size, i.e., always shown. (In the `else` branch, where we don't _have_ a size, we always show them, so that discrepancy seemed odd to me.)

---

_@zanieb reviewed on 2025-02-06 02:28_

---

_Review comment by @zanieb on `crates/uv/src/commands/reporters.rs`:180 on 2025-02-06 02:28_

Let's track in https://github.com/astral-sh/uv/issues/11272

---
