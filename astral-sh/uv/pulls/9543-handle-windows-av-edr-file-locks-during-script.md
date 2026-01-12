```yaml
number: 9543
title: Handle Windows AV/EDR file locks during script installations
type: pull_request
state: merged
author: Coruscant11
labels:
  - windows
assignees: []
merged: true
base: main
head: install-script-retries-on-windows
created_at: 2024-11-30T15:40:26Z
updated_at: 2024-12-01T23:02:42Z
url: https://github.com/astral-sh/uv/pull/9543
synced_at: 2026-01-12T16:08:51Z
```

# Handle Windows AV/EDR file locks during script installations

---

_@Coruscant11_

Fixes #9531


## Context

While working with [uv](https://github.com/astral-sh/uv), I encountered issues with a python dependency, [httpx](https://www.python-httpx.org/) unable to be installed because of a **os error 5 permission denied**.
The error occur when we try to persist a **.exe file** from a temporary folder into a persistent one.
I only reproduce the issue in an enterprise **Windows** Jenkins Runner. In my virtual machines, I don't have any issues. So I think this is most probably coming from the system configuration. This windows runner **contains an AV/EDR**. And the fact that the file locked occured only once for an executable make me think that it's most probably the cause. 

While doing some research and speaking with some colleagues (hi @vmeurisse), it seems that the issue is a very recurrent one on Windows.
In the Javascript ecosystem, there is this package, created by the @isaacs, `npm` inventor: https://www.npmjs.com/package/graceful-fs, used inside `npm`, allowing its package installations to be more resilient to filesystem errors:
> The improvements are meant to normalize behavior across different platforms and environments, and to make filesystem access more resilient to errors.
One of its core feature is this one:
> On Windows, it retries renaming a file for up to one second if EACCESS or EPERM error occurs, likely because antivirus software has locked the directory.

So I tried to implement the same algorithm on `uv`, **and it fixed my issue**! I can finally install `httpx`.

Then, [as I mentionned in this issue](https://github.com/astral-sh/uv/issues/9531#issuecomment-2508981316), I saw that you already implemented exactly the same algorithm in an asynchronous function for renames 😄 
https://github.com/astral-sh/uv/blob/22fd9f7ff17adfbec6880c6d92c162e3acb6a41c/crates/uv-fs/src/lib.rs#L221

## Summary of changes 
- I added a similar function for `persist` (was not easy to have the benediction of the borrow checker 😄)
- I added a `sync` variant of `rename_with_retry`
- I edited `install_script` to use the function including retries on Windows

Let me know if I should change anything 🙂 

Thanks!!

## Test Plan
This pull-request should be totally iso-functional, so I think it should be covered by existing tests in case of regression.
All tests are still passing on my side.
Also, of course validated that my windows machines (windows 10 & windows 11) containing AV/EDR software are now able to install `httpx.exe` script.
However, if you think any additional test is needed, feel free to tell me!


---

_Renamed from "Handle Windows AV/EDR file locks during script installations" to "[DRAFT] Handle Windows AV/EDR file locks during script installations" by @Coruscant11 on 2024-11-30 15:49_

---

_Converted to draft by @Coruscant11 on 2024-11-30 15:49_

---

_Renamed from "[DRAFT] Handle Windows AV/EDR file locks during script installations" to "Handle Windows AV/EDR file locks during script installations" by @Coruscant11 on 2024-11-30 15:50_

---

_Comment by @Coruscant11 on 2024-11-30 16:32_

(Currently developping on a Windows virtual machine, have to admit I lost a lot of productivity 😆That's why I will stick to a draft for now and abuse of the GitHub CI, not enough cpu on my side unfortunately)

---

_Comment by @Coruscant11 on 2024-11-30 16:38_

Hi @charliermarsh, I wanted to reuse an existing function, but which is already async, instead of writing a duplicate sync one. Is it a good idea? The install wheel crate is synchronous.
I am currently facing some few tests not passing because of conflicted runtimes. Unfortunately I lack knowledge in async rust...

The goal is to re-use `rename_with_retry`.
Explained here: https://github.com/astral-sh/uv/issues/9531#issuecomment-2508981316
I can also implement a sync version,

Am I currently on the good way by trying to run it with a tokio runtime?
Or do you think we should convert the `uv-install-wheel` to async before?

Thanks!

---

_Marked ready for review by @Coruscant11 on 2024-11-30 19:55_

---

_Comment by @Coruscant11 on 2024-11-30 19:57_

So for now, I made a sync version of `rename_with_retry`.
My first goal is to let you validate the functional behaviour solving the issue, I'm open to any comments🙂
Confirmed it solved the issue under one Windows 10 and one Windows 11 machines both under the coverage of EDR security softwares.

---

_@charliermarsh reviewed on 2024-12-01 02:29_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:428 on 2024-12-01 02:29_

My only concern is that this is now using rename rather than `persist`. And on Windows, it looks like persist is a bit different, in `tempfile`?

```rust
pub fn persist(old_path: &Path, new_path: &Path, overwrite: bool) -> io::Result<()> {
    unsafe {
        let old_path_w = to_utf16(old_path);
        let new_path_w = to_utf16(new_path);

        // Don't succeed if this fails. We don't want to claim to have successfully persisted a file
        // still marked as temporary because this file won't have the same consistency guarantees.
        if SetFileAttributesW(old_path_w.as_ptr(), FILE_ATTRIBUTE_NORMAL) == 0 {
            return Err(io::Error::last_os_error());
        }

        let mut flags = 0;

        if overwrite {
            flags |= MOVEFILE_REPLACE_EXISTING;
        }

        if MoveFileExW(old_path_w.as_ptr(), new_path_w.as_ptr(), flags) == 0 {
            let e = io::Error::last_os_error();
            // If this fails, the temporary file is now un-hidden and no longer marked temporary
            // (slightly less efficient) but it will still work.
            let _ = SetFileAttributesW(old_path_w.as_ptr(), FILE_ATTRIBUTE_TEMPORARY);
            Err(e)
        } else {
            Ok(())
        }
    }
}
```

---

_@Coruscant11 reviewed on 2024-12-01 12:28_

---

_Review comment by @Coruscant11 on `crates/uv-install-wheel/src/wheel.rs`:428 on 2024-12-01 12:28_

You're right! I will try to see if I can still use the `persist` function.
Just currently fighting with the borrow-checker, as `persist` takes `self` and re-returns it in case of `PersistError`,

```rust
error[E0507]: cannot move out of `*from`, as `from` is a captured variable in an `FnMut` closure
   --> crates\uv-fs\src\lib.rs:277:42
    |
262 |     from: &NamedTempFile,
    |     ---- captured outer variable
...
277 |         backoff::retry(backoff, || match from.persist(to) {
    |                                 --       ^^^^ ----------- `*from` moved due to this method call
    |                                 |        |
    |                                 |        move occurs because `*from` has type `NamedTempFile`, which does not implement the `Copy` trait
    |                                 captured by this `FnMut` closure
    |
note: `NamedTempFile::<F>::persist` takes ownership of the receiver `self`, which moves `*from`
   --> C:\Users\Jp\.cargo\registry\src\index.crates.io-6f17d22bba15001f\tempfile-3.14.0\src\file\mod.rs:722:36
    |
722 |     pub fn persist<P: AsRef<Path>>(self, new_path: P) -> Result<F, PersistError<F>> {
```

It's a good rust exercise 😄 I will ping you when I solve it

---

_@Coruscant11 reviewed on 2024-12-01 17:59_

---

_Review comment by @Coruscant11 on `crates/uv-install-wheel/src/wheel.rs`:428 on 2024-12-01 17:59_

@charliermarsh Updated! Looking forward for your opinion, the signature and the behavior of `persist` was not easy at the beggining to put inside a `backoff::retry` closure,
```rust
pub fn persist<P: AsRef<Path>>(self, new_path: P) -> Result<F, PersistError<F>>
```

> Persist the temporary file at the target path.
>
>If a file exists at the target path, persist will atomically replace it. If this method fails, it will return self in the resulting [PersistError](https://docs.rs/tempfile/latest/tempfile/struct.PersistError.html).

I got bullied a lot by the borrow checker, but I finally managed to do it 😄 

So I found a way to get back the reference returned by the error for every retry.

Looking forward for your opinion on this!

---

_@charliermarsh approved on 2024-12-01 22:56_

This looks great, thank you for persisting :)

---

_Label `windows` added by @charliermarsh on 2024-12-01 22:57_

---

_Merged by @charliermarsh on 2024-12-01 22:57_

---

_Closed by @charliermarsh on 2024-12-01 22:57_

---

_Comment by @Coruscant11 on 2024-12-01 23:01_

> This looks great, thank you for persisting :)

Thanks for the fast review and for making this such a welcoming project for contributors! 🙌

---
