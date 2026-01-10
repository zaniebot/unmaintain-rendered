```yaml
number: 14574
title: Support target aarch64-linux-android
type: issue
state: open
author: mokeyish
labels:
  - enhancement
assignees: []
created_at: 2025-07-12T07:28:11Z
updated_at: 2025-12-08T10:34:58Z
url: https://github.com/astral-sh/uv/issues/14574
synced_at: 2026-01-10T03:11:34Z
```

# Support target aarch64-linux-android

---

_Issue opened by @mokeyish on 2025-07-12 07:28_

### Summary

Support target aarch64-linux-android on Termux https://termux.dev/en/

### Example

_No response_

---

_Label `enhancement` added by @mokeyish on 2025-07-12 07:28_

---

_Comment by @criticalthoughtdevelopers on 2025-08-27 13:11_

## Successful Build from Source on aarch64-linux-android with Workarounds

The target aarch64-linux-android is buildable from source with several workarounds. Below is a summary of the build failures encountered and their corresponding resolutions.

### System Specifications

```
Phone: Pixel 9 Pro
Android: v16
Total Ram: 15Gig
TERMUX_API_VERSION=0.52.0
TERMUX_VERSION=0.118.3
Rust: 1.89.0
```

### Build Failures & Resolutions

#### sys-info Crate Compilation Failure

- Error: Fails to compile C code due to an undeclared function index().
- Resolution: Pass -Dindex=strchr to the C compiler via the CFLAGS environment variable.

#### tikv-jemalloc-sys Linker Failure

- Error: Linker fails with error: unable to find library -lgcc.
- Diagnosis: The Termux ndk-sysroot package does not provide libgcc.a. The functional equivalent provided by the LLVM toolchain is libunwind.a. The jemalloc build script does not account for this substitution.
- Resolution: Create a symbolic link to satisfy the linker: ln -s $PREFIX/lib/libunwind.a $PREFIX/lib/rustlib/aarch64-linux-android/lib/libgcc.a.

#### Resource Exhaustion During Final Link

- Error: The rustc process is terminated by the kernel (SIGKILL), indicating an Out-Of-Memory condition.
- Resolution: Disable Link-Time Optimization (LTO) and restrict the build to a single job. export RUSTFLAGS="-C lto=off" and cargo install --jobs 1.

**Note**: On devices with less RAM, a swap file may still be necessary even with LTO disabled.

### Complete Working Command Sequence

The following sequence of commands produces a successful build:

```shell
# Install required build dependencies
pkg install binutils gcc

# (Optional, for low-memory devices) Setup swap file
# fallocate -l 2G ~/swapfile
# chmod 600 ~/swapfile
# mkswap ~/swapfile
# swapon ~/swapfile

# Create symbolic link to satisfy linker requirement for libgcc.a
ln -s $PREFIX/lib/libunwind.a $PREFIX/lib/rustlib/aarch64-linux-android/lib/libgcc.a

# Set environment variables for the build
export RUSTFLAGS="-C lto=off -C link-arg=-L$PREFIX/lib/rustlib/aarch64-linux-android/lib"
export AR=aarch64-linux-android-ar
export RANLIB=aarch64-linux-android-ranlib
export CFLAGS="-Dindex=strchr"

# Build and install
cargo install --locked --path crates/uv --jobs 1

# Cleanup
# swapoff ~/swapfile && rm ~/swapfile
rm $PREFIX/lib/rustlib/aarch64-linux-android/lib/libgcc.a

```

---

_Comment by @Jobians on 2025-11-24 21:00_

No updates?

 $ curl -LsSf https://astral.sh/uv/install.sh | sh

ERROR: there isn't a download for your platform aarch64-linux-android

---

_Comment by @konstin on 2025-11-26 15:56_

We don't provide binary installs for the android target at this point, but we accepted patches that help with building from source for aarch64-linux-android.

---

_Comment by @Jobians on 2025-11-26 16:12_

I managed to install it on Termux using 
```shell
pkg install uv
```
https://github.com/termux/termux-packages/tree/master/packages/uv

---

_Comment by @o-murphy on 2025-11-27 08:02_

> I managed to install it on Termux using
> 
> pkg install uv
> https://github.com/termux/termux-packages/tree/master/packages/uv

Yes it works but since python>=3.13 supports android as a 3-tier platform it could be great to have possibility to install uv as [described in uv docs](https://docs.astral.sh/uv/getting-started/installation/) and have precompiled python builds in [python-build-standalone](https://github.com/astral-sh/python-build-standalone)

It also can be usefull not only for termux, also for the chroots running over [Userland](https://github.com/CypherpunkArmory/UserLAnd)

---

_Comment by @Vinfall on 2025-12-07 06:49_

> since python>=3.13 supports android as a 3-tier platform

FYI: [python releases for android](https://www.python.org/downloads/android/) are available on www.python.org for 3.14+ (including 3.14.0rc and 3.15.0a). This should make it easier to support 3.14+ at least. 3.13 support in particular could a bit tricky though.

---

_Comment by @o-murphy on 2025-12-08 08:01_

> > since python>=3.13 supports android as a 3-tier platform
> 
> FYI: [python releases for android](https://www.python.org/downloads/android/) are available on [www.python.org](http://www.python.org) for 3.14+ (including 3.14.0rc and 3.15.0a). This should make it easier to support 3.14+ at least. 3.13 support in particular could a bit tricky though.

I doubt that embedded builds for android will be able to run arbitrary code, including compiled extensions without rebuilding the application where it is used _(although [kivy/python-for-android](https://github.com/kivy/python-for-android) able, if you have root access)_, it is still not system-wide python. So the most reliable methods to get common and well-known python env still remain chroot environments via UserLAnd and termux.

---
