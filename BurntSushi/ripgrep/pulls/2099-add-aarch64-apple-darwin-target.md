```yaml
number: 2099
title: Add aarch64-apple-darwin target
type: pull_request
state: closed
author: arbourd
labels: []
assignees: []
base: master
head: arm64-macos
created_at: 2021-12-12T01:40:54Z
updated_at: 2023-10-06T23:55:53Z
url: https://github.com/BurntSushi/ripgrep/pull/2099
synced_at: 2026-01-12T18:23:14Z
```

# Add aarch64-apple-darwin target

---

_@arbourd_

Adds a build target `aarch64-apple-darwin` to the release steps.

Closes #1737 

---

_Renamed from "Add macOS-arm64 build" to "Add aarch64-apple-darwin target" by @arbourd on 2021-12-12 01:58_

---

_@BurntSushi requested changes on 2021-12-13 12:42_

I think this also needs to be added to the `ci` workflow, otherwise macos-arm won't be tested on PRs.

---

_Review requested from @BurntSushi by @arbourd on 2021-12-15 18:21_

---

_Comment by @arbourd on 2021-12-15 19:25_

Not 100% sure why this is failing (not a Rust dev):

```
     Running `/Users/runner/work/ripgrep/ripgrep/target/aarch64-apple-darwin/debug/deps/globset-5ccc9bf3313e73b3`
error: test failed, to rerun pass '-p globset --lib'

Caused by:
  could not execute process `/Users/runner/work/ripgrep/ripgrep/target/aarch64-apple-darwin/debug/deps/globset-5ccc9bf3313e73b3` (never executed)
  ```
  
  Guessing we're building globset for aarch64 and trying to execute it? The macOS runners should be able to build arm64 binaries but don't think they'd be able to execute anything. 

---

_Comment by @BurntSushi on 2021-12-15 19:35_

> The macOS runners should be able to build arm64 binaries but don't think they'd be able to execute anything.

How come? If that's truly the case, then I think adding macOS/arm to CI is premature.

---

_Comment by @arbourd on 2021-12-15 19:48_

The Actions runners are amd64. I can easily build a Go or Swift arm64 binary on my intel amd64 machine, but if I try to execute that binary it will fail (unless it's universal, which is basically both the binaries squeezed into the same bin at double the size).

Homebrew uses its own arm64 runner VMs to build ripgrep binaries (it runs tests after that would require the runtime).

> If that's truly the case, then I think adding macOS/arm to CI is premature.

True, but I'd still love to see an official binary packaged here, rather than having to rely on just Homebrew/source. I'd have to check that the bin in `release.yml` actually work though, haha.

---

_Comment by @BurntSushi on 2021-12-15 19:56_

Basically, managing the release is already really time consuming work. Adding more release targets that are difficult to verify isn't great IMO. I'd be open to it, but I'm really not the right point-of-contact to help debug unfortunately. I'm neither a macOS nor an arm user. So if others want to chip in here and do the leg work to make sure this is all working properly, I'd be willing to bring it in. But that also means testing that the release workflow works (probably in another repo).

For other linux/arm builds, I believe we use Cross to both build binaries _and_ run tests. Ideally we could do that here, although I doubt the infrastructure on macOS exists for such a thing.

Given the popularity of macOS/arm, like I said, I'd be willing to bring this in, but I think others will need to put in the work here.

---

_Comment by @arbourd on 2021-12-15 20:20_

> I'm neither a macOS nor an arm user. So if others want to chip in here and do the leg work to make sure this is all working properly, I'd be willing to bring it in. But that also means testing that the release workflow works (probably in another repo).

Totally understand. I can get  the release working on my fork and verify if the binary functions. When you say "testing that the release workflow works" do you mean constantly or just the first time 😅 

> Ideally we could do that here, although I doubt the infrastructure on macOS exists for such a thing.

I think to get the full workflow in here we'd need https://github.com/actions/virtual-environments/issues/2187#issuecomment-737339211

> Given the popularity of macOS/arm, like I said, I'd be willing to bring this in, but I think others will need to put in the work here.

Happy to help here. I don't even have an arm64 Mac yet but I will eventually and when I do I wanna be able to `curl` here. 

Thank you for all your work on ripgrep. It's awesome and I use it everywhere ❤️

---

_Comment by @arbourd on 2021-12-15 20:27_

Updated the macos runner to macos-11 (latest is still 10.15). I don't think that'll fix it but arm64 cross compilation support should be better.

---

_Review comment by @arcsi42 on `.github/workflows/ci.yml`:84 on 2022-01-20 19:38_

I've tried running the `ci` workflow without this `target` specified, so `cross` isn't used for building, and all the runs have passed so far. Not sure if that's helpful.
```suggestion
```
https://github.com/arcsi42/ripgrep/actions/runs/1725214380

---

_@arcsi42 reviewed on 2022-01-20 19:38_

---

_Comment by @arcsi42 on 2022-01-20 20:48_

I have also tried modifying the `release.yml` workflow and seems to be working with the [`macos-11` runner](https://github.com/actions/virtual-environments/blob/main/images/macos/macos-11-Readme.md). Here is the passing workflow run: https://github.com/arcsi42/ripgrep/actions/runs/1725448592.

And the built artifacts could be tested by someone with an Arm Mac machine: https://github.com/arcsi42/ripgrep/releases/tag/TEST-0.0.2.

At least the file format looks okay:
```console
$ file ./ripgrep-TEST-0.0.2-aarch64-apple-darwin/rg 
./ripgrep-TEST-0.0.2-aarch64-apple-darwin/rg: Mach-O 64-bit arm64 executable, flags:<NOUNDEFS|DYLDLINK|TWOLEVEL|PIE|HAS_TLV_DESCRIPTORS>
```

---

_@arbourd reviewed on 2022-01-20 23:19_

---

_Review comment by @arbourd on `.github/workflows/ci.yml`:84 on 2022-01-20 23:19_

Without the target is it building a universal binary?

---

_Comment by @ellioseven on 2022-01-21 02:32_

@arcsi42 Can confirm I was able to get the version from the binary on my M1 Mac:

![image](https://user-images.githubusercontent.com/4191120/150455310-3aa85f2a-c54f-41b7-a1a8-143f53ecc3d9.png)


---

_@arcsi42 reviewed on 2022-01-21 17:09_

---

_Review comment by @arcsi42 on `.github/workflows/ci.yml`:84 on 2022-01-21 17:09_

Does cargo build universal binaries on Apple silicon? I'm not sure if it does yet, https://github.com/rust-lang/cargo/issues/8875.
But we could use `lipo` in a `macos-11` runner to merge both the `x86_64` and `arm64/aarch64` builds.
I have successful runs using `lipo`: https://github.com/arcsi42/ripgrep/actions/runs/1729830482, https://github.com/arcsi42/ripgrep/releases/tag/TEST-0.0.3

---

_Comment by @arcsi42 on 2022-01-21 17:13_

I have also built a universal binary using `lipo` on a `macos-11` runner: https://github.com/arcsi42/ripgrep/actions/runs/1729830482, can be downloaded from https://github.com/arcsi42/ripgrep/releases/tag/TEST-0.0.3.

Not sure what is the best way to integrate it into the `release` workflow or if a universal binary is needed at all, I just download previously built `macos` binaries and use `lipo` to merge them with github actions.

---

_Comment by @arbourd on 2022-01-21 18:23_

@BurntSushi on the universal binary: https://github.com/BurntSushi/ripgrep/issues/1737#issuecomment-732270480

---

_Comment by @arbourd on 2022-10-29 22:27_

I was able to build both amd64/arm64 binaries and run the tests with cargo/cross on macOS 13 with an arm64 machine @BurntSushi 

We might be good to go here if the tests pass 🤞🏻 

---

_Comment by @arbourd on 2022-11-07 19:28_

Hey @BurntSushi, I don't mean to bother you, but could you please **approve the workflow** so I can continue on this?

---

_Comment by @arbourd on 2022-11-07 19:56_

Thank you! So it's failing because it looks like the target `aarch64-apple-darwin`isn't present. I had this problem locally as well but I was able to solve it by resolving the rustup target. I'll look into this a bit more 👀 

---

_Comment by @arbourd on 2022-11-07 21:39_

Seems like this is still blocked by Cross because of https://github.com/cross-rs/cross/issues/558. We could also use M1 runners if GitHub ever releases them.

I'll finish this PR up if either of these things ever come to fruition.

---

_Comment by @unixsurfer on 2023-07-08 13:56_

Does anyone when this will be merged? I would like to use the pre-build package .

---

_Comment by @BurntSushi on 2023-07-08 14:11_

@unixsurfer Use brew?

Otherwise I don't schedule my free time. It  happens when it happens and if it happens. See: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#release

---

_Comment by @unixsurfer on 2023-07-08 22:20_

> @unixsurfer Use brew?
> 
> Otherwise I don't schedule my free time. It happens when it happens and if it happens. See: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#release

On my working laptop I can't use brew. I need to package it, using autopkg, and the easiest way is to use a pre-built dmg.

---

_Comment by @BurntSushi on 2023-07-09 13:45_

I looked into this, and the fundamental problem here is that I do not know how to run tests for the `aarch64-apple-darwin` target in CI. It is possible to produce a binary, but I'm not comfortable doing that without also being able to run tests. (This PR also doesn't work at all as-is because you can't use Cross to build macOS targets. You never could do that and I don't see how that would ever work. You'd need a macOS Docker image, which I'm not sure even exists. And Cross doesn't support it.)

So this looks blocked on GitHub providing macOS arm CI runners. I'm therefore going to close this out for now.

---

_Closed by @BurntSushi on 2023-07-09 13:45_

---

_Branch deleted on 2023-07-11 14:24_

---

_Comment by @bc-geoffl on 2023-10-06 19:26_

what's the latest and greatest way to install ripgrep on apple silicon without homebrew? 

Is `ripgrep-13.0.0-arm-unknown-linux-gnueabihf.tar.gz` the correct binary from [releases?](https://github.com/BurntSushi/ripgrep/releases)



---

_Comment by @BurntSushi on 2023-10-06 19:35_

@bc-geoffl No, that isn't correct. Look at its name... It has "linux" in it, so it can't be for macOS.

I'm not a macOS user, but if you want to do it without Homebrew, you can install Rust and then build ripgrep from source.

---

_Comment by @tekumara on 2023-10-06 22:25_

FYI m1 arm runners are now available https://github.blog/2023-10-02-introducing-the-new-apple-silicon-powered-m1-macos-larger-runner-for-github-actions/

---

_Comment by @BurntSushi on 2023-10-06 22:35_

@tekumara Wow, about time! I had not seen that. I'll try it out and if it works, I'll get binaries available for the ripgrep 14 release.

---

_Comment by @tekumara on 2023-10-06 23:49_

One gotcha I've just discovered is that the m1 runners are `large` and not standard sized runners, and so aren't free for open-source projects, and require [a billing plan](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions#per-minute-rates):

> Larger runners are only available for organizations and enterprises using the GitHub Team or GitHub Enterprise Cloud plans.
> Larger runners are only billed at the per-minute rate for the amount of time workflows are executed on them. There is no cost associated with creating a larger runner that is not being used by a workflow.

---

_Comment by @BurntSushi on 2023-10-06 23:55_

Sigh.

I do personally have an M2 mac mini now, so I can worst case scenario upload a binary like I do for the Debian package.

---
