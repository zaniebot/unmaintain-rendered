```yaml
number: 1523
title: Add CMD support
type: pull_request
state: merged
author: MichaReiser
labels:
  - windows
  - compatibility
assignees: []
merged: true
base: main
head: bat-activation-script
created_at: 2024-02-16T18:40:00Z
updated_at: 2024-02-18T11:04:51Z
url: https://github.com/astral-sh/uv/pull/1523
synced_at: 2026-01-12T16:04:38Z
```

# Add CMD support

---

_@MichaReiser_

## Sumamry

This PR adds the `activation.bat`, `deactivation.bat` and `pyenv.bat` files to add support for using uv from CMD. 

This PR further fixes an issue with our trampoline implementation where calling an executable like `black` failed:

```
(venv) C:\Users\Micha\astral\test>where black
C:\Users\Micha\astral\test\.venv\Scripts\black.exe

(venv) C:\Users\Micha\astral\test>black
C:\Users\Micha\AppData\Local\Programs\Python\Python312\python.exe: can't open file 'C:\\Users\\Micha\\astral\\test\\black': [Errno 2] No such file or directory
```

The issue was that CMD doesn't extend `black` to its full path before passing it to the trampoline and our trampoline generated the command `<python> black` instead of `<python> .venv/Scripts/black`, and Python can't find `black` in the project directory. 

This PR fixes this by using the full executable name (that we already parsed out to discover the Python version). This adds one complication, we need to preserve the arguments without repeating the executable name that is the first argument. 
One option is to use [`CommandLineToArgvW`](https://learn.microsoft.com/de-de/windows/win32/api/shellapi/nf-shellapi-commandlinetoargvw) and then serialize the arguments 1.. to a string again. I decided against that. Win32 API calls are easy to get wrong. That's why I implemented the parsing rules specified in [`CommandLineToArgvW`](https://learn.microsoft.com/de-de/windows/win32/api/shellapi/nf-shellapi-commandlinetoargvw) to skip the first argument.

Fixes https://github.com/astral-sh/uv/issues/1471

## Test Plan

https://github.com/astral-sh/uv/assets/1203881/bdb537b6-97c8-4f7e-bb4a-3a614eb5e0f6

Powershell continues to work

https://github.com/astral-sh/uv/assets/1203881/6c806477-a7c6-4047-9ffc-5ed91c6f1c84

I haven't been able to test the aarch binaries. 


---

_Converted to draft by @MichaReiser on 2024-02-16 18:40_

---

_Label `compatibility` added by @MichaReiser on 2024-02-16 18:41_

---

_Label `windows` added by @zanieb on 2024-02-16 18:42_

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:7 on 2024-02-16 19:17_

Out of curiosity, how come you moved this? I'd group it with `core` above since it's part of the standard library.

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:61 on 2024-02-16 19:31_

I don't think we need to use unchecked conversion here? The cost of UTF-8 validation on short strings is likely insignificant compared to starting a process.

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:61 on 2024-02-16 19:31_

(I think this was one of my main complaints with `uv-trampoline`. Very excessive use of `unsafe`.)

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:61 on 2024-02-16 19:33_

I'm also not even sure that these unchecked conversions are correct. The child path in particular is coming from a `&CStr`, and that is guaranteed to be valid UTF-8. Something higher level _might_ be guaranteeing it, but it is tricky for me to reason about in this context.

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:133 on 2024-02-16 19:34_

Bah. I assume we need to do this because `uv-trampoline` doesn't have `std`?

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:144 on 2024-02-16 19:37_

I'd drop the `unsafe` here too. (Although I do believe it is correct.) And replace it with [`CString::new(buffer).expect("no interior NUL bytes")`](https://doc.rust-lang.org/std/ffi/struct.CString.html#method.new).

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:144 on 2024-02-16 19:39_

I'd write a safety comment here. I think something like this:

```
// SAFETY: We rely on `GetCommandLineA()` to return a valid
// pointer to a NUL terminated string.
```

---

_@BurntSushi reviewed on 2024-02-16 19:41_

Nice! I left a few comments where I think `unsafe` can be removed, but some do appear necessary. With that said, this seems somewhat more brutal than we probably really need. If the trampoline could use `std`, then I don't think we'd need to re-roll arg parsing/quoting ourselves. But that's a bigger issue and we shouldn't block fixing this issue on it.

---

_@MichaReiser reviewed on 2024-02-17 17:33_

---

_Review comment by @MichaReiser on `crates/uv-trampoline/src/bounce.rs`:7 on 2024-02-17 17:33_

I hit organize imports in RustRover and I guess, that's what came out. I don't mind reverting

---

_@MichaReiser reviewed on 2024-02-17 17:33_

---

_Review comment by @MichaReiser on `crates/uv-trampoline/src/bounce.rs`:61 on 2024-02-17 17:33_

These are debug statements, they'll go away ;)

---

_@MichaReiser reviewed on 2024-02-17 17:34_

---

_Review comment by @MichaReiser on `crates/uv-trampoline/src/bounce.rs`:133 on 2024-02-17 17:34_

Probably, but I don't know. I only moved this line (I'm trying to keep my changes minimal)

---

_@MichaReiser reviewed on 2024-02-17 17:36_

---

_Review comment by @MichaReiser on `crates/uv-trampoline/src/bounce.rs`:144 on 2024-02-17 17:36_

The null terminator on line 138 should guarantee the null termination ;) 

I tried to keep the implementation consistent with the existing code that exclusively use the unchecked variation because of what's mentioned here

https://github.com/astral-sh/uv/blob/2586f655bbf650a9797c8f88b6d9066eefe7a3dc/crates/uv-trampoline/README.md#L75-L86

I don't mind reconsidering the decision to prefer checked versions over unchecked ones but we might want to do this separately because we should look at the generated artifact size etc.

For example, I changed the `unchecked` call in `find_python_exe` to ` CString::from_vec_with_nul(buffer).expect("String to be correctly null terminated")` and it increases the binary size from  16896 to 23040. From what I understand, the size of the executables is a primary concern of this library. 

The size is slightly smaller for `CString::new(buffer).expect("String to be correctly null terminated")` but it still increases to 22528. That's why I think this must be a more holistic decision where we decide to favor safety over binary size (it's still tiny)

---

_@BurntSushi reviewed on 2024-02-17 18:12_

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:144 on 2024-02-17 18:12_

> I don't mind reconsidering the decision to prefer checked versions over unchecked ones but we might want to do this separately because we should look at the generated artifact size etc.

I reluctantly agree that this is probably the wise course of action for now.

Yeah, binary size is a primary concern for this particular library but I would like to eventually litigate whether it's a primary concern for us.

---

_@MichaReiser reviewed on 2024-02-17 18:51_

---

_Review comment by @MichaReiser on `crates/uv-trampoline/src/bounce.rs`:159 on 2024-02-17 18:51_

I tried to write tests for this function but failed to get this crate to compile with `cargo test`.... I ended up coping the implementation into another crate and do some smoke testing (and I found and fixed a bug in the implementation)

---

_Marked ready for review by @MichaReiser on 2024-02-17 19:15_

---

_Merged by @charliermarsh on 2024-02-17 21:47_

---

_Closed by @charliermarsh on 2024-02-17 21:47_

---

_Branch deleted on 2024-02-17 21:47_

---

_Comment by @charliermarsh on 2024-02-17 21:48_

(Gonna merge this based on comments + responses so it can be part of v0.1.4.)

---

_@AlexWaygood reviewed on 2024-02-17 23:11_

---

_Review comment by @AlexWaygood on `crates/gourgeist/src/activator/activate.bat`:24 on 2024-02-17 23:11_

I think this might mean that the prompt in a cmd shell always has `(venv)` prepended to it when the virtual environment is activated, even when the virtual environment is in a directory with a different name:

![image](https://github.com/astral-sh/uv/assets/66076021/c875bdc2-16ca-44bb-848c-2c69922e62dd)

It _looks_ like the CPython `activate.bat` script has some more complex logic around this (but I know nothing about `.bat` files)

---

_@zanieb reviewed on 2024-02-18 07:11_

---

_Review comment by @zanieb on `crates/gourgeist/src/activator/activate.bat`:24 on 2024-02-18 07:11_

Perhaps #1570 addresses this?

---

_@AlexWaygood reviewed on 2024-02-18 11:04_

---

_Review comment by @AlexWaygood on `crates/gourgeist/src/activator/activate.bat`:24 on 2024-02-18 11:04_

Right, thanks!

---
