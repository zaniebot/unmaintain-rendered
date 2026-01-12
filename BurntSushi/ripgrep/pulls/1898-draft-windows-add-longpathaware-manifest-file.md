```yaml
number: 1898
title: "[Draft] Windows: Add `longPathAware` manifest file"
type: pull_request
state: closed
author: ChrisDenton
labels: []
assignees: []
draft: true
base: master
head: manifest
created_at: 2021-06-16T09:02:14Z
updated_at: 2024-08-02T01:14:28Z
url: https://github.com/BurntSushi/ripgrep/pull/1898
synced_at: 2026-01-12T18:23:14Z
```

# [Draft] Windows: Add `longPathAware` manifest file

---

_@ChrisDenton_

This adds a manifest file which enables support for long paths, without needing any special handling in the application's code.

Building with this manifest requires the following black magic (and an MSVC compatible linker):

```
cargo rustc --release -- -C link-arg="/MANIFEST:EMBED" -C link-arg="/MANIFESTINPUT:WinManifest.xml"
```

The PR is a draft because:
* I'm uncertain how to nicely integrate this with the cargo build system without needing to type the correct commands each time. I guess it isn't too bad if copy/pasting from the readme? Or a script might be better?
* Either way, it'd also be good to integrate it with the ci but I'm unsure how best to do that.

# Limitations

This still requires the ripgrep user to also enable long path support. See: [Enable Long Paths in Windows 10, Version 1607, and Later](https://docs.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=cmd#enable-long-paths-in-windows-10-version-1607-and-later).

As far as I know, the commands must be typed on the command line because `.cargo/config.toml` and the `rustflags` environment variable pass link args to all linker invocation. There's no way to apply them only for a particular binary.

---

_Comment by @BurntSushi on 2021-06-16 14:06_

cc @ehuss Do you have any thoughts on how to do this better? Or is `cargo rustc` required here?

---

_Review comment by @BurntSushi on `WinManifest.xml`:39 on 2021-06-16 14:06_

Is this stanza necessary? What happens if we don't include it?

---

_Review comment by @BurntSushi on `WinManifest.xml`:15 on 2021-06-16 14:07_

Could you add a comment explaining how to update this stanza when a new version of Windows comes out?

---

_Review comment by @BurntSushi on `WinManifest.xml`:28 on 2021-06-16 14:07_

Will this have any harmful effects for versions of Windows older than Windows 10 v 1607?

---

_Review comment by @BurntSushi on `WinManifest.xml`:7 on 2021-06-16 14:08_

Is this required? I try not to put the version number in too many places. Right now, it's in exactly two places, and in the most recent release, I forgot to update one of them.

If it's unavoidable, then I think adding a step to `RELEASE-CHECKLIST.md` would be the right thing to do.

---

_@BurntSushi requested changes on 2021-06-16 14:16_

Amaaazing! I've been hoping someone with more Windows knowledge than myself would put something like this together. If `cargo rustc` is indeed the best we can do, then I definitely think this is still worth doing and adding a note in the README for it. Ideally, we could provide some nicer way of building without requiring the user to pass linker arguments.

I would also request that we create a new top-level directory called `windows` and drop this file there, instead of putting it at the top-level itself. Putting a README in that directory explaining the files would be bonus points. :-) 

> This still requires the ripgrep user to also enable long path support.

I think we will want to include these instructions in the README. The fact that a user has to edit their registry for this seems absolutely bonkers to me though.

> Either way, it'd also be good to integrate it with the ci but I'm unsure how best to do that.

I think we want it in both the normal build workflow and the release workflow. For the release workflow for example, I think you'll want to add a `if: matrix.os != 'windows-2019'` to [this block](https://github.com/BurntSushi/ripgrep/blob/e33d6e73f57d498c65abf83b7fa74ee35f835bcf/.github/workflows/release.yml#L136-L137), and then add another block with `if: matrix.os == 'windows-2019'` that uses `cargo rustc` instead. Modifying the `ci` workflow will be more tedious, but I think it's the same idea. Maybe splitting the build command to an env var (`cargo build` by default, `cargo rustc` for Windows MSVC) would be better. Similarly for the release workflow. (And maybe my suggestion above is wrong, because it would try to use this manifest with the GNU builds too.)

---

_@ChrisDenton reviewed on 2021-06-16 14:37_

---

_Review comment by @ChrisDenton on `WinManifest.xml`:39 on 2021-06-16 14:37_

Sure, we can exclude it. The linker will add it back anyway.

---

_@ChrisDenton reviewed on 2021-06-16 14:37_

---

_Review comment by @ChrisDenton on `WinManifest.xml`:28 on 2021-06-16 14:37_

In that case it will be unrecognised so it should be ignored. I will test on a Windows 7 VM to double check.

---

_@ChrisDenton reviewed on 2021-06-16 14:38_

---

_Review comment by @ChrisDenton on `WinManifest.xml`:7 on 2021-06-16 14:38_

Hmm, I'm actually unsure here. [The docs](https://docs.microsoft.com/en-us/windows/win32/sbscs/application-manifests) list it as "required" but excluding `assemblyIdentity` entirely still seems to work. I was just a bit nervous about breaking with the docs.

EDIT: In any case, I don't think the version actually has to be accurate or meaningful. It could just be set to `0.0.0.0`

---

_Comment by @lespea on 2021-06-16 15:02_

Couldn't you use something like https://crates.io/crates/embed-resource to avoid the explicit flag requirement?

---

_Comment by @ChrisDenton on 2021-06-16 15:08_

Yeah, that might be easier! It would save having to mess about with the linker. I'll try it.

---

_Comment by @BurntSushi on 2021-06-16 15:08_

@lespea I personally don't know anything about this, so "couldn't you use" is kind of framing this in the wrong direction. I don't know what we can and can't use. I'm not an expert here.

With that said, looking at `embed-resource` and its transitive dependencies does not inspire confidence. And it looks like it pulls in C code? I'd rather not do that.

---

_Comment by @lespea on 2021-06-16 15:10_

@BurntSushi oh sorry that was mostly directed at Chris.  I haven't used embed-resource personally I just remember coming across it and kept it in the back of my mind for the future.  Afaik it should only be used during build and wouldn't add anything to the binary itself, but I'm no expert in the matter... I was just trying to make the builds smoother for windows.

---

_Comment by @BurntSushi on 2021-06-16 15:14_

Yeah, I'd rather not use it. The whole thing is built on something written by someone I do not trust. I'm not going to say anything more on that matter publicly. (And the README specifically warns about [memory corruptions](https://github.com/nabijaczleweli/rust-embed-resource/issues/20#issuecomment-686496753).)

---

_Review comment by @BurntSushi on `WinManifest.xml`:7 on 2021-06-16 15:24_

Let's go with `0.0.0.0` for now until someone complains with a good reason to change what we're doing. :-)

---

_@BurntSushi reviewed on 2021-06-16 15:24_

---

_@BurntSushi reviewed on 2021-06-16 15:25_

---

_Review comment by @BurntSushi on `WinManifest.xml`:39 on 2021-06-16 15:25_

Great. General philosophy here is to just keep this file as small as possible so that it does the absolute minimum thing required.

---

_Comment by @ChrisDenton on 2021-06-16 19:08_

Ok, second draft. I've added the command to `release.yml` using explicit test for `win-msvc` or `win32-msvc` so as to avoid breaking when using gnu. Not sure if that's the best choice though. The `ci.yml` is trickier because it builds everything in the workspace but `cargo rustc` is used to build only one thing.

I've started writing some documentation but it could probably use some more work. I've also tentatively added some small utility files to make things a bit easier for users.
* `build.bat` can be used to do the `cargo rustc` magic.
* `LongPathsEnabled.reg` provides an easier way for users to enable long path support for their machine (just double-click the file in the Windows file explorer).

---

_Comment by @ehuss on 2021-06-16 19:58_

> cc @ehuss Do you have any thoughts on how to do this better? Or is `cargo rustc` required here?

I'm not too familiar with embedding Windows manifests.  Long-file path can be tricky (for example, AFAIK Rust's `Command` does not support it, even with this change), and if you have any C libraries, they need to be audited.

You can try using [RUSTFLAGS](https://doc.rust-lang.org/nightly/cargo/reference/config.html#buildrustflags) (either the environment variable or config file), but that can have issues.

There is an unstable feature for setting flags like these. It is described [here](https://doc.rust-lang.org/nightly/cargo/reference/unstable.html#rustc-link-arg-bin), and essentially you have a build.rs script that prints `cargo:rustc-link-arg-bin=rg=linker flag here`.  Hopefully it will get stabilized soonish.


---

_Comment by @ChrisDenton on 2021-06-16 20:13_

> AFAIK Rust's `Command` does not support it, even with this change

Imho, that's an issue with Rust's current implementation. It can be fixed, although doing so isn't as trivial as I'd like,

> There is an unstable feature for setting flags like these...

That would be ideal! It would greatly simplify things when stabilized. @BurntSushi would it be worth pausing this PR until that feature is ready?

---

_Comment by @BurntSushi on 2021-06-16 22:47_

@ChrisDenton Yeah, let's post-pone this until we have a better path here. This has been a long-standing bug, so I think we can wait a bit longer.

With respect to the `ci` workflow, if it's easier to just add a separate job for only Windows/MSVC that builds with `cargo rustc`, then that would be OK. But then again, if we're pausing this PR, then we shouldn't need to bother with `cargo rustc` at all.

So I guess I'll close this for now. I am now also subscribed to https://github.com/rust-lang/cargo/issues/9426, which appears to be the issue tracking the stabilization of passing link arguments through `build.rs`. Thanks for the tip @ehuss!

---

_Closed by @BurntSushi on 2021-06-16 22:47_

---

_Branch deleted on 2024-08-02 01:14_

---
