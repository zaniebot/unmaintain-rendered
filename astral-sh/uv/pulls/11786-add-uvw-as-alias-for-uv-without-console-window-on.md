```yaml
number: 11786
title: "Add `uvw` as alias for `uv` without console window on Windows"
type: pull_request
state: merged
author: CrendKing
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2025-02-26T01:40:28Z
updated_at: 2025-05-29T00:02:59Z
url: https://github.com/astral-sh/uv/pull/11786
synced_at: 2026-01-10T11:10:39Z
```

# Add `uvw` as alias for `uv` without console window on Windows

---

_Pull request opened by @CrendKing on 2025-02-26 01:40_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Related to https://github.com/astral-sh/uv/issues/6801.

Currently on Windows, uv itself will always creates a console window, even though the window could be empty if `uv run --gui-script` is used. This is due to it using the [default `console` window subsystem](https://rust-lang.github.io/rfcs/1665-windows-subsystem.html).

This PR introduces a wrapper `uvw` that, similar to the existing `uvx`, invokes `uv` with the [`CREATE_NO_WINDOW`](https://learn.microsoft.com/en-us/windows/win32/procthread/process-creation-flags#:~:text=CREATE_NO_WINDOW) process creation flag on Windows, which creates child process without console window.

Note that this PR does not alter any behaviors regarding `run --script` and `run --gui-script`.

## Test Plan

Built and tested locally by doing something like `uvw run test.py`.

---

_Comment by @CrendKing on 2025-02-26 01:42_

I personally don't like the amount of duplicate code between `uvx` and `uvw`. Should I extract the common logic to a separate file?

---

_Comment by @zanieb on 2025-02-26 03:27_

Thanks for posting the pull request — I haven't looked at the implementation yet but note you'll want to look back at when we added `uvx` because we messed that up and broke the release process. I think there are various places you need to put the binary in an archive.

---

_Comment by @samypr100 on 2025-02-26 03:36_

> Thanks for posting the pull request — I haven't looked at the implementation yet but note you'll want to look back at when we added `uvx` because we messed that up and broke the release process. I think there are various places you need to put the binary in an archive.

Is the goal to distribute uvw only on windows or all platforms? For example, I don't think pythonw is available on other platforms besides windows. That will determine the distribution changes needed.

e.g. In the case of windows only, I don't expect bundling this binary in the docker images

---

_Comment by @zanieb on 2025-02-26 03:42_

Yeah I think we should not publish this on other platforms.

---

_Comment by @CrendKing on 2025-02-26 03:43_

> Thanks for posting the pull request — I haven't looked at the implementation yet but note you'll want to look back at when we added `uvx` because we messed that up and broke the release process. I think there are various places you need to put the binary in an archive.

Can you link the issue(s) and where do I put this in?

> Is the goal to distribute uvw only on windows or all platforms? For example, I don't think pythonw is available on other platforms besides windows. That will determine the distribution changes needed.
> 
> e.g. In the case of windows only, I don't expect bundling this binary in the docker images

True. Technically this binary wouldn't need the existence of `pythonw`. But it also wouldn't do anything special under non-Windows OSes. So it makes sense not to distribute there.

---

_Comment by @zanieb on 2025-02-26 03:48_

- https://github.com/astral-sh/uv/pull/4632
- https://github.com/astral-sh/uv/pull/4743
- https://github.com/astral-sh/uv/pull/4744
- https://github.com/astral-sh/uv/pull/4756

---

_Comment by @CrendKing on 2025-02-26 03:56_

I added the relevant lines to the Action config. However, I think currently the binary is generated under all platforms regardless. I don't know how to generate `uvw` only under Windows. https://users.rust-lang.org/t/compile-a-binary-for-a-specific-platform/56528 says there isn't built-in way.

---

_Comment by @zanieb on 2025-02-26 04:50_

That's sort of a blocker.

@Gankra may know some dark arts.

---

_Comment by @CrendKing on 2025-03-03 00:12_

Any update? I personally don't think having a harmless executable in the build directory, which is not included in the final release archive, is that big of a deal. On the other hand, if you guys have to introduce dark magic to get rid of it, that could become maintenance burden in the future. Would documenting the behavior somewhere in the `docs` suffice?

---

_Comment by @Gankra on 2025-03-04 19:34_

Ah sorry I missed this!

Yes cargo doesn't let you have platform-conditional-binaries. It *does* however let you have feature-flag-conditional binaries. So you could, in theory, introduce a `windows-gui-bin` feature or something and add `required-features = ["windows-gui-bin"]` to the `[[bin]]` entry for `uvw`.

https://doc.rust-lang.org/cargo/reference/cargo-targets.html#binaries

Then in our maturin release build tooling, we could add `--features windows-gui-bin` to the build invocation:

https://github.com/astral-sh/uv/blob/b7f98f1ff2b7e4a9ff4d62d1946f697dd9198ca7/.github/workflows/build-binaries.yml#L181

and add uvw to the files we archive:

https://github.com/astral-sh/uv/blob/b7f98f1ff2b7e4a9ff4d62d1946f697dd9198ca7/.github/workflows/build-binaries.yml#L198-L201

Probably the biggest issue with this is I don't think cargo-dist('s installers) would understand? I was actually remarkably pedantic about this possibility in *a lot* of cargo-dist's code/metadata, so actually it might be a relatively small change to support. But I believe there's no UI to tell cargo-dist "ok I'm actually doing this!" (unless it was added in the last 4 months).


---

_Comment by @CrendKing on 2025-03-04 20:07_

Thank you Gankra. I updated the PR. Could you check if I'm doing it correctly?

---

_Comment by @Gankra on 2025-03-06 15:00_

Sorry I should have been more clear -- I believe we can't ship this without upstream work in
https://github.com/axodotdev/cargo-dist/

If I checkout your branch (which is indeed written Correctly as I described) we see that dist doesn't understand and will expect every platform to provide uvw. This will likely lead to installers erroring out as they try to validate the presence of missing files:

```
$ dist plan
...
announcing v0.6.3
  uv 0.6.3
    source.tar.gz
      [checksum] source.tar.gz.sha256
    uv-installer.sh
    uv-installer.ps1
    sha256.sum
    uv-aarch64-apple-darwin.tar.gz
      [bin] uv
      [bin] uvw
      [bin] uvx
      [checksum] uv-aarch64-apple-darwin.tar.gz.sha256
    uv-aarch64-pc-windows-msvc.zip
      [bin] uv.exe
      [bin] uvw.exe
      [bin] uvx.exe
      [checksum] uv-aarch64-pc-windows-msvc.zip.sha256
      ...
```



---

_Comment by @CrendKing on 2025-03-06 18:54_

Sorry I didn't understand that cargo-dist is a required component/step for uv.

I think there are three options forward.

1) Like I mentioned [above](https://github.com/astral-sh/uv/pull/11786#issuecomment-2692986842), we can make it so every platform builds the uvw binary. It is just that uvw does nothing special other than directly wrapping `uv` on non-Windows platforms, which I consider being useless but also harmless. Maybe this requires some documentation somewhere, if you guys are OK with that.
2) We wait for cargo-dist to update to support platform-specific binaries.
3) We abandon this PR. I could move the source code to a new repository that you guys own (like under astral-sh account, e.g. `astral-sh/uvw`). Setup a release process there.

---

_Comment by @zanieb on 2025-03-06 19:02_

Regarding (1), I think there's a cost to every additional artifact we add to releases. It's user-facing complexity.

(2) I think we'll need to push for support in `cargo-dist` directly, or wait until we start to take more ownership over that component of our releases.

(3) A derived third-party artifact is an option, though I'd be a bit sad about it. I understanding pursuing it as a stop-gap, but hopefully we can do better for users.

@Gankra do you have an idea of the scope of work needed upstream?



---

_Comment by @CrendKing on 2025-03-06 19:08_

Thank you zanieb. I do believe option 2 is the best to go, if we can push the required changes to `cargo-dist` in timely fashion. Please let me know if you need me to create or follow-up issue ticket in their repo.

---

_Comment by @Gankra on 2025-03-06 20:08_

On paper it's actually not a huge lift. It's ideally mostly just adding config option similar to the [min-glibc-version](https://opensource.axo.dev/cargo-dist/book/reference/config.html?highlight=bins#min-glibc-version) but for [package.binaries](https://opensource.axo.dev/cargo-dist/book/reference/config.html?highlight=bins#packagebinaries) instead.

Then that config value would need to be queried/applied here:

https://github.com/axodotdev/cargo-dist/blob/c8ba950c63f9c38c77782912ec6cdb6807bd0fbd/cargo-dist/src/tasks/mod.rs#L1340-L1344

So in an ideal world 99% of the work is agreeing on the config and piping it to this location and it Just Works.

However there's a non-zero chance that random things will blow up because although the code tries really hard to make things deal with per-platform-binaries it hasn't ever *really* been a thing so it's very easy for something to have cut a corner and for it to have been fine up until now. However the only way to know is Try It And See.

---

_Comment by @CrendKing on 2025-03-08 00:34_

I see that currently the `package.binaries` is internally a Vec of string, while `min-glibc-version` is a `SortedMap<String, LibcVersion>`. Pushing through this backward incompatible change doesn't seem to be a trivial task at all, not to mention the cargo-dist owners might not like the idea of platform-specific artifacts to begin with. Maybe I'm a bit pessimistic here.

Do you guys want me to start an issue ticket on cargo-dist and reference to this PR?

---

_Comment by @zanieb on 2025-03-08 01:31_

@Gankra is a former cargo-dist maintainer and has a fair amount of perspective there. Anyway, please do open an issue there and we can try to coordinate with them.

---

_Comment by @CrendKing on 2025-03-08 04:27_

https://github.com/axodotdev/cargo-dist/issues/1791

---

_Comment by @Gankra on 2025-04-09 18:03_

platform-specific bins support in cargo-dist https://github.com/astral-sh/cargo-dist/pull/17

---

_Comment by @CrendKing on 2025-04-09 21:50_

Great news! I'll wait for the release there.

Below is for documentation.

I've been using the `uvw` locally myself, and the only thing bothering me is that even though there is no visible console window now, since `uvw` actually invokes `uv`, which is built with console option, it still spawns a useless `conhost.exe` process.

The only way to make that conhost.exe never spawn is to change our creation flag to [`DETACHED_PROCESS`](https://learn.microsoft.com/en-us/windows/win32/procthread/process-creation-flags#:~:text=0x00000008), and always use `--gui-script`. Unfortunately, if user doesn't use `--gui-script` (which is the default), the console window is back.

For maximum ease of use, I think we should keep the current `CREATE_NO_WINDOW` flag, unless one day `uv` supports an option to always use `--gui-script` through config file.

---

_Assigned to @Gankra by @zanieb on 2025-04-15 20:31_

---

_Comment by @dpy013 on 2025-04-20 03:12_

Suggested Resolve conflicts

---

_Comment by @CrendKing on 2025-04-21 00:02_

I added a `[workspace.metadata.dist.binaries]` section in Cargo.toml. I tested with `dist plan` and see only Windows target has the extra `uvw.exe`. I'm not sure if there is more concise way to write all Windows targets though (e.g. `*-windows-msvc = ...`)

I also applied the update to `uvx` from https://github.com/astral-sh/uv/pull/12923 about suffix searching. I tested myself. Though now there are even more duplicate logic between `uvx` and `uvw`.

---

_Comment by @CrendKing on 2025-05-14 06:31_

Anyone still interested?

---

_Comment by @zanieb on 2025-05-14 14:27_

@Gankra can take a look, but is at a conference right now.

---

_@Gankra approved on 2025-05-28 17:05_

Apologies for all the delays, this seems to just be... done? I don't see any obvious reasons to complain, other than we should probably add some tests.

---

_Comment by @Gankra on 2025-05-28 17:51_

Ok I've locally confirmed uvw.exe does what it intends, by creating a windows shortcut

```
C:\Users\gankra\dev\uv\target\release\uvw.exe run C:\Users\gankra\dev\tmp\tkinter-app\main.py
```

And double-clicking it. (And changing uvw.exe to uv.exe does indeed spawn a shell)

---

_Comment by @Gankra on 2025-05-28 18:05_

Hmm I guess this is basically as tested as `uvx` is?

---

_Merged by @Gankra on 2025-05-28 18:37_

---

_Closed by @Gankra on 2025-05-28 18:37_

---

_Comment by @Gankra on 2025-05-28 18:37_

Thanks so much for this!

---

_Comment by @CrendKing on 2025-05-29 00:02_

> Hmm I guess this is basically as tested as `uvx` is?

I'm sorry I am not familiar with the code structure of uv. I did not see `uvx` itself have dedicated test, so I didn't start one for `uvw`. I see `uvx --help` is called in `build-binaries.yml`, so I added for `uvw`.

I see there is also a `scripts/smoke-test` which seems like an integration test. Maybe we can add some `uvw` calls there?

---
