```yaml
number: 14743
title: Directly ask the NVIDIA kernel driver what version it is
type: pull_request
state: open
author: geofft
labels:
  - no-build
  - no-test
assignees: []
draft: true
base: main
head: geofft/nvidiactl-ioctl
created_at: 2025-07-19T06:50:04Z
updated_at: 2025-08-07T16:45:27Z
url: https://github.com/astral-sh/uv/pull/14743
synced_at: 2026-01-12T16:11:23Z
```

# Directly ask the NVIDIA kernel driver what version it is

---

_@geofft_

This is the same API called by `nvidia-smi`, but doing it ourselves is both much faster and more robust to installation conditions (e.g., nvidia-smi not being installed).

---

_Review requested from @charliermarsh by @geofft on 2025-07-19 06:50_

---

_Comment by @geofft on 2025-07-19 06:52_

Obviously this has a big "for amusement purposes only" sign on it and needs a bunch of cleanup, but it does work,.

Tested with
```
geofft@mercer:~$ busybox unshare -Urm
root@mercer:~# mount -t tmpfs tmpfs /proc/driver/
root@mercer:~# mount -t tmpfs tmpfs /sys/module/
root@mercer:~# ./uv -v pip compile --torch-backend=auto <(echo torch==0)
DEBUG uv 0.7.21+23 (a8bb7be52 2025-07-17)
DEBUG Starting Python discovery for a default Python
DEBUG Looking for exact match for request a default Python
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Using Python 3.12.3 interpreter at /usr/bin/python3 for builds
DEBUG Imma firin ma ioctl
DEBUG Hey, that worked!
DEBUG Detected CUDA driver version from `/dev/nvidiactl`: 570.133.7
[...]
```

---

_Label `no-build` added by @geofft on 2025-07-19 06:53_

---

_Label `no-test` added by @geofft on 2025-07-19 06:53_

---

_Comment by @charliermarsh on 2025-08-02 13:05_

Was the conclusion that this can also return the SM, or no?

---

_Comment by @geofft on 2025-08-02 17:34_

> Was the conclusion that this can also return the SM, or no?

Not directly, but it's feasible to do it indirectly.

The kernel driver has [an operation that can return an "architecture"](https://github.com/NVIDIA/open-gpu-kernel-modules/blob/575.64.03/src/common/sdk/nvidia/inc/ctrl/ctrl2080/ctrl2080mc.h), e.g., TU102, GA100, etc. Wikipedia refers to these as "code names" or "dies" or "chips". A particular architecture might be used in multiple products, for instance, the GA102 is used in the A40 datacenter GPU and in the non-laptop versions of the GeForce RTX 3080 and 3090 GPUs (among others).

I believe that the mapping from an architecture to a compute capability (aka "SM") is a well-defined function, i.e., every device that uses the same architecture has the same compute capability. I also believe that `libcuda.so` internally has a mapping from architecture to compute capability, and uses that to implement `cuDeviceGetAttribute(CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MAJOR/MINOR)`. But I don't think there is a public table for doing that conversion.

It's not very hard to construct a table from public sources - the [pci.ids file](https://pci-ids.ucw.cz/) tends to list the architecture followed by the product name:
```
$ grep AD104GL /usr/share/misc/pci.ids 
        27b0  AD104GL [RTX 4000 SFF Ada Generation]
        27b1  AD104GL [RTX 4500 Ada Generation]
        27b2  AD104GL [RTX 4000 Ada Generation]
        27b6  AD104GL [L2]
        27b7  AD104GL [L16]
        27b8  AD104GL [L4]
        27ba  AD104GLM [RTX 4000 Ada Generation Laptop GPU]
        27bb  AD104GLM [RTX 3500 Ada Generation Laptop GPU]
        27fa  AD104GLM [RTX 4000 Ada Generation Embedded GPU]
        27fb  AD104GLM [RTX 3500 Ada Generation Embedded GPU]
```
and you can cross-reference this against the public [table of compute capabilities](https://developer.nvidia.com/cuda-gpus) which has product names.

You can also go straight from the product name in pci.ids to compute capability, of course. (And you can query the PCI ID of the device through the ioctl interface, so you can take this approach in cases where you don't get to see the host sysfs, e.g. gVisor and I think WSL2). But the advantage of using the "architecture" here is that it will support new models of the same GPU (like increased GPU RAM, etc.) that the pci.ids file might not yet know about. For instance, the L2 and L16 aren't listed on that web page, only the L4 is.

It won't support brand new architectures / compute capabilities, though.

https://gist.github.com/geofft/9d63bb7c32b926ed492b46dbe2a7ee44 is a Python script to parse the pci.ids file and the NVIDIA web pages and construct a two-level mapping from PCI ID to architecture to compute capability. (I actually wrote this code intending to just start from PCI IDs, but it was helpful to use architecture to fill in the gaps.)

I asked in https://github.com/astral-sh/uv/issues/14664#issuecomment-3120904133 if NVIDIA can provide their version of the lookup table publicly, which would probably be more complete.

I also have a theory that you could get this in some other way like asking the GPU to launch a kernel compiled for a particular compute capability and seeing how it feels about it. I haven't explored that route, and probably the performance would be poor.

---

_Comment by @geofft on 2025-08-07 16:45_

Actually - I just saw [ctrl2080gr.h](https://github.com/NVIDIA/open-gpu-kernel-modules/blob/575.64.03/src/common/sdk/nvidia/inc/ctrl/ctrl2080/ctrl2080gr.h) in the same directory, which seems to have a `NV2080_CTRL_GR_INFO_INDEX_SM_VERSION` query returning plausible responses like `NV2080_CTRL_GR_INFO_SM_VERSION_9_00` - and the values of those constants appear to be two-byte major and minor. I can give that a shot at some point!

(Serves me right for grepping for things like "compute capability" and not "SM")

---
