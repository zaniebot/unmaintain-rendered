```yaml
number: 10265
title: Fix multi-user file locking
type: pull_request
state: closed
author: guoci
labels: []
assignees: []
base: main
head: change_umask
created_at: 2025-01-01T20:41:34Z
updated_at: 2025-01-06T18:50:26Z
url: https://github.com/astral-sh/uv/pull/10265
synced_at: 2026-01-12T16:09:12Z
```

# Fix multi-user file locking

---

_@guoci_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Try to resolve #8032 with suggestion in https://github.com/astral-sh/uv/issues/8032#issuecomment-2402907189

## Test Plan

Tested on the Dockerfile (minimal reproducible example) in https://github.com/astral-sh/uv/issues/8032#issue-2574635582


---

_Review requested from @BurntSushi by @zanieb on 2025-01-01 21:43_

---

_Review comment by @BurntSushi on `crates/uv-fs/src/lib.rs`:567 on 2025-01-06 17:16_

Can you use [`nix::sys::stat::umask`](https://docs.rs/nix/latest/nix/sys/stat/fn.umask.html#) here instead? (Otherwise, you're adding a dependency here on `nix` when just `libc` would do. But if we're depending on `nix`, we might as well use the safe API...)

---

_Review comment by @BurntSushi on `crates/uv-fs/src/lib.rs`:561 on 2025-01-06 17:20_

I unfortunately think this is not quite right. `umask` is a _process global_ property, but we might have multiple threads in the same process creating files. So when you twiddle the `umask` like this, you might very well be impacting the creation of other completely unrelated files in an undesirable way.

Is it possible to change the permissions after the file is created instead?

I suppose another alternative here is to suggest that end users change their `umask` when running `uv` if they want it to run in multi-user mode.

---

_@BurntSushi requested changes on 2025-01-06 17:21_

---

_@guoci reviewed on 2025-01-06 18:13_

---

_Review comment by @guoci on `crates/uv-fs/src/lib.rs`:561 on 2025-01-06 18:13_

Thanks for the review.
> Is it possible to change the permissions after the file is created instead?

Yes, but multiple processes can cause errors between the time of creation and the time of changing permissions.
> I suppose another alternative here is to suggest that end users change their umask when running uv if they want it to run in multi-user mode.

Another alternative I have is to set the umask at `main`.

Yet another alternative to change the locking implementation to check the existence of a .lock file. Then, the .lock file must be deleted when unlocking.

If a uv process abnormally terminates without unlocking, subsequent runs may cause a deadlock. If that happens the Python environment is possibly broken and needs to be set up again anyway.


---

_@guoci reviewed on 2025-01-06 18:25_

---

_Review comment by @guoci on `crates/uv-fs/src/lib.rs`:567 on 2025-01-06 18:25_

> Can you use [`nix::sys::stat::umask`](https://docs.rs/nix/latest/nix/sys/stat/fn.umask.html#) here instead? (Otherwise, you're adding a dependency here on `nix` when just `libc` would do. But if we're depending on `nix`, we might as well use the safe API...)

Tested, `nix::sys::stat::umask` works and is safe.

---

_@BurntSushi reviewed on 2025-01-06 18:36_

---

_Review comment by @BurntSushi on `crates/uv-fs/src/lib.rs`:561 on 2025-01-06 18:36_

I think we need to be very cautious about changing our locking strategy. The strategy you're proposing is the "classic" approach (although for maximum portability, a directory and not a file should be used, as SQLite does IIRC), but it has the predictable downside as you point out: on some occasions, the lock file isn't deleted when unlocked. And the only way around this is user intervention, which is usually to delete the lock file and "hope for the best." Probably in this context, the simplest approach would be for the user to just completely delete the virtual environment.

IMO, the success of this strategy depends a lot on how often that lock file hangs around. In a perfect world, sure, its existence after process termination implies something went wrong with uv and thus the user should be blowing away the venv anyway. But I'm not convinced that this is the only way for a lock file to hang around. It's not uncommon for me to use programs that utilize this locking strategy, and despite their best efforts, I still need to remove the lock file from time to time.

Have you tried setting `umask` yourself outside of uv? Does that work? If it does, then I kinda feel like that's the most appropriate solution here. Because you're fundamentally working in a shared environment, and it seems sensible to set a `umask` that supports that environment.

---

_@guoci reviewed on 2025-01-06 18:46_

---

_Review comment by @guoci on `crates/uv-fs/src/lib.rs`:561 on 2025-01-06 18:46_

> I think we need to be very cautious about changing our locking strategy. The strategy you're proposing is the "classic" approach (although for maximum portability, a directory and not a file should be used, as SQLite does IIRC), but it has the predictable downside as you point out: on some occasions, the lock file isn't deleted when unlocked. And the only way around this is user intervention, which is usually to delete the lock file and "hope for the best." Probably in this context, the simplest approach would be for the user to just completely delete the virtual environment.
> 
> IMO, the success of this strategy depends a lot on how often that lock file hangs around. In a perfect world, sure, its existence after process termination implies something went wrong with uv and thus the user should be blowing away the venv anyway. But I'm not convinced that this is the only way for a lock file to hang around. It's not uncommon for me to use programs that utilize this locking strategy, and despite their best efforts, I still need to remove the lock file from time to time.

Thanks for the explanation.
> Have you tried setting `umask` yourself outside of uv? Does that work? If it does, then I kinda feel like that's the most appropriate solution here. Because you're fundamentally working in a shared environment, and it seems sensible to set a `umask` that supports that environment.

Yes, external setting of `umask` does work and also pointed out in https://github.com/astral-sh/uv/issues/8032#issue-2574635582.


---

_Review comment by @guoci on `crates/uv-fs/src/lib.rs`:561 on 2025-01-06 18:47_

I think this PR can be closed then.

---

_@guoci reviewed on 2025-01-06 18:47_

---

_@BurntSushi reviewed on 2025-01-06 18:50_

---

_Review comment by @BurntSushi on `crates/uv-fs/src/lib.rs`:561 on 2025-01-06 18:50_

All righty, thansk!

---

_Closed by @BurntSushi on 2025-01-06 18:50_

---
