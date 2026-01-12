```yaml
number: 16280
title: Add auto-detection for Intel GPU on Windows
type: pull_request
state: merged
author: guangyey
labels:
  - enhancement
assignees: []
merged: true
base: main
head: guangyey/intel_win
created_at: 2025-10-13T11:56:17Z
updated_at: 2025-10-17T02:31:25Z
url: https://github.com/astral-sh/uv/pull/16280
synced_at: 2026-01-12T16:12:12Z
```

# Add auto-detection for Intel GPU on Windows

---

_@guangyey_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR enables `--torch-backend=auto` to automatically detect Intel GPUs. It follows up on [#14386](https://github.com/astral-sh/uv/pull/14386).
On Windows, detection is implemented by querying the `Win32_VideoController` class via the [WMI crate](https://github.com/ohadravid/wmi-rs/tree/v0.16.0).

Currently, Intel GPUs (XPU) do not depend on specific driver or toolkit versions to determine which PyTorch wheel to use.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Using `uv pip install torch --torch-backend=auto` could install torch-xpu successfully when Intel GPU is detected.

<img width="906" height="745" alt="image" src="https://github.com/user-attachments/assets/d5eac4b4-0d09-40d5-83d7-4c645a6cfa25" />



---

_Converted to draft by @guangyey on 2025-10-13 11:56_

---

_Renamed from "Add auto-detection for Intel GPU on Windows" to "[WIP] Add auto-detection for Intel GPU on Windows" by @guangyey on 2025-10-13 11:56_

---

_Marked ready for review by @guangyey on 2025-10-14 13:23_

---

_Renamed from "[WIP] Add auto-detection for Intel GPU on Windows" to "Add auto-detection for Intel GPU on Windows" by @guangyey on 2025-10-14 13:23_

---

_Comment by @guangyey on 2025-10-14 13:24_

@charliermarsh and @geofft could you please help review this PR when you have time.

---

_Review requested from @charliermarsh by @konstin on 2025-10-14 13:38_

---

_Review requested from @geofft by @konstin on 2025-10-14 13:38_

---

_Label `enhancement` added by @konstin on 2025-10-15 18:45_

---

_Comment by @guangyey on 2025-10-16 07:39_

@charliermarsh and @geofft May I know if you could help review this PR.

---

_Comment by @konstin on 2025-10-16 14:19_

Please have patience with the uv team, we only have limited resources for reviewing complex PRs.

---

_Comment by @guangyey on 2025-10-16 14:42_

> Please have patience with the uv team, we only have limited resources for reviewing complex PRs.

Totally understand, thanks for taking the time to review! Really appreciate the efforts from the uv team.

---

_Label `no-build` added by @geofft on 2025-10-16 20:05_

---

_Label `no-test` added by @geofft on 2025-10-16 20:05_

---

_@konstin reviewed on 2025-10-16 20:29_

---

_Review comment by @konstin on `crates/uv-torch/src/accelerator.rs`:9 on 2025-10-16 20:29_

nit: this should be `#[cfg(windows)]`

---

_Comment by @geofft on 2025-10-16 20:55_

Thanks, code looks good to me! As discussed in https://github.com/astral-sh/uv/pull/14386#issuecomment-3031706334, we _will_ grab the XPU version on machines with an Intel iGPU that isn't actually useful for GPGPU work, but we said that's probably okay because this version still has CPU support, and detecting other types of GPUs takes precedence.

I manually tested this a little bit and the behavior is what I expect. (I tested on two EC2 VMs by modifying the code to match on Microsoft's PCI ID instead of Intel's, and on the one without an additional GPU it downloads the XPU version, and on the one that has an NVIDIA T4 it downloads the CUDA version.)

If anyone in the future feels like we should not download the XPU version on machines with insufficiently powerful iGPUs, we can definitely iterate on the behavior, please report a new issue and tag me.

---

_Merged by @geofft on 2025-10-16 20:56_

---

_Closed by @geofft on 2025-10-16 20:56_

---

_Label `no-build` removed by @geofft on 2025-10-16 20:56_

---

_Label `no-test` removed by @geofft on 2025-10-16 20:56_

---

_Comment by @guangyey on 2025-10-17 02:24_

@geofft  Thank you so much for the thorough testing and clear explanation! Really appreciate your detailed validation and feedback. Please feel free to tag me if there’s anything I can help with.

---

_Comment by @charliermarsh on 2025-10-17 02:25_

Thanks @guangyey, appreciate the contribution!

---

_Comment by @guangyey on 2025-10-17 02:31_

@charliermarsh Thanks! Happy to contribute. It’s been an interesting and rewarding experience.

---
