---
number: 16838
title: "Add brew specific message for `uv self update`"
type: pull_request
state: closed
author: terror
labels:
  - error messages
assignees: []
base: main
head: self-update-brew
created_at: 2025-11-24T17:38:16Z
updated_at: 2025-12-04T18:44:07Z
url: https://github.com/astral-sh/uv/pull/16838
synced_at: 2026-01-07T13:12:19-06:00
---

# Add brew specific message for `uv self update`

---

_Pull request opened by @terror on 2025-11-24 17:38_

Resolves https://github.com/astral-sh/uv/issues/16833

`uv self update` could error with a better message if it knows the user has installed it with brew. This diff adds an `InstallSource` we can use the detect where a `uv` binary comes from and augment error messages with better context. We're only adding brew for now, but it could easily be extend with other detection heuristics.

---

_@terror reviewed on 2025-11-24 17:46_

---

_Review comment by @terror on `crates/uv/src/install_source.rs`:48 on 2025-11-24 17:46_

We could also run this command 🤔 

---

_@zanieb reviewed on 2025-11-24 19:30_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1235 on 2025-11-24 19:30_

I think I'd use the `hint: ...` formatting for this.

---

_@zanieb reviewed on 2025-11-24 19:31_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1233 on 2025-11-24 19:31_

I think I'd retain the core message here, e.g., "uv was installed through an external package manager and cannot update itself".

---

_Review comment by @zanieb on `crates/uv/src/install_source.rs`:44 on 2025-11-24 19:31_

Why include the URL?

---

_@zanieb reviewed on 2025-11-24 19:31_

---

_@zanieb reviewed on 2025-11-24 19:33_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1235 on 2025-11-24 19:33_

I might have the addition of a hint be conditional on detecting which external package manager was used and keep the core message unbranched?

---

_Comment by @zanieb on 2025-11-24 19:34_

Running the external tool to perform the update seems interesting but can definitely happen after / separately.

Maybe it's worth looking at what tailscale does?

---

_Closed by @zanieb on 2025-11-24 19:34_

---

_Reopened by @zanieb on 2025-11-24 19:34_

---

_Comment by @zanieb on 2025-11-24 19:34_

(sorry wrong button)

---

_@zanieb reviewed on 2025-11-24 19:35_

---

_Review comment by @zanieb on `crates/uv/src/install_source.rs`:32 on 2025-11-24 19:35_

cc @woodruffw on this approach

---

_Comment by @terror on 2025-11-24 19:36_

> Running the external tool to perform the update seems interesting but can definitely happen after / separately.
> 
> Maybe it's worth looking at what tailscale does?

Yeah I'll follow up with this!

---

_@terror reviewed on 2025-11-24 19:36_

---

_Review comment by @terror on `crates/uv/src/install_source.rs`:44 on 2025-11-24 19:36_

No specific reason, just for additional context. Removed it!

---

_@terror reviewed on 2025-11-24 19:37_

---

_Review comment by @terror on `crates/uv/src/lib.rs`:1233 on 2025-11-24 19:37_

Changed it back to the original formatting.

---

_@terror reviewed on 2025-11-24 19:41_

---

_Review comment by @terror on `crates/uv/src/install_source.rs`:32 on 2025-11-24 19:41_

Some things to note:
- `OsStr` is case-sensitive, we may not work for case-insensitive file systems (someone would have to manually rename the path, unlikely, but something worth considering)
- We could probably avoid allocating components and early-return, but this hyper-optimization territory

---

_@terror reviewed on 2025-11-24 19:45_

---

_Review comment by @terror on `crates/uv/src/lib.rs`:1235 on 2025-11-24 19:45_

Yeah that makes sense, I pulled it this out into a constant to re-use.

---

_@terror reviewed on 2025-11-24 19:46_

---

_Review comment by @terror on `crates/uv/src/install_source.rs`:48 on 2025-11-24 19:46_

See https://github.com/astral-sh/uv/pull/16838#issuecomment-3572411511.

---

_Review comment by @woodruffw on `crates/uv/src/install_source.rs`:32 on 2025-11-24 20:23_

This approach LGTM -- there are definitely some edge cases with custom prefixes, but 99.99% of Homebrew users who use a brew installed uv should have `readlink -f $(which uv)` match `/<prefix>/Cellar/uv/...`.



---

_@woodruffw reviewed on 2025-11-24 20:23_

---

_@woodruffw reviewed on 2025-11-24 20:26_

---

_Review comment by @woodruffw on `crates/uv/src/install_source.rs`:39 on 2025-11-24 20:26_

Noting: I think this works because `current_exe` returns the resolved path on macOS, but this isn't actually guaranteed to be true:

https://doc.rust-lang.org/std/env/fn.current_exe.html#platform-specific-behavior

```
% which uv
/opt/homebrew/bin/uv
% readlink -f $(which uv)
/opt/homebrew/Cellar/uv/0.9.11/bin/uv
```

For full generality, we probably need `std::fs::read_link` or `std::fs::canonicalize` as an intermediate step here.

---

_@terror reviewed on 2025-11-24 20:28_

---

_Review comment by @terror on `crates/uv/src/install_source.rs`:39 on 2025-11-24 20:28_

We do currently canonicalize the path in `from_path`: https://github.com/astral-sh/uv/pull/16838/files#diff-0485cb93b3da92da3ccb4e1b62ea7af254d7bdb056398364d8f0e72cc1ab4301R15-R17

---

_@woodruffw reviewed on 2025-11-24 20:30_

---

_Review comment by @woodruffw on `crates/uv/src/install_source.rs`:39 on 2025-11-24 20:30_

My bad, I missed that! 

---

_@woodruffw reviewed on 2025-11-24 20:31_

---

_Review comment by @woodruffw on `crates/uv/src/install_source.rs`:32 on 2025-11-24 20:31_

(Another alternative to this approach is to do a PATH lookup on `brew`, then match that prefix to the uncanonicalized prefix for `current_exe`. But I think this approach is cleaner and involves fewer system calls.)

---

_@zanieb reviewed on 2025-11-24 20:39_

---

_Review comment by @zanieb on `crates/uv/src/install_source.rs`:32 on 2025-11-24 20:39_

Note in the error case we're rarely concerned about performance. However, I agree we don't need to get fancy here.

---

_@terror reviewed on 2025-11-24 20:40_

---

_Review comment by @terror on `crates/uv/src/install_source.rs`:32 on 2025-11-24 20:40_

I agree the custom-prefix edge cases are possible (albeit rare); if we see reports there we could potentially add a fallback that compares against `brew --prefix` / `brew --cellar`, this would add more overhead/failure modes which is why we aren't doing it already.

---

_@zanieb reviewed on 2025-11-24 22:16_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1235 on 2025-11-24 22:16_

This doesn't match our `hint` styling elsewhere, you should search for it and copy the pattern.

---

_@zanieb reviewed on 2025-11-24 22:17_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1237 on 2025-11-24 22:17_

I think we'll want backticks around the command? And we usually use green for commands, I think?

---

_@terror reviewed on 2025-11-24 22:20_

---

_Review comment by @terror on `crates/uv/src/lib.rs`:1237 on 2025-11-24 22:20_

Yah it looks like we use green elsewhere, just pushed this up :)

---

_@botantony reviewed on 2025-11-24 22:30_

---

_Review comment by @botantony on `crates/uv/src/install_source.rs`:32 on 2025-11-24 22:30_

This approach seems good for Homebrew but I cannot say the same about other package managers . Maybe there can be `UV_INSTALLATION_SOURCE` variable that can be set at compile time to specify a package manager (f.e. binaries compiled with `UV_INSTALLATION_SOURCE=brew cargo build` print "use brew upgrade uv", `UV_INSTALLATION_SOURCE=apt cargo build` prints "use sudo apt upgrade uv", etc.)
And if `UV_INSTALLATION_SOURCE` is not set or invalid it fallbacks to the default message

---

_@terror reviewed on 2025-11-24 22:31_

---

_Review comment by @terror on `crates/uv/src/install_source.rs`:32 on 2025-11-24 22:31_

Yeah other package managers are out of scope for this PR. We should be able to add other heuristics quite easily to `InstallSource`.

---

_Label `error messages` added by @konstin on 2025-11-26 15:57_

---

_Comment by @terror on 2025-12-04 03:29_

@zanieb Was there something else blocking this? 

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1239 on 2025-12-04 14:42_

This still doesn't match our usual hint styling, e.g., https://github.com/astral-sh/uv/blob/2d9fe7ca701f72b35202bd48d5758dd5b5ef12fe/crates/uv/src/commands/project/remove.rs#L470-L472

---

_@zanieb reviewed on 2025-12-04 14:42_

---

_@zanieb approved on 2025-12-04 14:42_

---

_Merged by @zanieb on 2025-12-04 18:44_

---

_Closed by @zanieb on 2025-12-04 18:44_

---

_Referenced in [Homebrew/homebrew-core#257541](../../Homebrew/homebrew-core/pulls/257541.md) on 2025-12-06 14:23_

---
