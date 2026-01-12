```yaml
number: 2787
title: Set up ripgrep for compilation on non-unix, non-windows platforms
type: pull_request
state: merged
author: holzschu
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2024-04-23T09:10:17Z
updated_at: 2024-04-23T17:12:20Z
url: https://github.com/BurntSushi/ripgrep/pull/2787
synced_at: 2026-01-12T18:23:14Z
```

# Set up ripgrep for compilation on non-unix, non-windows platforms

---

_@holzschu_

The code of ripgrep compiles on almost any kind of architecture, including WebAssembly, except in two tiny places related to hyperlinks: 
- when it searches for the local hostname in `hostname.rs`, 
- when it does path canonalization in `hyperlink.rs`.

In both cases, there is a path with `#[cfg(unix)]` and a path with `#[cfg(windows)]`. WebAssembly being neither unix nor windows, compilation fails. This PR adds a third path:
- for localhost, it returns "localhost" instead of calling `gethostname()`, for all environments that are neither unix nor windows.
- for path canonalization, it uses `std::os::wasi::ffi::OsStrExt` instead of `std::os::unix::ffi::OsStrExt`, which is not available; this part is specific for wasm32-wasi. It could be possible to deactivate path canonalization entirely for architectures that are neither unix nor windows, but I aimed for the smallest working change.

With these two changes, it's possible to compile ripgrep with `cargo build --target wasm32-wasi` (and, more importantly, it works in wasm32-wasi environments). 

---

_Comment by @ltrzesniewski on 2024-04-23 09:39_


> It could be possible to deactivate path canonalization entirely for architectures that are neither unix nor windows

`std::path::absolute` [should be stabilized soon](https://github.com/rust-lang/rust/issues/92750), and I suppose it will replace canonicalization in ripgrep, which will both simplify and optimize this part of the code.


---

_@blyxxyz reviewed on 2024-04-23 10:28_

---

_Review comment by @blyxxyz on `crates/printer/src/hyperlink.rs`:857 on 2024-04-23 10:28_

The [WASI spec](https://github.com/WebAssembly/wasi-filesystem/blob/174dbd7fa134672e594fd38dba9acfb53afaf312/imports.md#import-interface-wasifilesystemtypes020) seems to ban absolute paths and the [stdlib](https://github.com/rust-lang/rust/blob/c67277301c896857d0534f2bb7431680796833fb/library/std/src/sys/pal/wasi/fs.rs#L655) doesn't implement `canonicalize`. I don't know how black-and-white this is (especially in the long term) but right now this might as well be stubbed out with `None`.

When running on top of Windows I imagine you'd want to return a Windows path here, even though WASI is more Unix-like internally.

---

_Review comment by @BurntSushi on `crates/printer/src/hyperlink.rs`:857 on 2024-04-23 12:05_

I'm curious how `std::path::absolute` will work here. It seems to depend on what `std::env::current_dir` will return, and that does seem to have an implementation on WASI: https://github.com/rust-lang/rust/blob/eff958c59e8c07ba0515e164b825c9001b242294/library/std/src/sys/pal/wasi/os.rs#L71-L95

But I agree, for now, this should just always return `None` and emit a DEBUG log that hyperlinks aren't supported on WASI.

---

_Review comment by @BurntSushi on `crates/cli/src/hostname.rs`:29 on 2024-04-23 12:06_

I'm not a huge fan of giving wrong answers like this. And it seems like WASI can't really support hyperlinks anyway, due to the ban on absolute paths. So I think this change should probably be reverted.

---

_@BurntSushi requested changes on 2024-04-23 12:08_

I think the idea here seems good, but as I mentioned in the comments, it seems like hyperlinks and WASI are fundamentally incompatible.

Also, can we add CI support WASI so that we don't regress here? You should be able to lift this nearly directly: https://github.com/BurntSushi/memchr/blob/20ef11fa92d8b393735f10906c436a8ce6e792a4/.github/workflows/ci.yml#L133-L163

If tests don't work, that's okay. But at least having a `cargo build --verbose` in CI would be nice.

---

_@blyxxyz reviewed on 2024-04-23 13:30_

---

_Review comment by @blyxxyz on `crates/printer/src/hyperlink.rs`:857 on 2024-04-23 13:30_

AFAICT it works like this:
- WASI syscalls take a directory descriptor and a path relative to that directory descriptor.
- The WASI runtime gives the WASM binary a list of directory descriptors plus their actual original paths ([preopens](https://github.com/WebAssembly/wasi-filesystem/blob/e79b05803e9ffd3b0cfdc0a8af20ac743abbe36a/imports.md#import-interface-wasifilesystempreopens020)). So the runtime grants selective access. It could easily pretend the filesystem is smaller than it really is, chroot-style, but it doesn't have to: it can grant a `/home/user` descriptor with the `/home/user` path.
- wasi-libc takes absolute paths and [resolves](https://github.com/WebAssembly/wasi-libc/blob/9e8c542319242a5e536e14e6046de5968d298038/libc-bottom-half/sources/preopens.c#L182) them to a directory descriptor + relative path based on the longest matching prefix from the preopens, if any. (This is inspired by [libpreopen](https://github.com/musec/libpreopen).)
- wasi-libc also [emulates](https://github.com/WebAssembly/wasi-libc/blob/9e8c542319242a5e536e14e6046de5968d298038/libc-bottom-half/sources/getcwd.c) a working directory in WASM-space.

So it's not as bad as I thought. Thanks to the emulation absolute paths should for the most part just work as long as access to that part of the filesystem is granted, particularly on Unix. (Except for missing syscalls I guess.)

---

_@BurntSushi reviewed on 2024-04-23 14:11_

---

_Review comment by @BurntSushi on `crates/printer/src/hyperlink.rs`:857 on 2024-04-23 14:11_

Interesting. So `std::env::current_dir()` works, and thus `std::path::absolute` might in turn work here as well.

---

_@holzschu reviewed on 2024-04-23 14:12_

---

_Review comment by @holzschu on `crates/cli/src/hostname.rs`:29 on 2024-04-23 14:12_

The problem I was trying to fix here, and that's also a question I have about setting up CI, is that the current version of the code causes a compilation error:    
```
Compiling grep-cli v0.1.10 (/Users/holzschu/src/Xcode_iPad/ripgrep_clone/crates/cli)
error[E0308]: mismatched types
  --> crates/cli/src/hostname.rs:28:9
   |
16 |   pub fn hostname() -> io::Result<OsString> {
   |                        -------------------- expected `Result<OsString, std::io::Error>` because of return type
...
28 | /         io::Error::new(
29 | |             io::ErrorKind::Other,
30 | |             "hostname could not be found on unsupported platform",
31 | |         )
   | |_________^ expected `Result<OsString, Error>`, found `Error`
   |
   = note: expected enum `Result<OsString, std::io::Error>`
            found struct `std::io::Error`
help: try wrapping the expression in `Err`
   |
28 ~         Err(io::Error::new(
29 |             io::ErrorKind::Other,
30 |             "hostname could not be found on unsupported platform",
31 ~         ))
   |

For more information about this error, try `rustc --explain E0308`.
```
So if the goal of setting up CI is to get the same result after the PR as before, that's not going to happen. 

---

_@BurntSushi reviewed on 2024-04-23 14:20_

---

_Review comment by @BurntSushi on `crates/cli/src/hostname.rs`:29 on 2024-04-23 14:20_

Oh I see. I think you just want `Err(io::Error::new(...))`, as suggested in the error message you're showing. This particular code path is probably entirely untested in CI at present, and thus the fact that it is wrong now and has always been wrong was never noticed.

My main objection was to silently using a potentially incorrect value. I'm open to doing that in the future to get hyperlinks working in WASI after we've considered the possible alternatives. But since `std::fs::canonicalize` isn't going to work on WASI right now anyway, we should just avoid changing the semantic intent of this code.

---

_Review comment by @holzschu on `crates/cli/src/hostname.rs`:29 on 2024-04-23 16:18_

I've done the changes for `hostname.rs`. But then, for symmetry, I think I should have a branch  `#[cfg(not(any(windows, unix)))]`  instead of `#[cfg(target_os = "wasi")]` in `hyperlink.rs`. But what should `from_path()` return in that case? 



---

_@holzschu reviewed on 2024-04-23 16:18_

---

_@BurntSushi reviewed on 2024-04-23 16:21_

---

_Review comment by @BurntSushi on `crates/cli/src/hostname.rs`:29 on 2024-04-23 16:21_

`None`

---

_@BurntSushi reviewed on 2024-04-23 16:21_

---

_Review comment by @BurntSushi on `crates/cli/src/hostname.rs`:29 on 2024-04-23 16:21_

With a DEBUG log message. Mentioned here: https://github.com/BurntSushi/ripgrep/pull/2787#discussion_r1576137687

---

_Renamed from "Set up ripgrep for compilation on WebAssembly (wasm32-wasi)" to "Set up ripgrep for compilation on non-unix, non-windows platforms" by @holzschu on 2024-04-23 16:29_

---

_Comment by @holzschu on 2024-04-23 16:31_

I've changed the files and the title of the PR.
Now, the goal is for ripgrep to compile without error on platforms that are neither unix nor windows (that includes WebAssembly, but there might be other platforms as well.

It will emit an error in `hostname.rs` and a debug message in `hyperlinks.rs`. 

---

_Review comment by @BurntSushi on `.github/workflows/ci.yml`:205 on 2024-04-23 16:34_

Did running tests via `wasmtime` fail? If so this is fine, but if they worked, then we should just include them here.

---

_Review comment by @BurntSushi on `crates/printer/src/hyperlink.rs`:860 on 2024-04-23 16:35_

To be consistent with other log messages, could you please use "hyperlinks" instead of "Hyperlinks"?

---

_Review comment by @BurntSushi on `crates/printer/src/hyperlink.rs`:857 on 2024-04-23 16:35_

This should be using `///`.

---

_Review comment by @BurntSushi on `crates/printer/src/hyperlink.rs`:857 on 2024-04-23 16:35_

And please move this to be directly below the other `from_path` constructors.

---

_@BurntSushi requested changes on 2024-04-23 16:36_

---

_@holzschu reviewed on 2024-04-23 16:53_

---

_Review comment by @holzschu on `.github/workflows/ci.yml`:205 on 2024-04-23 16:53_

Indeed, running tests via `wasmtime` does fail (but compilation works):
```
error[E0433]: failed to resolve: could not find `unix` in `os`
   --> tests/util.rs:197:22
    |
197 |         use std::os::unix::fs::symlink;
    |                      ^^^^ could not find `unix` in `os`
    |
```

---

_@holzschu reviewed on 2024-04-23 16:55_

---

_Review comment by @holzschu on `crates/printer/src/hyperlink.rs`:860 on 2024-04-23 16:55_

This is done.

---

_@holzschu reviewed on 2024-04-23 16:55_

---

_Review comment by @holzschu on `crates/printer/src/hyperlink.rs`:857 on 2024-04-23 16:55_

These changes have been incorporated.

---

_@BurntSushi reviewed on 2024-04-23 17:10_

---

_Review comment by @BurntSushi on `.github/workflows/ci.yml`:205 on 2024-04-23 17:10_

Ah fair enough. We can save that for another day.

---

_@BurntSushi approved on 2024-04-23 17:10_

Thanks!

---

_Comment by @holzschu on 2024-04-23 17:11_

Thank *you* for your patience and pedagogy! 

---

_Merged by @BurntSushi on 2024-04-23 17:12_

---

_Closed by @BurntSushi on 2024-04-23 17:12_

---
