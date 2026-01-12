```yaml
number: 676
title: "Request: Binaries for ARM / Raspberry Pi"
type: issue
state: closed
author: n8henrie
labels:
  - enhancement
  - help wanted
  - question
assignees: []
created_at: 2017-11-12T20:11:39Z
updated_at: 2018-03-18T22:27:07Z
url: https://github.com/BurntSushi/ripgrep/issues/676
synced_at: 2026-01-12T16:13:22Z
```

# Request: Binaries for ARM / Raspberry Pi

---

_@n8henrie_

Ripgrep is so fast and lightweight that it really seems like it would shine on low-resources devices like the Raspberry Pi. Unfortunately, the current binaries don't include an ARM compatible release. Additionally, the process for cross-compiling with Rust from OSX to ARM seems [more complicated than I anticipated](https://github.com/Ragnaroek/rust-on-raspberry-docker) (at least compared to Go), and installing Rust on the Pi seems like a little more work than I'd hope for just to be able to install ripgrep.

If there's any way the `armv7-unknown-linux-gnueabihf` target could be added as one of the binaries you provide for easy download, I bet it would see a decent amount of use from fellow Raspberry Pi users.  As someone unfamiliar with Rust, trying to figure out rustup and cross compiling is currently taking up a lot of my day, and a simple `wget` and `tar xzf` certainly sounds appealing.

---

_Comment by @jeffvandyke on 2017-11-20 16:39_

I'm actually pretty interested in a "Rust -> LLVM -> anything else" build process, so in the next week or so I might investigate getting that to work with ripgrep. Can't promise anything though!

---

_Comment by @BurntSushi on 2017-11-22 12:13_

I will say two things:

1. In general, I am in favor of this.
2. The release process is already a slog for me. I don't have any easy way of testing ARM release artifacts. If this somehow increases the release burden for me in any appreciable way, then I probably won't do it. In that case, someone else will need to step up. However, I am willing to try it.

---

_Label `enhancement` added by @BurntSushi on 2017-11-22 12:13_

---

_Label `help wanted` added by @BurntSushi on 2017-11-22 12:13_

---

_Label `question` added by @BurntSushi on 2017-11-22 12:13_

---

_Comment by @n8henrie on 2017-11-23 22:13_

Thanks for considering! I would think this should just add an additional target to however Rust does its builds, as long as you're on Linux (which seems to have better options for ARM cross compiling than MacOS). I don't know if there is a (free) CI that has ARM support, if so that might be helpful.

---

_Closed by @BurntSushi on 2017-12-18 21:26_

---

_Comment by @n8henrie on 2018-02-12 18:56_

Was excited to test these out after seeing the 0.8.0 release notes and remembering your limited ability to test per this conversation. The release at https://github.com/BurntSushi/ripgrep/releases/download/0.8.0/ripgrep-0.8.0-arm-unknown-linux-gnueabihf.tar.gz seems to work great on my RPi 3 -- thank you!

Unfortunately I get 

```shellsession
$ ./rg --version
Illegal instruction
```
on my Pi Zero, which is armv6.

Are you sure the release labeled `ripgrep-0.8.0-arm-unknown-linux-gnueabihf` is actually `arm-unknown-linux-gnueabihf` and not `armv7-unknown-linux-gnueabihf` (which I had mentioned above)?

When I cross-compile on my Pi3, `cargo build --release --target arm-unknown-linux-gnueabihf` produces a build that works on both the ARMv6 Pi Zero and ARMv7 Pi3, but the `armv7-unknown-linux-gnueabihf` target produces a build that results in the `Illegal instruction` like I'm seeing with the new release, so it makes me wonder.

---

_Comment by @BurntSushi on 2018-02-12 19:06_

@n8henrie TL;DR - I have no idea. :-)

An illegal instruction usually pops up when someone tries to run a binary that was compiled for more advanced CPU features than their system has. But the [ARM build does not add any additional CPU features since it isn't considered an SSSE3 target](https://github.com/BurntSushi/ripgrep/blob/81afe8c5a0a04960353b68731a0c6d31436da04e/ci/before_deploy.sh#L10-L17) (which makes sense, since SSSE3 is an x86 thing).

Basically, someone else will need to debug this. I did actually try running the CI build on my machine, but installing the requisite cross compiler required me to build from source on Archlinux, and I don't feel like doing that. But basically, the compilers being used are in the `.travis.yml` here: https://github.com/BurntSushi/ripgrep/blob/81afe8c5a0a04960353b68731a0c6d31436da04e/.travis.yml#L40-L43 And the `$TARGET` used is here: https://github.com/BurntSushi/ripgrep/blob/81afe8c5a0a04960353b68731a0c6d31436da04e/.travis.yml#L36 which is `arm-unknown-linux-gnueabihf`, which seems to be what you want.

So... I don't know. :-/ 

---

_Comment by @n8henrie on 2018-02-22 07:12_

Also running Arch as my primary Linux box -- took a little figuring out, but seems to be sufficient to install `arm-linux-gnueabihf-gcc` from AUR and set `~/.cargo/config` to

```
[target.arm-unknown-linux-gnueabihf]                                                                                                                                                                                          
linker = "arm-linux-gnueabihf-gcc"                                                                                                                                                                                            

[target.armv7-unknown-linux-gnueabihf]                                                                                                                                                                                        
linker = "arm-linux-gnueabihf-gcc"   
```

With `cargo build --release --target=arm-unknown-linux-gnueabihf`, I get a version that runs fine on my armv6 Pi Zero. With `cargo build --release --target=armv7-unknown-linux-gnueabihf`, I get `Illegal instruction`, just like with the version from the 0.8 release (which works on my armv7 machines). Very odd.

Wonder if it may be [related to this SO answer](https://raspberrypi.stackexchange.com/a/58986) saying that the `armhf` packages available in Debian-based distros may not work for armv6.

---

_Comment by @n8henrie on 2018-02-22 21:08_

I got an AWS Ubuntu instance to see if I could reproduce this issue. Interestingly, I also have to manually install `gcc`, which I don't see in the travis file (and isn't pre-installed AFAIK), and I just used the `gcc-arm-linux-gnueabihf` package instead of the individual ones you're using.

```bash
sudo apt -y install gcc gcc-arm-linux-gnueabihf
```

After that, I can compile `armv7-unknown-linux-gnueabihf` and `arm-unknown-linux-gnueabihf` versions which -- just like the others -- both work on my armv7 RPi3, but not on my armv6 RPi Zero.

---

_Comment by @BurntSushi on 2018-02-22 21:16_

@n8henrie Bummer. Thanks for looking into this though! Is this a bug in the Ubuntu packages, or are there additional/different gcc compilers that should be installed?

---

_Comment by @n8henrie on 2018-03-09 15:56_

Looks like this confirms a problem with the Ubuntu package being armv7 specific: https://github.com/rust-lang/rust/issues/38570

Everything I'm seeing recommends using https://github.com/raspberrypi/tools, which I imagine may make a RPi-specific binary. If I figured out how to do this on Travis, would you be willing to accept a PR to that effect?

While it is an official repo (so I figure should be somewhat reliable), it looks like there are no releases or tags, so I imagine it would involve downloading the master branch and using that to cross-compile and releasing a version specific for RPi.

---

_Comment by @BurntSushi on 2018-03-09 16:05_

> If I figured out how to do this on Travis, would you be willing to accept a PR to that effect?

Yes, as long as the build times are reasonable. If you have to build the compiler, then I expect the build times would become unreasonable.

(And my usual caveat applies. If I'm trying to do a release and the ARM build fails, then I won't spend much time on trying to fix it. If it does fail, I'll make the release anyway, and would be happy to try a subsequent patch release if the problem with the ARM build got fixed.)

---

_Comment by @n8henrie on 2018-03-18 22:27_

It doesn't need to build the compiler, just cloning from the official [raspberrypi/tools](https://github.com/raspberrypi/tools) and set that as the linker.

On my AWS Ubuntu testbox, the following script installs a binary that works on my armv6 and armv7 Raspberry Pis (like building on Arch).

```bash
#! /bin/bash

set -e

BUILDDIR="${HOME}/build-rpi"
mkdir -p "${BUILDDIR}"
test -d "${BUILDDIR}/tools" || git -C "${BUILDDIR}" clone --depth=1 https://github.com/raspberrypi/tools.git
test -d "${BUILDDIR}/ripgrep" || git -C "${BUILDDIR}" clone --depth=1 --branch=0.8.1 https://github.com/BurntSushi/ripgrep.git

sudo apt update && sudo apt install -y gcc

PATH="${HOME}/.cargo/bin:${PATH}"
rustc --version || curl https://sh.rustup.rs -sSf | sh -s -- -y --default-toolchain="stable"

rustup target add arm-unknown-linux-gnueabihf

cat <<EOF > "${HOME}"/.cargo/config
[target.arm-unknown-linux-gnueabihf]
linker = "${HOME}/build-rpi/tools/arm-bcm2708/arm-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc"
EOF

pushd "${BUILDDIR}/ripgrep"
cargo build --release --target=arm-unknown-linux-gnueabihf
popd
```

Assuming the ripgrep repo is already cloned and that gcc and rustup are already installed (since the Travis environment will already have those), it takes 5m34s to add the `arm-unknown-linux-gnueabihf` target, clone the linker, and build the rpi release on the AWS nano instance. If I also assume the `arm-unknown-linux-gnueabihf` target is already added, it takes 5m25s. I made sure to `cargo clean` in between. I can't say how these times on a nano instance will compare to the Travis build environment.

If I first run the `armv7-unknown-linux-gnueabihf` target (after installing the `addons` you've included for arm), it takes 4m12s, so I guess there are some intermediates that can be reused.

I'd be happy to work on a PR, but I'm not sure how to change the `TARGET` envvar without conflicting with the current ARM target, which is probably more appropriate for generic ARM architectures (this would probably be best released as `arm-rpi-` or something of the sort, since I don't know if the toolchain will work on other non-rpi devices, but it still uses `--target=arm-unknown-linux-gnueabihf` which I assume would overwrite the other ARM release).

The other ARM release could be specified as the ARMV7 target (since that what it seems to work on), and `arm-unknown-linux-gnueabihf` could be used for this one, but that still wouldn't help point out to users that this one may be rpi specific.

---
