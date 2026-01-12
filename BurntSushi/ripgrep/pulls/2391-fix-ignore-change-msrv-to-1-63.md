```yaml
number: 2391
title: "fix(ignore): change msrv to 1.63"
type: pull_request
state: closed
author: ctrlaltf24
labels: []
assignees: []
base: master
head: ignore-msrv-1.63
created_at: 2023-01-05T22:21:25Z
updated_at: 2023-01-11T19:57:20Z
url: https://github.com/BurntSushi/ripgrep/pull/2391
synced_at: 2026-01-12T18:23:14Z
```

# fix(ignore): change msrv to 1.63

---

_@ctrlaltf24_

Recent changes introduced by version 0.4.19 require rust 1.63. This formalizes that requirement.

See https://github.com/BurntSushi/ripgrep/commit/e95254a86f484eec663be4714924d91d3cf39cb8#commitcomment-95170910 for more context

See https://github.com/rust-lang/api-guidelines/discussions/231 for community discussion for how changes to MSRV impact versioning. Would be nice to bump MINOR to avoid compatibility issuers, however ignore is pre-v1, so you can do what you want.

---

_Comment by @atouchet on 2023-01-05 23:39_

If the ignore crate has the same MSRV as ripgrep I believe this should be 1.65 rather than 1.63: https://github.com/BurntSushi/ripgrep/blob/master/Cargo.toml#L20

---

_Comment by @ctrlaltf24 on 2023-01-06 01:03_

1.63 works with the current version of ignore.

---

_Comment by @iitalics on 2023-01-11 18:33_

encountered this when trying to build `cargo-make`, so FYI this affects more than just ripgrep

---

_Comment by @BurntSushi on 2023-01-11 18:41_

> however ignore is pre-v1, so you can do what you want

This is not accurate in the Cargo ecosystem. Bumping the leftmost non-zero number in a version results in a semver incompatible change. `ignore`'s leftmost non-zero number is the minor version, so bumping that would result in putting out a semver incompatible release. I have no desire to do such things and will never do it just because of an MSRV bump. So even though `ignore` is pre-v1, it is not "you can do what you want." Bumping the patch version still needs to produce a semver compatible release.

> If the ignore crate has the same MSRV as ripgrep I believe this should be 1.65 rather than 1.63

I'm not quite ready to make a decision about this. And in particular, whatever decision I do make, I'd like to apply it to all the crates in this repo. Maintaining different MSRVs for each sounds like a maintenance burden to me.

---

_Closed by @BurntSushi on 2023-01-11 18:41_

---

_Comment by @ctrlaltf24 on 2023-01-11 19:14_

Thanks for your care and maintenance, even if it is just a hobby project!

>  I have no desire to do such things and will never do it just because of an MSRV bump. 

Don't blame ya, MSRV compatibility is an unsolved issue. https://blog.illicitonion.com/rust-minimum-versions-semver-is-a-lie/

> I'm not quite ready to make a decision about this. And in particular, whatever decision I do make, I'd like to apply it to all the crates in this repo. Maintaining different MSRVs for each sounds like a maintenance burden to me.

One MSRV for all crates sounds sensible, that way the CI can catch it and us developers don't need to think about it.

FYI tools like https://crates.io/crates/cargo-msrv exists to help detect which rust version is required for a given codebase.

Have a good one!

---

_Comment by @BurntSushi on 2023-01-11 19:33_

Thank you. :-) I'm on libs-api and have been part of the MSRV discussions for many years.

---

_Comment by @ctrlaltf24 on 2023-01-11 19:57_

> I'm on libs-api and have been part of the MSRV discussions for many years.

Ouch. Thanks for that too!

---
