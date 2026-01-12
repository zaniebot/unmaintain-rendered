```yaml
number: 1094
title: Not available for aarch64
type: issue
state: closed
author: jwatte
labels: []
assignees: []
created_at: 2018-10-28T16:48:15Z
updated_at: 2023-01-25T04:13:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1094
synced_at: 2026-01-12T16:13:22Z
```

# Not available for aarch64

---

_@jwatte_

Most ARM boards these days are AArch64, not armhf. (The Raspberry Pi, with a gigabyte of RAM, is one of the few that's still armhf.)

It would be convenient to have downloads for aarch64/arm64 in addition to armhf.

Also, the installation instructions say "users of debian and derivatives can use the provided .deb package" but only the amd64 .deb package is actually provided.

FWIW, I'm using an NVIDIA Jetson Xavier board, which is AArch64, and previously jetsons like the TX2 are, too. These come with Utuntu 18.04.

So, minimal need satisfied: aarch64 tgz file available.
Fully convenient: aarch64 deb file available.

---

_Comment by @BurntSushi on 2018-10-28 17:24_

I sympathize, but the ARM builds are done on a purely best effort basis because it requires cross compilation AFAIK. I am barely keeping the release builds running for the current ARM release. If someone wants to contribute an additional ARM build, then I can try to run with that on a hest effort basis as well, as long as it doesn't substantially increase the release process burden.

Deb packages are almost certainly not going to happen unless `cargo deb` starts providing binary releases themselves.

I'm going to close this issue because this is probably not something I'll ever plan to do. To be honest, i would prefer that someone else pick up the release burden of maintaining ARM binaries.

PRs are welcome to correct inaccuracies in the README.

---

_Closed by @BurntSushi on 2018-10-28 17:24_

---

_Comment by @eggbean on 2021-08-11 22:48_

Just noticed that Microsoft has prebuilt binaries here: https://github.com/microsoft/ripgrep-prebuilt/releases

---

_Comment by @balupton on 2022-04-27 07:09_

I've ended up adopting ripgrep quite heavily inside https://github.com/bevry/dorothy - a cross-platform dotfile ecosystem - and noticed that on arm devices (such as my raspberry pis) it needs to download rust to compile ripgrep, which is unfortunate, as it means installing dorothy needs to install rust.

Out of other rust packages that Dorothy provides installers for, I've noticed:

- has arm64 builds among usual
    - https://github.com/sharkdp/bat
    - https://github.com/dandavison/delta
    - https://github.com/bootandy/dust
    - https://github.com/sharkdp/fd
    - https://github.com/sharkdp/hyperfine
    - https://github.com/XAMPPRocky/tokei
- has arm64 and apple silicon builds among usual
    - https://github.com/ajeetdsouza/zoxide

Perhaps the necessary build tooling could be pulled from them?

---

_Comment by @ngocphamm on 2023-01-25 04:13_

@balupton https://github.com/dandavison/delta has got apple silicon build since version 0.15, Dec 2022.

---
