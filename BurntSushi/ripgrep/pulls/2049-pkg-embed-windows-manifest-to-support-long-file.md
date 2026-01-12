```yaml
number: 2049
title: "pkg: embed windows manifest to support long file paths if enabled"
type: pull_request
state: closed
author: thargor
labels:
  - rollup
assignees: []
draft: true
base: master
head: embed-windows-long-file-path-manifest
created_at: 2021-11-04T12:28:29Z
updated_at: 2023-07-08T22:53:05Z
url: https://github.com/BurntSushi/ripgrep/pull/2049
synced_at: 2026-01-12T18:23:14Z
```

# pkg: embed windows manifest to support long file paths if enabled

---

_@thargor_

fixes #364 

Implementation is based on https://gal.hagever.com/posts/windows-long-paths-in-rust/ .

This still requires long file paths to be enabled in the windows registry.

---

_Comment by @thargor on 2021-11-04 12:35_

this is also what https://github.com/nabijaczleweli/cargo-update is using

---

_Comment by @SimplyDanny on 2021-11-18 17:53_

Works fine for me with the registry key `LongPathsEnabled` set to 1.

---

_Review comment by @SimplyDanny on `build.rs`:9 on 2021-11-18 18:04_

This line is [no longer needed](https://doc.rust-lang.org/nightly/edition-guide/rust-2018/path-changes.html) since Rust Edition 2018.

---

_Review comment by @SimplyDanny on `build.rs`:52 on 2021-11-18 18:06_

From what I see browsing through the code base, comments are written as sentences, i.e. they start with a capital letter and end with a period.

---

_@SimplyDanny reviewed on 2021-11-18 18:07_

---

_Review comment by @BurntSushi on `build.rs`:1 on 2021-11-18 18:35_

Why was this moved? Please put it back. :)

---

_Review comment by @BurntSushi on `build.rs`:9 on 2021-11-18 18:35_

Indeed, this should be removed.

---

_Review comment by @BurntSushi on `build.rs`:52 on 2021-11-18 18:37_

Yeah, that's preferred. But what would be better here is more elaboration. Mostly, this comment nearly repeats what the code itself says, without adding much. I think this comment should:

* Say why this is being done.
* Include instruction on how to take advantage of it. If you're someone who is reading this `build.rs` trying to understand it, and we say, "this enables long file paths" without saying that it also requires a registry edit, that might produce some confusion. So let's shine a light where we can.

---

_Review comment by @BurntSushi on `rg-manifest.rc`:2 on 2021-11-18 18:38_

I can't comment on it, but where did the `rg.exe.manifest` file come from? How was it generated?

Also, I think it would be better to put these files into a `windows` sub-directory, instead of polluting the root of the repo even more than it is.

---

_Review comment by @BurntSushi on `rg-manifest.rc`:2 on 2021-11-18 18:41_

Oh I see, GitHub falsely detects it as a binary file, but it's just plain text. Phew. That's good. Yeah, I think just move these into a new `windows` sub-directory. Thanks!

---

_Review comment by @BurntSushi on `build.rs`:53 on 2021-11-18 18:42_

What does this do on non-Windows targets? Even if nothing, it might be nice to gate this on `#[cfg(windows)]` to make it a bit clearer to those reading the code.

---

_Review comment by @BurntSushi on `Cargo.toml`:64 on 2021-11-18 18:43_

I don't think this is needed for non-Windows builds right? Could you please make this a target specific dependency for windows only? Thanks!

---

_@BurntSushi requested changes on 2021-11-18 18:57_

I got through most of this PR review before realizing that it's using a dependency that I will not bring in. I'm sorry. Someone tried this before in #1898. Specifically, see [my comment on that PR](https://github.com/BurntSushi/ripgrep/pull/1898#issuecomment-862466605).

---

_Closed by @BurntSushi on 2021-11-18 18:59_

---

_Comment by @thargor on 2021-11-18 21:47_

@BurntSushi @SimplyDanny thank you for your feedback!

Would an implementation based on https://crates.io/crates/windres (which is based on https://crates.io/crates/find-winsdk ) be acceptable for ripgrep?

---

_Comment by @BurntSushi on 2021-11-18 21:52_

As long as there's nothing written by Jonathan Blow in the dependency tree, the licenses are compatible and the crates are decently maintained. Remember, once this gets added to ripgrep, it's forever mine to maintain, evev if the dependencies go defunct.

So in short: probably, but I'll still need to do a review if a PR comes in.

---

_Reopened by @BurntSushi on 2021-11-18 21:52_

---

_Converted to draft by @thargor on 2021-11-18 22:05_

---

_Comment by @t-rapp on 2022-06-09 13:58_

For anyone interested in long path support: Stumbled over this recent [pull request for rustc](https://github.com/rust-lang/rust/pull/96737) which adds a similar manifest file to the rustc executable. It apparently works for the *-msvc target by using a build script without external libraries.

---

_Comment by @BurntSushi on 2023-07-08 14:27_

@ChrisDenton what are your thoughts on this PR? Is this the right way to support long paths on Windows?

---

_Comment by @ChrisDenton on 2023-07-08 15:17_

Yep. I'd be minded to append `.xml` to the file name to make github happy. And while an xml declaration (`<?xml ...`) isn't strictly necessary, it can make other tools happy. Also linking to [the docs](https://learn.microsoft.com/en-us/windows/win32/sbscs/application-manifests#longpathaware) would make sense.

My only quibble is that it's a lot of dependencies for what can be [a couple of lines](https://github.com/rust-lang/rust/blob/ce519c5945c90596cf429dc2a91c846daeb75cd1/compiler/rustc/build.rs#L23-L24) in a build script (plus a few more lines to apply it conditionally). Though unfortunately windows-gnu does not support this.

---

_Comment by @BurntSushi on 2023-07-08 17:14_

@ChrisDenton Oh yessss! Thank you for that link. I will do what you did for `rustc`. That looks much better.

---

_Label `rollup` added by @BurntSushi on 2023-07-08 17:28_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
