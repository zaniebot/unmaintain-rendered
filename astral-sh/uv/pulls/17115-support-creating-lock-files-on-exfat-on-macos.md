```yaml
number: 17115
title: Support creating lock files on ExFAT on MacOS
type: pull_request
state: merged
author: EliteTK
labels:
  - bug
assignees: []
merged: true
base: main
head: tk/fix-macos-exfat-lockfile
created_at: 2025-12-12T23:04:34Z
updated_at: 2025-12-15T14:05:08Z
url: https://github.com/astral-sh/uv/pull/17115
synced_at: 2026-01-10T05:49:14Z
```

# Support creating lock files on ExFAT on MacOS

---

_Pull request opened by @EliteTK on 2025-12-12 23:04_

## Summary

Partially address #16859 by falling back to simply creating the lock file and then attempting to apply permissions in cases where the temporary lockfile cannot be renamed without overwriting (persist_noclobber) due to lack of underlying support from the filesystem.

I've also improved the error handling, but I think the `as_io_error` thing isn't a good idea[^1]. Something for another PR though.

Assuming the error handling PR came first, the error from the issue would look like:

``` bash session
$ uv init --bare --cache-dir build/uv/cache -v
DEBUG uv 0.9.17+29 (9f1640aa7 2025-12-12)
error: Could not acquire lock
  Caused by: Could not open or create lock file at `build/uv/cache/.lock`
  Caused by: Could not persist temporary file `build/uv/cache/.tmpegtM2h`
  Caused by: Operation not supported (os error 45)
```

I think this is more helpful.

## Test Plan

Manually on MacOS with an ExFAT partition.

~~~ bash session
$ hdiutil create -size 1g -fs ExFAT -volname EXFATDISK exfat.dmg
$ hdiutil attach exfat.dmg
$ cd /Volumes/EXFATDISK
$ uv init --bare --cache-dir build/uv/cache -v 
~~~

[^1]: In summary, I noticed that all the `as_io_error` implementations seem to combine disparate underlying sources of errors and are used to check for cases where there is only one appropriate source for the error case. For example, in `crates/uv-cache/src/lib.rs` `as_io_error` is being used to check if _shared locking_ is unsupported, which only has one source: `LockedFileError::Lock`, but `as_io_error` will also include other sources of errors causing the check to actually be broader than appropriate.

---

_Review requested from @konstin by @EliteTK on 2025-12-12 23:04_

---

_Label `bug` added by @EliteTK on 2025-12-12 23:04_

---

_Review comment by @konstin on `crates/uv-cache/src/lib.rs`:50 on 2025-12-14 21:26_

nit: We usually have one top level error enum per module or crate. It's a developer convenience that allows the functions and methods in the module or crate to all return the same error type, where it's easier to add new variants to a single type.

---

_Review comment by @konstin on `crates/uv-fs/src/locked_file.rs`:48 on 2025-12-14 21:28_

Does fs_err already say which one it is? If so, we can do `[error(transparent)]` to avoid repeating the message.

---

_Review comment by @konstin on `crates/uv-fs/src/locked_file.rs`:245 on 2025-12-14 21:30_

Should we use `fs_err::set_permissions` instead? This will include the failed path in the warning.

---

_Review comment by @konstin on `crates/uv-fs/src/locked_file.rs`:286 on 2025-12-14 21:33_

You mentioned something about `io::ErrorKind` not capturing these, can you add a comment about this and potentially file an issue with rust-lang/rust? I looked for both NOTSUP and INVAL but found no results.

---

_Review comment by @konstin on `crates/uv-fs/src/locked_file.rs`:329 on 2025-12-14 21:35_

Can you move the permissions to a constant, to avoid them getting out of sync when we may change the permission set again?

---

_@konstin approved on 2025-12-14 21:35_

---

_@EliteTK reviewed on 2025-12-15 09:35_

---

_Review comment by @EliteTK on `crates/uv-cache/src/lib.rs`:50 on 2025-12-15 09:35_

I'm not sure I agree with the justification, but I will adjust to fit the style for the time being.

---

_@EliteTK reviewed on 2025-12-15 10:05_

---

_Review comment by @EliteTK on `crates/uv-fs/src/locked_file.rs`:286 on 2025-12-15 10:05_

I will add the comment, I was on the fence but you persuaded me :)

Regarding upstream, the problem is complicated and this topic seems to have been discussed at length already: https://github.com/rust-lang/rust/pull/78880

The fundamental issues are:

* The underlying OS APIs are wildly inconsistent in how they use error values. For example, on Linux:
  * `setxattr`[^setxattr] uses `ENOTSUP` for "The namespace prefix of name is not valid", a role traditionally taken by `EINVAL`, and also uses it for "Extended attributes are not supported by the filesystem, or are disabled", which is a more conventional mapping.
  * Meanwhile `renameat2`[^renameat2] on Linux uses `EINVAL` for "An invalid flag was specified in flags" and for "The filesystem does not support one of the flags in flags."
* Different operating systems assign different error values to different conditions. As you can see here `renameatx_np`[^renameatx_np] on MacOS uses `ENOTSUP` for "flags has a value that is not supported by the file system."
* Lastly, `ErrorKind::Unsupported` could be mapped to by multiple different errno values:
  * `ENOTSUP` Operation not supported.
  * `EOPNOTSUPP` Operation not supported on socket (Which on Linux, but not on MacOS, has the same value as `ENOTSUP`.
  * `EPFNOSUPPORT` Protocol family not supported.
  * `EPROTONOSUPPORT` Protocol not supported.
  * `ESOCKTNOSUPPORT` Socket type not supported.
  * `EAFNOSUPPORT` Address family not supported.
  
  But it's not clear if it would still be meaningful if you mapped all of these to one kind.
* There is a fundamental ambiguity here between `EINVAL` - you just passed something nonseniscal, and the concept of "unsupported". For example, the APIs are unified, despite there being fundamental differences e.g. between stream and datagram sockets, so you can attempt to perform operations that don't make sense with the current socket type, these get marked as `EOPNOTSUPP` even though it could be argued they should be `EINVAL`.

So I am not certain there is anything meaningful we can add for upstream, the situation isn't great and there aren't many good solutions which don't involve going further upstream, and breaking userspace APIs is taboo. I think we are kind of stuck here.

[^setxattr]: [setxattr(2)](https://man7.org/linux/man-pages/man2/setxattr.2.html)
[^renameat2]: [renameat2(2)](https://man7.org/linux/man-pages/man2/rename.2.html)
[^renameatx_np]: [renameatx_np(2)](https://www.unix.com/man_page/mojave/2/renamex_np/)

---

_@EliteTK reviewed on 2025-12-15 11:00_

---

_Review comment by @EliteTK on `crates/uv-fs/src/locked_file.rs`:245 on 2025-12-15 11:00_

Ah, we can't easily. `NamedTempFile::as_file()` returns a `&File` and the only way to make an fs_err::File is using `fs_err::File::from_parts(file: File, path: P)`. I will just pass the path as a parameter and print it.

---

_Review comment by @konstin on `crates/uv-fs/src/locked_file.rs`:245 on 2025-12-15 11:24_

I see, I had only looked at the second callsite

---

_@konstin reviewed on 2025-12-15 11:24_

---

_@konstin reviewed on 2025-12-15 11:29_

---

_Review comment by @konstin on `crates/uv-fs/src/locked_file.rs`:286 on 2025-12-15 11:29_

Thanks for write-up!

---

_@konstin reviewed on 2025-12-15 12:07_

---

_Review comment by @konstin on `crates/uv-cache/src/lib.rs`:50 on 2025-12-15 12:07_

This isn't a rule or anything fwiw, it's more a pattern that seems common, and there's definitely exceptions.

---

_Merged by @EliteTK on 2025-12-15 14:05_

---

_Closed by @EliteTK on 2025-12-15 14:05_

---

_Branch deleted on 2025-12-15 14:05_

---
