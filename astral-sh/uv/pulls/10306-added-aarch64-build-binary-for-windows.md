```yaml
number: 10306
title: added aarch64 build binary for windows
type: pull_request
state: closed
author: kalradivyanshu
labels:
  - windows
  - releases
assignees: []
base: main
head: main
created_at: 2025-01-05T19:51:35Z
updated_at: 2025-01-23T04:39:29Z
url: https://github.com/astral-sh/uv/pull/10306
synced_at: 2026-01-12T16:09:13Z
```

# added aarch64 build binary for windows

---

_@kalradivyanshu_

## Summary

Adds an aarch64 windows build to build for windows on arm. Addresses: #1141

## Test Plan

I build uv for windows aarch64 locally by running `cargo build --target aarch64-pc-windows-msvc`, it worked.


---

_Comment by @zanieb on 2025-01-05 19:57_

For some reason the "Approve workflow" button is missing — some GitHub bug. I'll push an empty commit to trigger the run.

---

_Comment by @zanieb on 2025-01-05 19:59_

<img width="1795" alt="Screenshot 2025-01-05 at 13 59 03" src="https://github.com/user-attachments/assets/cbb878e8-4547-4650-aa8c-24a10bacacc4" />


---

_Comment by @kalradivyanshu on 2025-01-05 20:05_

@zanieb sorry about that, idk how that got there, fixed it.

---

_Label `windows` added by @charliermarsh on 2025-01-05 20:07_

---

_Label `releases` added by @charliermarsh on 2025-01-05 20:07_

---

_Comment by @kalradivyanshu on 2025-01-05 20:09_

@zanieb 
![image](https://github.com/user-attachments/assets/dbac94f8-3ab8-4dd1-b3ea-c531d5b582c8)
I will remove the dev drive, and try again.

---

_Comment by @kalradivyanshu on 2025-01-05 20:15_

https://github.com/astral-sh/uv/actions/runs/12622606852/job/35170731355

![image](https://github.com/user-attachments/assets/da8058f9-f4b8-4eac-9702-cbc19b3719e0)

`Swatinem/rust-cache@v2` failed. I will try to find one compatible with windows aarch64 in the morning, if you have any pointers, let me know, thanks 👍 

---

_Comment by @kalradivyanshu on 2025-01-05 20:18_

I don't think the runner has rust/cargo installed, since rustc -vV should not have failed. Is this an issue in the runner setup @zanieb?

---

_Comment by @zanieb on 2025-01-05 20:29_

Interesting. It looks like the aarch64 runner image is missing a bunch of the tools present on the x86_64 runners. Not much to do there but install what we need, I guess. I'd look for other people using the runner online and see if they have some standard setup we can copy.

(I did not have alternative image options when setting up the runner)

---

_Comment by @kalradivyanshu on 2025-01-05 20:33_

I tried https://github.com/dtolnay/rust-toolchain but it failed with `bash not found`.

---

_Comment by @zanieb on 2025-01-05 21:36_

More context

- https://github.com/dtolnay/rust-toolchain/issues/124
- https://github.com/actions/runner-images/issues/10479
- https://github.com/dennisameling/azure-arm64-templates/blob/main/post-deployment-script.ps1
- https://github.com/actions/runner-images/issues/768#issuecomment-2332882721
- https://github.com/actions/partner-runner-images/issues/19

---

_Comment by @kalradivyanshu on 2025-01-07 06:03_

I am trying to get clang working, its almost there, it fails while building ring, with 
```cargo:warning=Compiler family detection failed due to error: ToolNotFound: Failed to find tool. Is `clang` installed? (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)```

I had the same issue with my local, I had to add VS component for clang, I tried adding that no luck, will try again in a while.

Doc: https://learn.microsoft.com/en-us/visualstudio/install/workload-component-id-vs-build-tools?view=vs-2022

---

_Comment by @zanieb on 2025-01-08 04:44_

I'm impressed by your perseverance :)

---

_Comment by @kalradivyanshu on 2025-01-08 05:18_

Not sure if perseverance or sunk-cost fallacy, but thanks :)

---

_Comment by @kalradivyanshu on 2025-01-08 06:19_

![image](https://github.com/user-attachments/assets/feeca376-bda0-43c3-a753-4596c6a0e1dc)

@zanieb 19th try is the charm 🎉 Here is the run: https://github.com/astral-sh/uv/actions/runs/12664851516/job/35293615729, I downloaded the exe from:
```
Artifact download URL: https://github.com/astral-sh/uv/actions/runs/12664851516/artifacts/2399688891
```
Its working fine on my snapdragon x-elite windows aarch 64:
![image](https://github.com/user-attachments/assets/b33b0be2-0b0d-4fe2-a066-fe4a01c14ccc)

2 questions:
1. Do we need `- uses: Swatinem/rust-cache@v2` I am assuming it had something to do with the dev directory that didn't attach with aarch64 worker, so maybe its useless? Where will the cache persist if there is no disk?
2. Why are we not doing `cargo build --release`?

Let me know if there are any tests you want me to run/add to the CI! Thanks!

---

_Comment by @zanieb on 2025-01-08 17:12_

>  Do we need - uses: Swatinem/rust-cache@v2 I am assuming it had something to do with the dev directory that didn't attach with aarch64 worker, so maybe its useless? Where will the cache persist if there is no disk?

Let me look at the logs.

> Why are we not doing cargo build --release?

This is our "regular" CI that runs on every pull request for testing. There's a _separate_ workflow that build release binaries for distribution.


---

_Comment by @kalradivyanshu on 2025-01-08 17:50_

@zanieb do you want me to add the tests that are there for x86 to aarch64?

---

_Comment by @zanieb on 2025-01-08 17:51_

@kalradivyanshu that'd be great!

I'm looking into making things faster over in #10402 

---

_Comment by @kalradivyanshu on 2025-01-08 17:53_

Yes, it currently takes 25 mins, which is insane tbh. Also I will rename the windows step to windows-x86 to match macOS naming.

---

_Comment by @zanieb on 2025-01-09 14:44_

It looks like we'll have to hold off on those failing system checks and integration tests until this runner is better supported.

---

_Comment by @zanieb on 2025-01-09 14:44_

Wdyt of https://github.com/astral-sh/uv/pull/10402#issuecomment-2579012940

---

_Comment by @kalradivyanshu on 2025-01-09 15:04_

@zanieb I will try to get clang to work w/o using Native desktop.

> I think it might be worth it to only use the native ARM64 runner in a release CI, Rust can use --target aarch64... just fine on x86_64 hosts and this whole ">10 min CI just for getting a the bare minimum setup to compile rust" is kind of just burning money for no upside.

But also, I agree with this? I feel like `uv` is mostly depend on rust, and rust toolchain is giving aarch64 support on x86 means they have tested it, so that might cut our build time by a lot? Why do you think it might lead to issues down the road?

Also, i ran tests yesterday, many failed, will fix those in coming days.


---

_Comment by @zanieb on 2025-01-09 17:17_

> But also, I agree with this? I feel like uv is mostly depend on rust, and rust toolchain is giving aarch64 support on x86 means they have tested it, so that might cut our build time by a lot? Why do you think it might lead to issues down the road?

It might make sense to put up a separate pull request that just cross-compiles the binary then? We won't be able to test it, but at least we can say "it builds" in regular CI then follow by adding it tot he release process.

My concern with

> I think it might be worth it to only use the native ARM64 runner in a release CI

was that if we're not testing the build in CI the same way we do for releases, we can regress accidentally.

---

_Comment by @kalradivyanshu on 2025-01-09 17:44_

> It might make sense to put up a separate pull request that just cross-compiles the binary then? We won't be able to test it, but at least we can say "it builds" in regular CI then follow by adding it tot he release process.

I was thinking in this PR only, we add the aarch64 build via x86, and then run all the tests on aarch64, that way, we should be good, we can say it builds and passes all the tests in aarch64?

> was that if we're not testing the build in CI the same way we do for releases, we can regress accidentally.

This makes sense, but if all the tests pass, we can build it for release in x86 only, it will be cheaper and faster, I think microsoft is pushing for windows on ARM so a proper aarch64 runner shouldn't be far away, then we can just have build on aarch64 too.

If that works for you I can start testing it.

---

_Comment by @zanieb on 2025-01-09 18:04_

Yeah I think we're aligned here. I'm also happy to just have the cross-build working then tackle testing in follow-up pull requests.

---

_Comment by @zanieb on 2025-01-09 18:04_

(I suggested a separate PR for the cross-build approach so we can retain all the history here and start fresh on the conversation for a different approach)

---

_Comment by @kalradivyanshu on 2025-01-10 06:00_

Oh ok, makes sense, I will open the new PR, will let you know. Thanks.

---

_Comment by @zanieb on 2025-01-23 04:39_

Closing in favor of the cross-build at https://github.com/astral-sh/uv/pull/10540

---

_Closed by @zanieb on 2025-01-23 04:39_

---
