---
number: 1141
title: Re-enable ARM Windows builds in release pipeline
type: issue
state: closed
author: charliermarsh
labels:
  - wish
assignees: []
created_at: 2024-01-27T01:10:46Z
updated_at: 2025-01-24T15:24:24Z
url: https://github.com/astral-sh/uv/issues/1141
synced_at: 2026-01-10T01:23:05Z
---

# Re-enable ARM Windows builds in release pipeline

---

_Issue opened by @charliermarsh on 2024-01-27 01:10_

See: https://github.com/astral-sh/puffin/actions/runs/7673412766 (failure to build `libgit2`).

See: https://github.com/astral-sh/puffin/actions/runs/7673603363 (failure to build `libgit2`, even after enabling vendoring).

---

_Label `bug` added by @charliermarsh on 2024-01-27 01:11_

---

_Comment by @konstin on 2024-01-27 10:52_

Does this mean we want support aarch64 windows? Then we need e.g. launcher binaries and some testing that puffin actually works.

---

_Comment by @charliermarsh on 2024-01-27 11:24_

I'll turn this into a more general issue. This was just about getting CI building but that makes sense. I assume I should similarly disable the i686 Windows then?

---

_Label `wish` added by @charliermarsh on 2024-01-29 03:53_

---

_Label `bug` removed by @charliermarsh on 2024-01-29 03:55_

---

_Referenced in [astral-sh/uv#1190](../../astral-sh/uv/pulls/1190.md) on 2024-01-30 17:27_

---

_Comment by @konstin on 2024-01-30 17:50_

I don't understand which of the errors was the fatal one. On linux with  `cargo xwin build --target x86_64-pc-windows-msvc`, libgit2 compile but then the linking fails with

```
lld-link: error: could not open 'msvcrtd.lib': No such file or directory
```

---

_Comment by @messense on 2024-03-28 08:39_

@konstin This is because `cargo-xwin` does not keep debug libs by default, you can pass `--xwin-include-debug-libs` to keep them, or build in release mode instead.

FYI, `msvcrtd.dll` is a "debug"-version of the file `msvcrt.dll`.

---

_Comment by @konstin on 2024-03-28 08:58_

Thank you, release mode indeed works!

---

_Comment by @henryiii on 2024-05-14 03:31_

Any plans here? Did it just need to be built in release mode? Windows ARM is another platform cibuildwheel supports. I also have a Windows ARM dev box that I'd like to be able to use uv on. :)

---

_Comment by @konstin on 2024-05-14 08:51_

The trampolines are [committed](https://github.com/astral-sh/uv/tree/12a96ad4227b82420443d80e19ea997af27f7284/crates/uv-trampoline/trampolines) and [ready](https://github.com/astral-sh/uv/blob/ad99e3af631ff90b8aefa4544ae2a5bdaea23788/crates/install-wheel-rs/src/wheel.rs#L172-L179). Last time we were investigating arm windows we were told that not even microsoft themselves properly support it, so we just haven't made CI builds for a priority.

---

_Comment by @matterhorn103 on 2024-09-18 08:29_

If you guys would be helped at all by someone testing or compiling on a Windows ARM machine, I have one and am happy to help!

---

_Comment by @darthtrevino on 2024-10-17 16:30_

So with the advent of the Snapdragon X chips, we're seeing Windows ARM machines become more commonplace. Older ARM chips were pretty lame, but they're dramatically improving, and dev ecosystem support is too. I have one team member who uses Windows ARM as their personal dev machine.

---

_Comment by @zanieb on 2024-10-17 21:59_

If someone wants to put some effort into getting an ARM build working I'm all for it. I think we could start with something low impact like the #8269 build then move to distributing releases as a secondary goal.

---

_Referenced in [astral-sh/uv#9138](../../astral-sh/uv/issues/9138.md) on 2024-11-15 02:21_

---

_Comment by @kalradivyanshu on 2025-01-04 20:54_

@zanieb can you give me some guidance on how to go about it? I have a snapdragon laptop and heavily use UV, would love to contribute.


---

_Comment by @zanieb on 2025-01-04 21:55_

@kalradivyanshu 

I added a runner called `github-windows-11-aarch64-4` to the org. I think the idea would basically be to copy the following job with the aarch64 runner and call it `build-binary-windows-aarch64` (as we do for macOS)

https://github.com/astral-sh/uv/blob/9d417da4e19b590e1026fb65014facbcb28707da/.github/workflows/ci.yml#L529-L560

---

_Comment by @kalradivyanshu on 2025-01-05 10:28_

So @zanieb basically, I need to clone the uv repo, run the steps to build it on my local machine, fix any bugs, and then write: https://github.com/astral-sh/uv/blob/9d417da4e19b590e1026fb65014facbcb28707da/.github/workflows/ci.yml#L529-L560 for windows arm?

Is this the right approach?

---

_Comment by @zanieb on 2025-01-05 17:07_

That sounds correct to me, yep!

---

_Comment by @kalradivyanshu on 2025-01-05 18:58_

Ok @zanieb I was able to get `uv` installed for arm, and then after downloading python for arm windows, everything seems to be working fine, all I had to do was get clang, arm visual studio installed -> rust installed -> clone uv -> `cargo build --release --target aarch64-pc-windows-msvc`

It worked without any bugs/hiccups.

Do you think:
```
 build-binary-windows-arm:
    needs: determine_changes
    timeout-minutes: 10
    if: ${{ github.repository == 'astral-sh/uv' && !contains(github.event.pull_request.labels.*.name, 'no-test') && (needs.determine_changes.outputs.code == 'true' || github.ref == 'refs/heads/main') }}
    runs-on:
      labels: windows-latest-large
    name: "build binary | windows"
    steps:
      - uses: actions/checkout@v4

      - name: Create Dev Drive using ReFS
        run: ${{ github.workspace }}/.github/workflows/setup-dev-drive.ps1

      # actions/checkout does not let us clone into anywhere outside ${{ github.workspace }}, so we have to copy the clone...
      - name: Copy Git Repo to Dev Drive
        run: |
          Copy-Item -Path "${{ github.workspace }}" -Destination "${{ env.UV_WORKSPACE }}" -Recurse

      - uses: Swatinem/rust-cache@v2
        with:
          workspaces: ${{ env.UV_WORKSPACE }}

      - name: "Build"
        working-directory: ${{ env.UV_WORKSPACE }}
        run: cargo build --release --target aarch64-pc-windows-msvc

      - name: "Upload binary"
        uses: actions/upload-artifact@v4
        with:
          name: uv-windows-${{ github.sha }}
          path: ${{ env.UV_WORKSPACE }}/target/debug/uv.exe
          retention-days: 1
```
will work? Will `run: ${{ github.workspace }}/.github/workflows/setup-dev-drive.ps1` have rust for windows arm, or will that need to be modified?

---

_Comment by @zanieb on 2025-01-05 19:09_

Feel free to just post the PR and we can see if CI works.

```
    runs-on:
      labels: windows-latest-large
    name: "build binary | windows"
```

You need to change the `runs-on` to the arm runner and add `aarch64` to the name. We prefer `aarch64` over `arm`, as it's the official terminology.

> Will run: ${{ github.workspace }}/.github/workflows/setup-dev-drive.ps1 have rust for windows arm, or will that need to be modified?

Presumably it will work — if not we can just drop the dev-drive.

---

_Comment by @kalradivyanshu on 2025-01-05 19:28_

@zanieb Can you tell me the label for windows aarch64 runner? According to this https://github.blog/news-insights/product-news/arm64-on-github-actions-powering-faster-more-efficient-build-systems/#get-started-using-arm-hosted-runners-today and https://www.youtube.com/watch?v=cgI6SBP8pEM it is only available to enterprise github accounts, does `uv` have access?

---

_Comment by @zanieb on 2025-01-05 19:42_

I set one up, see https://github.com/astral-sh/uv/issues/1141#issuecomment-2571421457

---

_Referenced in [astral-sh/uv#10306](../../astral-sh/uv/pulls/10306.md) on 2025-01-05 19:51_

---

_Comment by @kalradivyanshu on 2025-01-05 19:52_

@zanieb , can you take a look at this https://github.com/astral-sh/uv/pull/10306?

---

_Referenced in [astral-sh/uv#10540](../../astral-sh/uv/pulls/10540.md) on 2025-01-12 13:46_

---

_Referenced in [astral-sh/uv#10885](../../astral-sh/uv/pulls/10885.md) on 2025-01-23 05:04_

---

_Closed by @zanieb on 2025-01-24 15:24_

---
