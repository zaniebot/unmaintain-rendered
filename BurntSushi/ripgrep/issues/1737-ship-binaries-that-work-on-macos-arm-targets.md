```yaml
number: 1737
title: ship binaries that work on macOS arm targets
type: issue
state: closed
author: nico
labels:
  - enhancement
  - help wanted
  - rollup
assignees: []
created_at: 2020-11-21T01:27:31Z
updated_at: 2024-02-04T04:09:49Z
url: https://github.com/BurntSushi/ripgrep/issues/1737
synced_at: 2026-01-12T16:13:24Z
```

# ship binaries that work on macOS arm targets

---

_@nico_

It'd be nice if there was a prebuilt binary of rg that ran without Rosetta 2 on ARM Macs. There could either be a separate macOS-arm binary, or there could be a single universal binary that runs on both ISAs.

https://github.com/rust-lang/rust/issues/73908 covers support for this in upstream rust. This feature request is probably blocked on that completing.

https://github.com/shepmaster/rust/blob/silicon/silicon/README.md has some notes on cross-compiling.

---

_Comment by @shepmaster on 2020-11-23 15:05_

FWIW, ripgrep compiles fine on these machines, so that shouldn't be a problem.

```
% file $(which rg)
~/.cargo/bin/rg: Mach-O 64-bit executable arm64
```



---

_Label `enhancement` added by @BurntSushi on 2020-11-23 15:08_

---

_Comment by @BurntSushi on 2020-11-23 15:14_

Thanks for the feature request. But there are some things I don't understand. Please consider that when filing feature requests, that not every uses macOS. So it would help to provide more details. Some questions I have for example:

* I have no idea what it means to "run without Rosetta 2." Probably because I don't know what Rosetta 2 is. But even if I did, I don't know what it means to produce a binary without it.
* I don't know what you mean by a "universal binary" in the feature title. Universal in what sense?

Either way, I don't see myself spending time on this any time soon. If someone else wanted to work on this, then my requirements are probably the following:

1. It should work with Rust stable. I'm open to something that only works with Rust nightly, but it should be using something that is on track to be stabilized and is unlikely to see major changes.
2. It should work with `cross`. @shepmaster's instructions look great, but I'd really prefer that it work like other binaries that are produced via cross compiling.
3. And finally, that it can be done automatically via GitHub Actions.

If these requirements aren't met, then I'd probably say that Rust and macOS arm aren't ready yet. The main thing I want to avoid is adding CI for a binary that breaks frequently.

---

_Label `help wanted` added by @BurntSushi on 2020-11-23 15:14_

---

_Comment by @atouchet on 2020-11-23 16:13_

@BurntSushi

Rosetta 2 is a x86-64 to ARM64 translator. See: https://developer.apple.com/documentation/apple_silicon/about_the_rosetta_translation_environment 

Running without Rosetta 2 means running as a native ARM64 application.

Universal binary is Apple's name for fat binaries which include binaries for multiple instruction sets. See: https://developer.apple.com/documentation/xcode/building_a_universal_macos_binary

Universal binary in this sense means that it's packaged with both x86-64 and ARM64 binaries.

---

_Comment by @BurntSushi on 2020-11-23 16:25_

OK, thanks, that helps clarify things. Unless producing a universal binary with the Rust toolchain is easy, I'd expect to host two binaries: one for x86_64 and one for arm. Just like we do for other platforms.

---

_Comment by @shepmaster on 2020-11-23 16:46_

> * It should work with Rust stable.

Not yet. In 6 weeks, perhaps, but right now you need beta.

> * It should work with `cross`. @shepmaster's instructions look great, but I'd really prefer that it work like other binaries that are produced via cross compiling.

Perhaps this comment should start some effort in the cross repo. This works today outside of cross, however:

```
% rustup component add rust-std --toolchain beta --target=aarch64-apple-darwin

% SDKROOT=$(xcrun -sdk macosx11.0 --show-sdk-path) \
MACOSX_DEPLOYMENT_TARGET=$(xcrun -sdk macosx11.0 --show-sdk-platform-version) \
cargo +beta build --target=aarch64-apple-darwin

    Finished dev [unoptimized + debuginfo] target(s) in 29.32s

% file target/aarch64-apple-darwin/debug/rg
target/aarch64-apple-darwin/debug/rg: Mach-O 64-bit executable arm64
```

> * And finally, that it can be done automatically via GitHub Actions.

This should be achievable.

> Unless producing a universal binary with the Rust toolchain is easy

Depends what "with the Rust toolchain" entails. After doing the above build...

```
% cargo +beta build --target=x86_64-apple-darwin

% lipo -create -output rg target/{aarch64,x86_64}-apple-darwin/debug/rg

% file rg
rg: Mach-O universal binary with 2 architectures: [x86_64:Mach-O 64-bit executable x86_64] [arm64]
rg (for architecture x86_64):	Mach-O 64-bit executable x86_64
rg (for architecture arm64):	Mach-O 64-bit executable arm64
```

---

_Comment by @BurntSushi on 2020-11-23 16:50_

That looks great! I'd be happy adopting all of that at any time. Looks simple enough, and it being in beta would be fine with me.

---

_Comment by @BurntSushi on 2020-11-23 16:50_

@shepmaster Thank you for answering my questions with great detail. :-)

---

_Renamed from "Make macOS prebuilt binary universal" to "ship binaries that work on macOS arm targets" by @BurntSushi on 2023-07-09 13:57_

---

_Comment by @BurntSushi on 2023-07-09 13:59_

I'm not too keen on doing a universal binary here. It seems wasteful? I'd rather just build separate binaries for x86-64 and arm targets like I do for everything else.

It looks like it is possible to build an arm binary by just doing `rustup target add aarch64-apple-darwin` and then `cargo build --release --target aarch64-apple-darwin` on a macOS x86-64 host. However, there is no way to run tests and I'd really rather I only ship binaries that I can actually test.

So for now, I'd consider this blocked on GitHub providing macOS arm runners in CI.

---

_Comment by @vadz on 2023-07-09 14:06_

FWIW https://www.macstadium.com/ provides free access to ARM Macs for open source projects and we used it to set up GitHub self-hosted runner there -- and this seems to work pretty well.

Sorry if this looks like advertisement, I'm not affiliated with MacStadium in any way, I'm just grateful to them for allowing us to build and test under ARM and thought this could be useful for this project too.

---

_Comment by @BurntSushi on 2023-07-09 14:08_

Thanks. Unless it's super easy to do, I'm not inclined to do anything self-hosted here.

---

_Comment by @claviola on 2023-07-11 22:44_

> FWIW https://www.macstadium.com/ provides free access to ARM Macs for open source projects and we used it to set up GitHub self-hosted runner there -- and this seems to work pretty well.

Doesn't look like they do anymore. I came across https://www.macstadium.com/opensource, but it 404s - last in the Internet Archive on March of this year: http://web.archive.org/web/20230319190128/https://www.macstadium.com/opensource-application

edit: FWIW this seems to work quite well: https://github.com/joseluisq/rust-linux-darwin-builder
the only downside is that the image is rather large.

---

_Comment by @larkost on 2023-11-02 21:21_

It looks like the Apple Silicon workers are now available: https://github.blog/2023-10-02-introducing-the-new-apple-silicon-powered-m1-macos-larger-runner-for-github-actions/

---

_Comment by @BurntSushi on 2023-11-02 21:24_

@larkost That blog doesn't make it clear, but I believe you need to pay for it.

---

_Comment by @BurntSushi on 2023-11-25 18:51_

My plan is to do this manually on my M2 mac mini until GitHub provides free runners: https://github.com/BurntSushi/ripgrep/blob/a2907db2de20fd33b0bf02d9bd1375da06218865/ci/build-and-publish-m2

---

_Label `rollup` added by @BurntSushi on 2023-11-25 18:51_

---

_Closed by @BurntSushi on 2023-11-25 20:04_

---

_Comment by @tekumara on 2024-02-04 04:09_

Looks like those free runners are finally here 🥳 

https://github.blog/changelog/2024-01-30-github-actions-introducing-the-new-m1-macos-runner-available-to-open-source/

---
