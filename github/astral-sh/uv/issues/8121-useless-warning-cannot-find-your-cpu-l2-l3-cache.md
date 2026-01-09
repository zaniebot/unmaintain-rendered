---
number: 8121
title: "Useless Warning: cannot find your CPU L2 & L3 cache size in /sys/devices/system/cpu/cpuX/cache"
type: issue
state: closed
author: dimaqq
labels: []
assignees: []
created_at: 2024-10-11T05:08:17Z
updated_at: 2025-06-05T10:30:18Z
url: https://github.com/astral-sh/uv/issues/8121
synced_at: 2026-01-07T13:12:17-06:00
---

# Useless Warning: cannot find your CPU L2 & L3 cache size in /sys/devices/system/cpu/cpuX/cache

---

_Issue opened by @dimaqq on 2024-10-11 05:08_

I get this warning on every uvx [command] run in a VM:

`Warning: cannot find your CPU L2 & L3 cache size in /sys/devices/system/cpu/cpuX/cache`

System setup up:
Host: macos arm64
Virt: vz
Guest: colima 6.8.0-31-generic aarch64; Ubuntu 24.04

I doubt that this warning is particularly useful to the end users anyway. Maybe it can be removed?

If it matters, here's my /proc:
```
# ls /sys/devices/system/cpu/
cpu0/  cpu2/  cpu4/  cpu6/  cpufreq/  hotplug/  kernel_max  nohz_full  online    power/   smt/    vulnerabilities/
cpu1/  cpu3/  cpu5/  cpu7/  cpuidle/  isolated  modalias    offline    possible  present  uevent

# find /sys/devices/system/cpu/cpu0/cache/
/sys/devices/system/cpu/cpu0/cache/
/sys/devices/system/cpu/cpu0/cache/uevent
/sys/devices/system/cpu/cpu0/cache/index2
/sys/devices/system/cpu/cpu0/cache/index2/uevent
/sys/devices/system/cpu/cpu0/cache/index2/shared_cpu_list
/sys/devices/system/cpu/cpu0/cache/index2/type
/sys/devices/system/cpu/cpu0/cache/index2/level
/sys/devices/system/cpu/cpu0/cache/index2/shared_cpu_map
/sys/devices/system/cpu/cpu0/cache/index0
/sys/devices/system/cpu/cpu0/cache/index0/uevent
/sys/devices/system/cpu/cpu0/cache/index0/shared_cpu_list
/sys/devices/system/cpu/cpu0/cache/index0/type
/sys/devices/system/cpu/cpu0/cache/index0/level
/sys/devices/system/cpu/cpu0/cache/index0/shared_cpu_map
/sys/devices/system/cpu/cpu0/cache/index1
/sys/devices/system/cpu/cpu0/cache/index1/uevent
/sys/devices/system/cpu/cpu0/cache/index1/shared_cpu_list
/sys/devices/system/cpu/cpu0/cache/index1/type
/sys/devices/system/cpu/cpu0/cache/index1/level
/sys/devices/system/cpu/cpu0/cache/index1/shared_cpu_map

# cat /sys/devices/system/cpu/cpu0/cache/*/*
1
0
01
Data
1
0
01
Instruction
2
0-7
ff
Unified
```

---

_Comment by @dimaqq on 2025-06-05 03:57_

P.S. the issue is getting stale and I no longer use arm64 vm's. 

---

_Closed by @konstin on 2025-06-05 10:30_

---
