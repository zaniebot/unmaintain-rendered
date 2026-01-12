```yaml
number: 16746
title: Migrate sys_info to sysinfo
type: pull_request
state: open
author: LIghtJUNction
labels: []
assignees: []
base: main
head: sysinfo
created_at: 2025-11-15T14:03:50Z
updated_at: 2025-11-21T16:43:18Z
url: https://github.com/astral-sh/uv/pull/16746
synced_at: 2026-01-12T16:12:25Z
```

# Migrate sys_info to sysinfo

---

_@LIghtJUNction_

## Summary

Migrate the usage of the `sys_info` crate to the `sysinfo` crate throughout the project. 

Key changes:
- Replace `sys_info::os_type()` with `std::env::consts::OS` (standardized to lowercase) or `sysinfo` equivalents for consistency.
- Replace `sys_info::os_release()` with `sysinfo::System::kernel_version()`.
- Replace `sys_info::linux_os_release()` with `sysinfo::System::name()` and `os_version()`, plus custom parsing of `/etc/os-release` for `VERSION_CODENAME` (as `sysinfo` lacks direct support).
- Add `heck` dependency to `uv-python` for string case conversion (e.g., title casing OS names).
- Update linehaul.rs and related tests to use the new system info retrieval methods.
- Ensure backward compatibility in user agent and distro detection.

This migration eliminates reliance on the unmaintained `sys_info` crate, improves cross-platform system info accuracy, and aligns with modern Rust ecosystem practices.

## build

ubuntu(wsl)
<img width="1034" height="1207" alt="图片" src="https://github.com/user-attachments/assets/8a7335d8-dd36-47c6-a428-621a9e4c7690" />

windows(host)
<img width="712" height="287" alt="图片" src="https://github.com/user-attachments/assets/4da67c6c-46cc-4a0a-adad-ad732b33701f" />

android(cross)
<img width="753" height="1397" alt="图片" src="https://github.com/user-attachments/assets/cacd00ce-7645-4ecb-899a-11558cc69601" />

## Test Plan

- Compare the differences between the two libraries
- Download this test project, I tested it on Windows (host) and ubuntu (wsl).
[test_sysinfo.zip](https://github.com/user-attachments/files/23561495/test_sysinfo.zip)

<img width="722" height="323" alt="图片" src="https://github.com/user-attachments/assets/0bfb4ad0-9441-420c-a35d-8c38801db273" />


- Both changes are not perfect and require developer input
1.  I couldn't find the method to obtain version odename in the sysinfo library. I copied the corresponding function source code directly from the sys info source code and added a private function, which is not elegant, so it still needs to be confirmed by the developer.

2. To ensure consistency, I need to add a string (std::env::consts::OS), for which I added a dependency to the heck library, I'm not sure if I agree to add a dependency, this task is very simple, if you don't want to add a dependency, you can change it to a standard library implementation.

- Finally, list the specific substitutions
sys_info::linux_os_release : remove
id: info.version_codename --> Find the source code for this function from the sys_info library and apply it.
name: info.name --> sysinfo::System::name() e.g  Some("Ubuntu")
version: info.version_id --> sysinfo::System::os_version()  Some("24.04")

...
sys_info::os_release() -->sysinfo::System::kernel_version() e.g Some("6.6.87.2-microsoft-standard-WSL2")

If you find that sysinfo provides version_codename relevant API, please alert me.






---

_Marked ready for review by @LIghtJUNction on 2025-11-15 14:25_

---

_Comment by @samypr100 on 2025-11-16 00:57_

Thanks for the contribution. From a historical perspective, I did not use sysinfo due to its performance at the time and yielding wildly different results than what's expected in linehaul for some distros (when compared to pip). Things may have changed by now though, so I believe we'll need to look at two things here:

1. Compatibility: matching existing linehaul information output across different distributions (Ubuntu, Fedora, RHEL, RPI, OSX, etc.), not just WSL. Extensive manual testing was done previously across many OSes to ensure parity with existing tooling (e.g. pip). It would be great to see the user agent output for each platform (before and after) to note any differences (if any).
2. Performance: determine how performance is impacted as sys-info only added 50us of overhead. Generally querying system information is expensive and sysinfo was prohibitively expensive at the time (in the 50+ms realm). Some hyperfine results would be great.

---

_Comment by @samypr100 on 2025-11-16 02:26_

If you're trying to build uv for termux, I'd encourage you to take a look at https://github.com/termux/termux-packages/tree/master/packages/uv which shows the minimal patches needed to get uv to build for it.

---

_Comment by @LIghtJUNction on 2025-11-16 04:18_

> Thanks for the contribution. From a historical perspective, I did not use sysinfo due to its performance at the time and yielding wildly different results than what's expected in linehaul for some distros (when compared to pip). Things may have changed by now though, so I believe we'll need to look at two things here:
> 
> 1. Compatibility: matching existing linehaul information output across different distributions (Ubuntu, Fedora, RHEL, RPI, OSX, etc.), not just WSL. Extensive manual testing was done previously across many OSes to ensure parity with existing tooling (e.g. pip). It would be great to see the user agent output for each platform (before and after) to note any differences (if any).
> 2. Performance: determine how performance is impacted as sys-info only added 50us of overhead. Generally querying system information is expensive and sysinfo was prohibitively expensive at the time (in the 50+ms realm). Some hyperfine results would be great.

But now is different from the past, sys-info has not been updated for a long time.Sysinfo has been updated in recent months.I can try measuring the performance difference between the two, and I can also test it on a Linux system without using WSL.

---

_Comment by @LIghtJUNction on 2025-11-16 04:21_

> If you're trying to build uv for termux, I'd encourage you to take a look at https://github.com/termux/termux-packages/tree/master/packages/uv which shows the minimal patches needed to get uv to build for it.

Thank you, successfully compiling on termux is not very interesting. Becoming one of the contributors to UV is my goal. I will continue testing, and if I find that it is really as you described, I will close this PR. Thank you

---

_Comment by @LIghtJUNction on 2025-11-16 05:07_

Benchmark-sys-info::os_release vs sysinfo::System::kernel_version
<img width="734" height="80" alt="图片" src="https://github.com/user-attachments/assets/5952b95f-82db-4018-94f1-67a48e984d39" />

fn benchmark_os_release() {
    use std::time::Instant;

    const ITERATIONS: u32 = 10000;

    // Benchmark sys_info::os_release
    let start = Instant::now();
    for _ in 0..ITERATIONS {
        let _ = sys_info::os_release();
    }
    let duration = start.elapsed();
    println!("sys_info::os_release average time: {:.2} μs per call", duration.as_micros() as f64 / ITERATIONS as f64);

    // Benchmark sysinfo::System::kernel_version
    let start = Instant::now();
    for _ in 0..ITERATIONS {
        let _ = sysinfo::System::kernel_version();
    }
    let duration = start.elapsed();
    println!("sysinfo::System::kernel_version average time: {:.2} μs per call", duration.as_micros() as f64 / ITERATIONS as f64);

    // Compare values
    if let (Ok(sys_info_rel), Some(sysinfo_rel)) = (sys_info::os_release(), sysinfo::System::kernel_version()) {
        println!("Values equal: {}", sys_info_rel == sysinfo_rel);
    }
}

Benchmark-os_type

<img width="1051" height="86" alt="图片" src="https://github.com/user-attachments/assets/df6b96c5-11a5-4394-b039-1e5d94bac23f" />

fn benchmark_os_type() {
    use std::time::Instant;
    use heck::ToTitleCase;

    const ITERATIONS: u32 = 10000;

    // Pre-fetch values to simulate initialization once
    let sys_info_os = sys_info::os_type().unwrap_or_default();
    let env_os_title = std::env::consts::OS.to_title_case();

    // Benchmark accessing pre-fetched sys_info value
    let start = Instant::now();
    for _ in 0..ITERATIONS {
        let _ = &sys_info_os; // Simulate reading the value
    }
    let duration = start.elapsed();
    println!("Pre-fetched sys_info::os_type value access average time: {:.2} μs per call", duration.as_micros() as f64 / ITERATIONS as f64);

    // Benchmark accessing pre-fetched env OS title case value
    let start = Instant::now();
    for _ in 0..ITERATIONS {
        let _ = &env_os_title; // Simulate reading the value
    }
    let duration = start.elapsed();
    println!("Pre-fetched std::env::consts::OS.to_title_case() value access average time: {:.2} μs per call", duration.as_micros() as f64 / ITERATIONS as f64);

    // Compare values
    println!("Values equal: {}", sys_info_os == env_os_title);
}
Here the latter uses the Heck library to capitalize the initials, but the time difference is almost non-existent.

Benchmark-sys-info::linux_os_release vs sysinfo::System::name() / sysinfo::System::os_version()
This is not a fair estimate and is therefore for reference only

<img width="780" height="56" alt="图片" src="https://github.com/user-attachments/assets/fdefd5a9-771e-4062-a87f-5e0be4c104ff" />



---

_Comment by @zanieb on 2025-11-16 16:34_

As an aside, please don't use screenshots of text.

---

_Comment by @konstin on 2025-11-19 09:42_

While I agree that sysinfo looks better than sys_info, this PR is hard to review, because we need to ensure that the linehaul data doesn't have regressions from this, and we need to check this for Windows, macOS, and major Linux distros, across several versions.

---

_Comment by @LIghtJUNction on 2025-11-21 16:43_

> While I agree that sysinfo looks better than sys_info, this PR is hard to review, because we need to ensure that the linehaul data doesn't have regressions from this, and we need to check this for Windows, macOS, and major Linux distros, across several versions.

By directly comparing the source code, I found the API mappings; only one was missing. Also, feel free to modify the code here; you have a better understanding of this project. I haven't strictly followed the project's coding style here.

---
