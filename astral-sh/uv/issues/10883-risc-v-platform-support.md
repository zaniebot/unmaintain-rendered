```yaml
number: 10883
title: RISC-V platform support?
type: issue
state: closed
author: ifsheldon
labels:
  - enhancement
assignees: []
created_at: 2025-01-23T03:28:44Z
updated_at: 2025-09-29T11:03:18Z
url: https://github.com/astral-sh/uv/issues/10883
synced_at: 2026-01-12T16:00:23Z
```

# RISC-V platform support?

---

_@ifsheldon_

### Summary

As now RISC-V MCUs, dev boards and even servers get more and more popular, I'd really like to see uv support for RISC-V Linux platforms. There're officially supported RISC-V cpythons, so I think at least pure Python packages run fine on RISC-V machines. I think rustc can happily compile uv on RISC-V machines, but I don't know if there's anything else to get to the minimum support.

### Example

_No response_

---

_Label `enhancement` added by @ifsheldon on 2025-01-23 03:28_

---

_Comment by @zanieb on 2025-01-23 04:28_

I'm generally supportive of this. We added RISC-V builds to `python-build-standalone`. The first step is presumably a cross-build of uv in our release workflow.

---

_Comment by @FishAlchemist on 2025-01-23 05:47_


## Current Rust [Platform Support](https://doc.rust-lang.org/nightly/rustc/platform-support.html) for RISC-V Linux
###  [Tier 2 with Host Tools](https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-2-with-host-tools)
| target | notes | 
|--------|--------| 
| [riscv64gc-unknown-linux-gnu](https://doc.rust-lang.org/nightly/rustc/platform-support/riscv64gc-unknown-linux-gnu.html) | RISC-V Linux (kernel 4.20, glibc 2.29) | 
| [riscv64gc-unknown-linux-musl](https://doc.rust-lang.org/nightly/rustc/platform-support/riscv64gc-unknown-linux-musl.html) | RISC-V Linux (kernel 4.20, musl 1.2.3)|

### [Tier 3](https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3)
| target | notes | 
|-- | -- | 
riscv32gc-unknown-linux-gnu  | RISC-V Linux (kernel 5.4, glibc 2.33)
riscv32gc-unknown-linux-musl  | RISC-V Linux (kernel 5.4, musl 1.2.3 + RISCV32 support patches)

Ref: https://doc.rust-lang.org/1.84.0/rustc/platform-support.html
Ref: https://doc.rust-lang.org/nightly/rustc/platform-support.html

---

_Comment by @konstin on 2025-01-23 09:41_

We already support risc-v for installing packages (https://github.com/astral-sh/uv/pull/8934). For platforms without prebuilt binaries such as risc-v, you can install uv with `cargo install --git https://github.com/astral-sh/uv uv`. Please report anything that doesn't work on risc-v!

---

_Comment by @ifsheldon on 2025-01-24 03:11_

@konstin Thanks for the information. I didn't find it as I didn't search for PRs. `cargo install` works flawlessly, but I cannot install Python with uv.

<img width="762" alt="Image" src="https://github.com/user-attachments/assets/46fd064c-624d-4541-ab0e-e71f8e0e38af" />

So, I guess I should make a feature request in https://github.com/astral-sh/python-build-standalone ? I don't know how hard it will be to build Python against, say, linux-riscv64gc-musl target, which should enable installing Python with uv.

---

_Comment by @zanieb on 2025-01-24 03:36_

The downloads use the `riscv64` tag without the `gc` variant. I'll need to look into the differences to see what's appropriate.

---

_Comment by @zanieb on 2025-01-24 04:06_

> The difference is that riscv64gc has the extra features needed to run Linux. (ie Mutiplication, Floats, Atomics, Fences and CSRs), as well as Compressed instructions (16-bit encodings).

https://github.com/rust-lang/rust-bindgen/issues/2136

> A little more detail; RISC-V is a modular ISA, meaning that it only has a mandatory base, and everything else is an extension. RV64GC is basically "RISC-V 64-bit, extensions G and C", with the G extension being a shorthand for some other extensions;

https://github.com/flatpak/flatpak/issues/4594

Seems like we should change our builds to use the extended name? We'll discuss that upstream in https://github.com/astral-sh/python-build-standalone/issues/504


---

_Comment by @ifsheldon on 2025-01-24 05:56_

> the G extension being a shorthand for some other extensions

Yes, and I remember G actually means "general" in the sense of general computing. 

We could probably follow Rust Platform Support, like supporting [riscv64gc-unknown-linux-gnu](https://doc.rust-lang.org/nightly/rustc/platform-support/riscv64gc-unknown-linux-gnu.html) and [riscv64gc-unknown-linux-musl](https://doc.rust-lang.org/nightly/rustc/platform-support/riscv64gc-unknown-linux-musl.html), since Tier 2 supported targets of Rust means there's meaningful amount of user interest so they are "guaranteed to build" while Tier 3 targets are supported with basically best efforts.

---

_Closed by @zanieb on 2025-01-24 15:33_

---

_Comment by @ifsheldon on 2025-01-25 05:01_

@zanieb Thanks! 

I saw your commit https://github.com/astral-sh/uv/commit/98e7cd00c84d4c381955a5b0a0061bc377f75a18, so maybe you can add one more entry for building `risc64gc-unknown-linux-musl` target as well?

---

_Reopened by @zanieb on 2025-01-25 16:15_

---

_Comment by @zanieb on 2025-01-25 16:16_

Didn't mean to close this :)

---

_Comment by @taurus-forever on 2025-04-08 19:52_

Hi, just to share my experience here: `uv python install 3.11` works well on [fml13v01](https://github.com/DC-DeepComputing/fml13v01) using uv from snap:
```
taurus@taurus-uriscv:~$ uv python install 3.11
Installed Python 3.11.11 in 45.68s
 + cpython-3.11.11-linux-riscv64gc-gnu

taurus@taurus-uriscv:~$ uname -a
Linux taurus-uriscv 6.6.20-4-fml13v01 #1ubuntu1 SMP Tue Apr  8 10:07:42 UTC 2025 riscv64 riscv64 riscv64 GNU/Linux

taurus@taurus-uriscv:~$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.2 LTS
Release:	24.04
Codename:	noble

taurus@taurus-uriscv:~$ snap list astral-uv
Name       Version  Rev  Tracking       Publisher  Notes
astral-uv  0.6.7    479  latest/stable  lengau     classic
````

In the same time I cannot install uv using [uv-installer.sh](https://github.com/astral-sh/uv/releases/download/0.6.13/uv-installer.sh):
```
> sh uv-installer.sh
ERROR: there isn't a download for your platform riscv64gc-unknown-linux-gnu
```
Effectively the function `select_archive_for_arch()` has no support for RISC-V and also [binary artifacts are missing](https://github.com/astral-sh/uv/releases/tag/0.6.13).
Sad but hoping for the new architecture support soon!🤞 

---

_Comment by @zanieb on 2025-04-08 19:57_

See #12688 for risc-v release binaries. cc @Gankra re the installer

---

_Comment by @ifsheldon on 2025-07-20 09:34_

I just saw new releases had already got RISC-V binaries! Thank you (everyone and especially @Xeonacid who authored #12688) very much!

Just one last question for @Xeonacid: Is it easy, you think, to add another build target [riscv64gc-unknown-linux-musl](https://doc.rust-lang.org/nightly/rustc/platform-support/riscv64gc-unknown-linux-musl.html) based on your PR? I'd like to build against musl because now RISC-V ecosystem is very fragmented and basically every vendor has its own linux kernel fork of different versions with patches that are yet to be upstreamed. Probably a musl build can run on more RISC-V machines.

---

_Comment by @davidism on 2025-09-26 22:09_

I use cibuildwheel to build MarkupSafe, configured with `build-frontend = "build[uv]"`. I'm currently enabling `riscv64` builds, and cibuildwheel fails for `musllinux_riscv64` builds because uv is not available there. I've added an override to use `build` without uv there, but it would be nice if uv binaries were available for that combination.

---

_Comment by @konstin on 2025-09-29 11:03_

We support RISC-v64 generally, and have PyPI binaries for manylinux RISC-V64. I've split out an issue for musllinux RISC-V64 (https://github.com/astral-sh/uv/issues/16063). For further problems with RISC-V, please on new issues, to keep issues around one specific problem.

---

_Closed by @konstin on 2025-09-29 11:03_

---
