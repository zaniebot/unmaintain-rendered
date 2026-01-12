```yaml
number: 14386
title: Add auto-detection for Intel GPUs
type: pull_request
state: merged
author: guangyey
labels:
  - enhancement
assignees: []
merged: true
base: main
head: guangyey/uv
created_at: 2025-07-01T07:17:41Z
updated_at: 2025-07-10T02:00:32Z
url: https://github.com/astral-sh/uv/pull/14386
synced_at: 2026-01-12T16:11:12Z
```

# Add auto-detection for Intel GPUs

---

_@guangyey_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR intends to enable `--torch-backend=auto` to detect Intel GPUs automatically:
 - On Linux, detection is performed using the `lspci` command via `Display controller` id.
 - On Windows, ~~detection is done via a `powershell` query to `Win32_VideoController`~~. Skip support for now—revisit once a better solution is available.

Currently, Intel GPUs (XPU) do not rely on specific driver or toolkit versions to distribute different PyTorch wheels.

## Test Plan

<!-- How was it tested? -->
On Linux:
![image](https://github.com/user-attachments/assets/f7f238e3-a797-42ea-b8fa-9b028dfd4db5)
~~On Windows:
![image](https://github.com/user-attachments/assets/a10d774e-1cb9-431b-bb85-e3e8225df98f)~~

---

_Renamed from "Add auto-detection for Intel GPUs" to "[WIP] Add auto-detection for Intel GPUs" by @guangyey on 2025-07-01 07:18_

---

_Comment by @konstin on 2025-07-01 09:28_

Do we need to spawn a `std::process::Command` to check this or is there e.g. a file we could read that has the same information?

---

_Review requested from @charliermarsh by @konstin on 2025-07-01 09:29_

---

_Label `enhancement` added by @konstin on 2025-07-01 09:29_

---

_Comment by @guangyey on 2025-07-01 09:53_

> Do we need to spawn a `std::process::Command` to check this or is there e.g. a file we could read that has the same information?

OK, I will try it.

---

_Comment by @konstin on 2025-07-01 09:56_

If you have links to websites (e.g. from Intel) about how to detect these cards, please share them, it helps with reviewing this code.

---

_Converted to draft by @guangyey on 2025-07-01 10:00_

---

_Marked ready for review by @guangyey on 2025-07-02 10:16_

---

_Renamed from "[WIP] Add auto-detection for Intel GPUs" to "Add auto-detection for Intel GPUs" by @guangyey on 2025-07-02 10:17_

---

_Comment by @guangyey on 2025-07-02 10:22_

> Do we need to spawn a `std::process::Command` to check this or is there e.g. a file we could read that has the same information?

At the moment, we lack a stable and consistent method to retrieve Intel driver or GPU details from a file. So I intend to use system calls to keep the approach stable across platforms. May I know if it is reasonable and acceptable to you @konstin @charliermarsh 

---

_Review requested from @geofft by @zanieb on 2025-07-02 14:52_

---

_Comment by @geofft on 2025-07-02 15:34_

Overall adding this detection seems good, but I agree with avoiding the subprocess.

On the Linux side, you can do this by using sysfs (which is what lspci is going to do anyway), here's some very rough Python for it that I'll let someone else convert to Rust 😄:

```python
import pathlib

PCI_BASE_CLASS_MASK = 0xff0000
PCI_BASE_CLASS_DISPLAY = 0x030000
PCI_VENDOR_ID_INTEL = 0x8086

def has_intel_graphics():
    try:
        for device in pathlib.Path("/sys/bus/pci/devices").iterdir():
            try:
                pci_class = int((device / "class").read_text(), 16)
                vendor = int((device / "vendor").read_text(), 16)
            except IOError:
                continue # maybe log something?
            if (pci_class & PCI_BASE_CLASS_MASK == PCI_BASE_CLASS_DISPLAY) and (vendor == PCI_VENDOR_ID_INTEL):
                return True
    except IOError:
        pass # maybe log something?
    return False
```

The Windows side looks like it's using WMI and I would expect there's an equivalent API approach for this, too, to save the subprocess.

Maybe the [pci-info crate](https://docs.rs/pci-info/) is helpful? It claims to be cross platform and probably looking at PCI is correct on Windows too?

(If we absolutely must go with lspci, let's use `lspci -m -n -d 8086::03xx` to get machine-parseable output and have lspci do the filtering, instead of using a shell and grep. Among other things, the current code matches the word "Intelligent" which shows up in various non-Intel product names in the PCI DB.)

---

_Comment by @geofft on 2025-07-02 15:55_

Also - should we be trying to do something to detect whether we have an Intel GPU actually capable of interesting GPGPU work? I assume a lot of users will have a very boring Intel integrated GPU, and this change as written will switch them to the XPU download instead of the CPU one because we'll see an Intel-brand display device. Will they actually benefit from the XPU download?

I saw that WMI has some info on whether "accelerators" are present, is it worth trying to parse that? Or should we look for whether `/dev/dri*/render` is present and accessible to the current user? Or should we compare against the list of PCI device IDs at https://dgpu-docs.intel.com/devices/hardware-table.html?

---

_Comment by @geofft on 2025-07-02 16:20_

OK, from a bit of very quick poking - I think the only useful info WMI gives us is the (human-readable) device name and a parseable string with PCI vendor/device IDs. So if we want to avoid string-matching `Intel`, we might as well look at PCI info... can we try using the `pci-info` crate?

(It appears that `pci-info` has two backends for Windows, "SetupAPI" and WMI. I don't know what the benefits of each are, but WMI is an optional crate feature and not used by default so I assume it's a heavier-weight dependency. In either case let's keep an eye on performance and binary size.)

By the way, what's a good way to test this? Intel Tiber AI Cloud? (AWS/Google/etc. don't have Intel GPUs, do they?)

---

_Comment by @guangyey on 2025-07-03 10:15_

@geofft Thanks for your suggestion. I followed your code to change the Linux detection pass instead of using subprocess. And change Windows pass to match vendor id instead of string `Intel`, which is easy to mismatch with `intelligent`.
>>> Also - should we be trying to do something to detect whether we have an Intel GPU actually capable of interesting GPGPU work? I assume a lot of users will have a very boring Intel integrated GPU, and this change as written will switch them to the XPU download instead of the CPU one because we'll see an Intel-brand display device. Will they actually benefit from the XPU download?

That’s a fair concern. Right now the detection is fairly coarse. Some old iGPU are widely available and easily accessible, which gives community users a great oppotunity to learn and experiment with the GPU-enabled version of PyTorch. In addition, some newer iGPU are actually quite powerful (e.g. LNL), which even outperform certain discrete GPUs, and users can benefit from real performance gains.
Also the XPU version of PyTorch runs perfectly fine on CPUs, so there's no downside there. Since we only fall back to detecting Intel GPUs after checking NVDIA and ROCm, this logic shouldn't interfere with users on those platforms. Perhaps, we could go with this apporach for now and adjust the detection logic later basd on real feedback.

>>> can we try using the pci-info crate

TBH, I'm still a Rust beginner, so integrating `pci-info` might take some time. Also, if you insist on preferring `pci-info`, I will try it. Should we consider using it for both Linux and Windows to handle detection consistently?

>>> By the way, what's a good way to test this? Intel Tiber AI Cloud? (AWS/Google/etc. don't have Intel GPUs, do they?)

Yes, the better way is test it in iGPU, which is more widely available.





---

_Comment by @guangyey on 2025-07-03 10:23_

BTW, the windows CI failure is irrelevant to this PR, right?

---

_Comment by @guangyey on 2025-07-04 03:40_

> BTW, the windows CI failure is irrelevant to this PR, right?

I forgot to update `Cargo.lock`, which caused the Clippy check to fail on Windows. Rust is smart enough to catch it — I'm really starting to love this language!

---

_@geofft approved on 2025-07-04 14:14_

OK, this seems fine to me, at least on the Linux side - I'd appreciate if someone else can form an opinion on the Windows side on e.g. whether we can assume it's safe to run `powershell`, but the logic seems plausible there too.

Thanks for the updates and for contributing this!

---

_Comment by @charliermarsh on 2025-07-04 16:02_

Is it important that we support this on Windows for now? I’m hesitant to merge code that relies on a PowerShell invocation.

---

_Comment by @guangyey on 2025-07-06 06:21_

@charliermarsh @geofft I completely understand your concerns regarding Windows. You're absolutely right — relying on `shellpower` doesn’t seem like an elegant or robust solution, and it's reasonable that we shouldn't put that burden on `uv`. Respecting your perspective, I agree that for now we should limit Intel GPU auto-detection to Linux only, and revisit Windows support once we have a cleaner and more reliable approach. Do you think this is reasonable?

Removed the code for Windows support and highlighted the limitation on the docs.

---

_@charliermarsh reviewed on 2025-07-08 20:16_

---

_Review comment by @charliermarsh on `crates/uv-torch/src/accelerator.rs`:176 on 2025-07-08 20:16_

Is this different than using `fs::read_dir`, since we're only going one depth?

---

_@charliermarsh reviewed on 2025-07-08 20:37_

---

_Review comment by @charliermarsh on `crates/uv-torch/src/accelerator.rs`:176 on 2025-07-08 20:37_

(Apart from this, the rest of the change looks good and I'm happy to merge once resolved.)

---

_Review comment by @guangyey on `crates/uv-torch/src/accelerator.rs`:176 on 2025-07-09 01:21_

CI clippy told me `fs::read_dir` is disallowed in `uv`, please refer to https://github.com/astral-sh/uv/blob/5e2dc5a9aa18aaa942f26513ca3b8ae4704eb018/clippy.toml#L21-L29

---

_@guangyey reviewed on 2025-07-09 01:21_

---

_@charliermarsh reviewed on 2025-07-09 01:54_

---

_Review comment by @charliermarsh on `crates/uv-torch/src/accelerator.rs`:176 on 2025-07-09 01:54_

Ohh, it wants you to use `fs_err::read_dir`.

---

_@guangyey reviewed on 2025-07-09 03:00_

---

_Review comment by @guangyey on `crates/uv-torch/src/accelerator.rs`:176 on 2025-07-09 03:00_

@charliermarsh Thanks for the suggestion! I’ve switched to using `fs_err::read_dir` as recommended. Both the CI and local checks now pass.
![image](https://github.com/user-attachments/assets/c5c05406-b2af-498d-ab88-028606a0a014)


---

_@charliermarsh approved on 2025-07-09 13:19_

Thank you!

---

_Merged by @charliermarsh on 2025-07-09 13:31_

---

_Closed by @charliermarsh on 2025-07-09 13:31_

---

_Comment by @guangyey on 2025-07-10 02:00_

Happy to help. Thanks for your review and feedback.

---
