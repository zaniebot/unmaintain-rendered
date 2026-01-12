```yaml
number: 2739
title: "Provide Statically Compiled Binaries for (aarch64|arm64) Linux"
type: issue
state: open
author: Azathothas
labels: []
assignees: []
created_at: 2024-02-20T06:52:37Z
updated_at: 2025-10-23T14:56:30Z
url: https://github.com/BurntSushi/ripgrep/issues/2739
synced_at: 2026-01-12T16:13:24Z
```

# Provide Statically Compiled Binaries for (aarch64|arm64) Linux

---

_@Azathothas_

Hi, the current releases for arm64 Linux is based on gnu and not musl.
As a result, the binary is dynamically linked:
```bash
$ https://github.com/BurntSushi/ripgrep/releases/download/14.1.0/ripgrep-14.1.0-aarch64-unknown-linux-gnu.tar.gz
$ file rg && ldd rg
rg: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, for GNU/Linux 3.7.0, BuildID[sha1]=62e25ad353379dfb4ddf3c94be5e00cd086d56d5, stripped
        linux-vdso.so.1 (0x0000ffffa91df000)
        libgcc_s.so.1 => /lib/aarch64-linux-gnu/libgcc_s.so.1 (0x0000ffffa8c40000)
        libpthread.so.0 => /lib/aarch64-linux-gnu/libpthread.so.0 (0x0000ffffa8c20000)
        libdl.so.2 => /lib/aarch64-linux-gnu/libdl.so.2 (0x0000ffffa8c00000)
        libc.so.6 => /lib/aarch64-linux-gnu/libc.so.6 (0x0000ffffa8a50000)
        /lib/ld-linux-aarch64.so.1 (0x0000ffffa91a6000)
$ du -sh rg
5.2M    rg
```

Adding target: `aarch64-unknown-linux-musl` in https://github.com/BurntSushi/ripgrep/blob/master/.github/workflows/release.yml should work.

However, you can release an even more optimized & smaller binary, based on : https://github.com/Azathothas/Toolpacks/blob/main/.github/scripts/aarch64_Linux/bins/ripgrep.sh
```bash
$ file "./target/$RUST_TARGET/release/rg"
./target/aarch64-unknown-linux-musl/release/rg: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, stripped
$ du -sh "./target/$RUST_TARGET/release/rg"
4.0M    ./target/aarch64-unknown-linux-musl/release/rg
```

---

_Comment by @BurntSushi on 2024-02-20 11:36_

I'd be okay with adding this target in a way that matches the configuration of everything else. (The question of whether to enable LTO or not has been discussed a few times before. I remain reluctant to enable it for releases.)

---

_Comment by @tzq0301 on 2024-06-06 10:55_

Any progress about this issue? It's really helpful in some specific situations.

---

_Comment by @BurntSushi on 2024-06-06 11:42_

Please don't ask for progress updates. If there was an update, you would see it here or linked from here.

---

_Comment by @tzq0301 on 2024-06-06 16:22_

Maybe this helpful? https://github.com/BurntSushi/ripgrep/pull/2834

The output (the artifact) can be well worked on my aarch64 Debian VM.

---

_Comment by @chrisdone-artificial on 2025-10-23 13:37_

@tzq0301 thanks - I'm trying that out [here](https://github.com/BurntSushi/ripgrep/pull/3207) for the latest release. 

For my home environment I've tried my best to love Nix (mixed results! 😂), but have settled on inlining statically built arm64/amd64 binaries into my Emacs git repo (that way I can move between my Linux on my work MacBook and Linux on my personal lappy seamlessly). It's a little unconventional but with my Emacs config intimiately tied to rg's behavior, I want it pinned. So I'm happy to attempt a one-off artifact build and then I won't have to do it again for years.

**EDIT**: Looks like I misunderstood the mechanics of ripgrep's GHA artifact genertation. I'll come back to this another time and install the Rust toolchain on a docker container or something and run a static build. 👍 

---

_Comment by @chrisdone-artificial on 2025-10-23 14:56_

Ended up using a Dockerfile: [rg arm64 static build](https://gist.github.com/chrisdone-artificial/14ca15717e94d0980865475d9602dcf7) Easy peasy.

---
