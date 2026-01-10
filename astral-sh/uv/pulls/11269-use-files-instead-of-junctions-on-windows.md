```yaml
number: 11269
title: Use files instead of junctions on Windows
type: pull_request
state: merged
author: charliermarsh
labels:
  - windows
  - no-build
assignees: []
merged: true
base: tracking/060
head: charlie/sym
created_at: 2025-02-06T01:03:40Z
updated_at: 2025-02-08T00:13:21Z
url: https://github.com/astral-sh/uv/pull/11269
synced_at: 2026-01-10T11:10:34Z
```

# Use files instead of junctions on Windows

---

_Pull request opened by @charliermarsh on 2025-02-06 01:03_

## Summary

Instead of using junctions, we can just write files that contain (as the file contents) the target path. This requires a little more finesse in that, as readers, we need to know where to expect these. But it also means we get to avoid junctions, which have led to a variety of confusing behaviors. Further, `replace_symlink` should now be on atomic on Windows.

Closes #11263.


---

_Label `no-build` added by @charliermarsh on 2025-02-06 01:09_

---

_Review requested from @BurntSushi by @charliermarsh on 2025-02-06 02:01_

---

_Review requested from @konstin by @charliermarsh on 2025-02-06 02:01_

---

_Label `windows` added by @charliermarsh on 2025-02-06 02:02_

---

_Marked ready for review by @charliermarsh on 2025-02-06 02:02_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-06 02:18_

---

_Comment by @charliermarsh on 2025-02-06 02:20_

We might be able to remove some of the extra `flock` usages we have on Windows after this, but I want to test and evaluate that separately. The proximate motivation here is that Ofek / Datadog are seeing a really strange behavior in their Windows container whereby we had a junction that... didn't point to anything? Like, it's not that the target was gone, the target was _empty_.

---

_@konstin reviewed on 2025-02-06 10:14_

---

_Review comment by @konstin on `crates/uv-fs/src/lib.rs`:52 on 2025-02-06 10:14_

Does this intend to say "fail if it _does_ exist"?

---

_Review comment by @konstin on `crates/uv-fs/src/lib.rs`:67 on 2025-02-06 10:15_

Do we need a tempdir or can we directly create a tempfile with a random name in the directory?

---

_@konstin approved on 2025-02-06 10:16_

---

_Review comment by @BurntSushi on `crates/uv-cache/src/lib.rs`:380 on 2025-02-06 13:11_

Should this be an error? Or perhaps a WARN log or something?

---

_Review comment by @BurntSushi on `crates/uv-cache/src/lib.rs`:384 on 2025-02-06 13:11_

Same here?

---

_Review comment by @BurntSushi on `crates/uv-cache/src/lib.rs`:371 on 2025-02-06 13:12_

And here.

Although I guess we weren't emitting logs before. I wonder if it might help debugging if something goes wrong here.

---

_Review comment by @BurntSushi on `crates/uv-fs/src/lib.rs`:61 on 2025-02-06 13:20_

Oh, okay, now that I know what you are doing here, I think I can suggest something more bullet proof that isn't too much work. Basically, on Windows, file paths are just arbitrary sequences of `u16` (with some restrictions), just like on Unix, file paths are just arbitrary sequences of `u8`. So the way to avoid the lossy conversion here is to just store the `u16` sequence. You can get the raw `u16` sequence from this API in std: https://doc.rust-lang.org/std/os/windows/ffi/trait.OsStrExt.html

Once you have a `Vec<u16>` build in memory, create a `Vec<u8>` with twice the length and then use `byteorder` to (safely) write the `&[u16]` into a `&mut [u8]`: https://docs.rs/byteorder/latest/byteorder/trait.ByteOrder.html#tymethod.write_u16_into

Then save _that_ to the file.

To go in the other direction, use [`ByteOrder::read_u16_into`](https://docs.rs/byteorder/latest/byteorder/trait.ByteOrder.html#tymethod.read_u16_into) and [`std::os::windows::ffi::OsStringExt`](https://doc.rust-lang.org/std/os/windows/ffi/trait.OsStringExt.html).

When using `byteorder`, I'd suggest picking a specific byteorder (`byteorder::BigEndian`) and always using it. Regardless of the endianness of the host platform. That way, the file format is portable.

---

_@BurntSushi reviewed on 2025-02-06 13:22_

Rolling your own symlinks because Windows-native symlinks are so bad... It's just crazy enough to work! Haha.

I made a suggestion about how to avoid the lossy call here. Sorry about being unclear last night.

I also wonder if perhaps we can emit some more logs in places, but I defer to you on that. I don't have a ton of context on this area of the code.

---

_@charliermarsh reviewed on 2025-02-06 14:31_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:61 on 2025-02-06 14:31_

Awesome, thank you!

---

_Review requested from @Gankra by @charliermarsh on 2025-02-06 17:14_

---

_@charliermarsh reviewed on 2025-02-06 17:19_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:52 on 2025-02-06 17:19_

Yes!

---

_Comment by @charliermarsh on 2025-02-07 02:35_

Ok, I simplified things a bit (I think). Instead of "generically" using these routines to read and write symlinks, they're now methods on the cache specifically framed at writing links to archive entries and reading links to archive entries.

---

_Closed by @charliermarsh on 2025-02-07 02:35_

---

_Reopened by @charliermarsh on 2025-02-07 02:35_

---

_Comment by @charliermarsh on 2025-02-07 02:37_

(I didn't mean to close, accidental click.)

---

_Comment by @ofek on 2025-02-07 02:39_

In the future, would it be possible to detect symlink capability on Windows and use this new approach as a fallback?

---

_Added to milestone `v0.6.0` by @charliermarsh on 2025-02-07 02:56_

---

_Merged by @charliermarsh on 2025-02-08 00:13_

---

_Closed by @charliermarsh on 2025-02-08 00:13_

---

_Branch deleted on 2025-02-08 00:13_

---
