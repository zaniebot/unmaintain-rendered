```yaml
number: 14619
title: "windows_exception: Improve async signal safety"
type: pull_request
state: merged
author: geofft
labels: []
assignees: []
merged: true
base: main
head: geofft/windows-exception-async-signal-safe
created_at: 2025-07-14T23:24:53Z
updated_at: 2025-07-17T01:39:22Z
url: https://github.com/astral-sh/uv/pull/14619
synced_at: 2026-01-10T06:53:02Z
```

# windows_exception: Improve async signal safety

---

_Pull request opened by @geofft on 2025-07-14 23:24_

It's not as bad as I feared to bypass libsys's stderr. (There's still a lock in libsys's backtrace, which might also not be too bad to bypass.)

---

_Review requested from @BurntSushi by @geofft on 2025-07-14 23:24_

---

_Comment by @geofft on 2025-07-14 23:37_

As discussed over in https://github.com/astral-sh/uv/pull/14582#discussion_r2205125048 , this reimplements the important parts of the logic in https://github.com/rust-lang/rust/blob/master/library/std/src/sys/stdio/windows.rs with some simplifying assumptions.

> I do not know if the underlying syscall (`WriteConsoleW`) is async-signal-safe on Windows, but this will at least avoid the possible deadlock that can occur by using `eprintln!`.

ReactOS's `WriteConsole` implementation says:
```
    /*
     * For optimization purposes, Windows (and hence ReactOS, too, for
     * compatibility reasons) uses a static buffer if no more than eighty
     * bytes are written. Otherwise a new buffer is allocated.
     * This behaviour is also expected in the server-side.
     */
```

"allocated" here means using `CsrAllocateCaptureBuffer`, which allocates out of some shared heap with CSRSS, which handles the console. I am not sure if it locks. We could constrain our writes to 80 bytes at a time, I guess.

---

_Comment by @geofft on 2025-07-14 23:41_

FWIW I tested with the same forced segfault as in the last PR, with
* running this in a default cmd.exe prompt (codepage 437)
* `chcp 65001` to switch to the UTF-8 code page (which the internet tells me does not work great in the old console host, but it works well enough to test)
* `target\debug\uv run python 2> file.txt` (`cmd.exe` implements sh-style syntax for this)

---

_Review requested from @samypr100 by @geofft on 2025-07-15 00:01_

---

_Review comment by @BurntSushi on `crates/uv/src/windows_exception.rs`:79 on 2025-07-15 12:20_

Would it make sense to make this a bit smaller? Maybe 512 bytes? The only reason I ask about it is because I know we've have stack size problems on Windows.

---

_@BurntSushi approved on 2025-07-15 12:25_

This is awesome work! Excellent docs and comments. And I appreciate the `SAFETY` comments. :-)

I do agree that it might be wise to do better with `backtrace` if that's possible. If it were just a lock specific to backtrace I don't think I'd be too worried about it (it seems unlikely we'd be in the middle of generating a backtrace when getting a signal), but if it's doing allocs, I guess the allocator could be holding a lock. That's annoying.

---

_@geofft reviewed on 2025-07-15 18:14_

---

_Review comment by @geofft on `crates/uv/src/windows_exception.rs`:79 on 2025-07-15 18:14_

Changed this to 80 bytes to avoid allocations (as mentioned above).

Also, I tried a different tactic for the format strings, wondering if this is more or less readable overall than the previous version. (`rustfmt` wants to expand the destructuring to one variable per line if it's more than a couple variables, and I didn't think this justified reconfiguring it or opting out.)

---

_@BurntSushi reviewed on 2025-07-15 18:30_

---

_Review comment by @BurntSushi on `crates/uv/src/windows_exception.rs`:79 on 2025-07-15 18:30_

Thank you!

I think I prefer your change weakly over what it was before, since while it takes up more space, it seems more resistant to accidental mistakes.

---

_Review comment by @samypr100 on `crates/uv/src/windows_exception.rs`:233 on 2025-07-17 00:46_

```suggestion
    writeln!(e, " x27={X27 :016x} x28={X28:016x}")?;
```

---

_@samypr100 reviewed on 2025-07-17 00:46_

---

_Review comment by @samypr100 on `crates/uv/src/windows_exception.rs`:79 on 2025-07-17 00:55_

Avoding the allocations makes sense to me to avoid a potential secondary crash given the much more limited stack space available in SEH

---

_@samypr100 reviewed on 2025-07-17 00:55_

---

_@samypr100 approved on 2025-07-17 00:55_

---

_@geofft reviewed on 2025-07-17 01:31_

---

_Review comment by @geofft on `crates/uv/src/windows_exception.rs`:233 on 2025-07-17 01:31_

oh no thank you!!

---

_Merged by @geofft on 2025-07-17 01:39_

---

_Closed by @geofft on 2025-07-17 01:39_

---

_Branch deleted on 2025-07-17 01:39_

---
