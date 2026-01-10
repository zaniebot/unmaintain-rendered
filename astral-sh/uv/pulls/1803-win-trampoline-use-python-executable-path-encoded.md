```yaml
number: 1803
title: "Win Trampoline: Use Python executable path encoded in binary"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: win-trampoline-symlinks
created_at: 2024-02-21T11:21:02Z
updated_at: 2024-02-22T15:10:04Z
url: https://github.com/astral-sh/uv/pull/1803
synced_at: 2026-01-10T14:54:43Z
```

# Win Trampoline: Use Python executable path encoded in binary

---

_Pull request opened by @MichaReiser on 2024-02-21 11:21_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/uv/issues/1766

The first commit replaces the `check!` macro with a standalone function that can be reused for non-boolean returning functions to get nice error messages in case a syscall fails. 

The second commit changes the python discovery to work with symlinks too. It also changes a few existing calls to use safe APIs as a surprise for @BurntSushi ;)

The way this is implemented is that I changed the launcher file layout to:

```
| --------------------------------|
|           <launcher.exe>        |
| --------------------------------|
|       <zipped python scrip      |
| --------------------------------|
|           <python_path>         |
| --------------------------------|
|        len(<python_path>)       | 
| --------------------------------|
|              "UVUV"             |
| --------------------------------|
```

Windows ignores any content after the executable, which is a fact that the launcher already makes use of today. Python ignores everything after the zip end, which allows us to pack additional data after the zip file. I decided to use the magic number `"UVUV"` at the end of the file as a "safety" mechanism that the file indeed has the right format rather than just assuming that the last u32 (little endian) is the length of the UTF8 encoded python path. 

## Alternatives

### Resolving symlinks 

The downside of this approach is that we now need to read the executable which adds some complexity. An alternative to solve #1766 would be to extend our existing relative resolution to resolve symlinks before searching the python executable. 
I intended that this also addresses https://github.com/astral-sh/uv/issues/1779 where we use a global python installation instead, where simply following symlinks isn't sufficient anymore (assuming it is something we want to support). I've set up a [repro PR](https://github.com/MichaReiser/tox-uv/pull/1) that uses the new `uv` binary (the good news, is the error messages are much better). The job still fails with

```
Failed to spawn the python child process with command '"C:\hostedtoolcache\windows\Python\3.12.2\x64\Scripts\python.exe" "C:\hostedtoolcache\windows\Python\3.12.2\x64\Scripts\tox.exe" -vvvv --notest'
```

The problem is that our wheel installer assumes that `python` is in `.\Scripts\python.exe` but that's not the case for a regular Python installation where the binary is in the root folder. We would need to find the python installation when running `uv pip install` and pass the instance through to the wheel building code. This feels out of scope for this PR and probably requires input from someone more familiar with `uv` than I. What I don't understand is why this works for unix where we, presumably, have the same problem (or are we just lucky because the binary in global install happens to be in a `bin` directory?)


I'm open to changing the implementation to resolve symlinks instead, but this approach seemed more flexible. 


### Shebang parsing

The launcher used by `distutil` searches for the python script and then parses the shebang line to retrieve the Python executable name. This is kind of nice because it doesn't require a custom data format, it just works similarly to unix. 

The main downside that I'm seeing is that it requires a bit more parsing (and navigating) than the current approach. The launcher first needs to find the end of the zip file entry. From there, find the start of the script, and then parse the shebang. 

I decided against this approach (but open to changing) because it is more involved and our launcher isn't intended to be used without `uv` where the extra ergonomics of simply having to write a shebang brings us much benefit.


## Binary size increase

One of the main reasons for the binary increase is that the binary now contains more static strings with possible error messages. 

## Test Plan

I followed the instructions in #1766 and the command now runs successfully

```pws
uv venv
uv pip install pycowsay
New-Item -Path .\pycatsay.exe -ItemType SymbolicLink -Value .\.venv\Scripts\pycowsay.exe
.\pycatsay.exe


<  >

   \   ^__^
    \  (oo)\_______
       (__)\       )\/\
           ||----w |
           ||     ||


```



---

_Label `bug` added by @MichaReiser on 2024-02-21 11:29_

---

_Label `windows` added by @MichaReiser on 2024-02-21 11:29_

---

_@MichaReiser reviewed on 2024-02-21 11:33_

---

_Review comment by @MichaReiser on `crates/uv-trampoline/src/bounce.rs`:182 on 2024-02-21 11:33_

I tested the "incremental" reading by setting the capacity to 10

---

_Marked ready for review by @MichaReiser on 2024-02-21 15:30_

---

_Review requested from @konstin by @MichaReiser on 2024-02-21 15:30_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-02-21 15:30_

---

_Renamed from "Win Trampoline: Use Pyhton executable path encoded in binary" to "Win Trampoline: Use Python executable path encoded in binary" by @AlexWaygood on 2024-02-21 15:36_

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:69 on 2024-02-21 17:02_

Did you mean to leave this uncommented?

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:109 on 2024-02-21 17:03_

Nice. I like the reduction in `unsafe` scope. :-)

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:109 on 2024-02-21 17:04_

With `std`, we could probably use: https://doc.rust-lang.org/std/io/struct.Error.html#method.last_os_error

And get the error messages that come with that.

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:546 on 2024-02-21 17:14_

Hmmm. Since you're using [`FormatMessageA`](https://docs.rs/windows-sys/latest/windows_sys/Win32/System/Diagnostics/Debug/fn.FormatMessageA.html) and the type of `lpbuffer` is `PSTR = *mut u8`, then I think this comment should say `*mut 8` instead?

In any case, I think most of this strangeness is coming from using `FORMAT_MESSAGE_ALLOCATE_BUFFER`. I'd suggest just putting a buffer on the stack and having this function read into it. And also use `FormatMessageW`. The `A` routines are legacy AFAIK.

You might be able to just copy what std does: https://github.com/rust-lang/rust/blob/bf9229a2e366b4c311f059014a4aa08af16de5d8/library/std/src/sys/windows/os.rs#L26-L80

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:546 on 2024-02-21 17:15_

Ahhhh and I just realized you didn't write this code... OK. I'll understand if you want to just keep it the way it is now because it presumably works.

---

_Review comment by @BurntSushi on `crates/install-wheel-rs/src/wheel.rs`:286 on 2024-02-21 17:20_

You sure do know how to make me happy.

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:275 on 2024-02-21 17:40_

OK, so reading the above, I think it works like this:

* It first tries to read 1KB from the end of a file.
* If it finds the magic number, path length and path all within that 1KB, it stops.
* Otherwise, it resizes the buffer's capacity to whatever the decoded `path_len` is.
* It then goes back to the first step again, but read `{path_len}KB` instead.

I think there might be a couple issues with this approach, both of which center around trusting `path_len`:

1. If `path_len` is `u32::MAX`, then this will allocate 4GB.
2. If the file is being mutated while we're doing this in a very specific way, then it's possible this loop isn't guaranteed to terminate.

(2) is kind of a stretch, but (1) I think is a real enough issue.

My suggestion here would be to read 4KB from the end of the file, and if you can't find the path in there, consider it malformed and give up.

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:69 on 2024-02-21 17:40_

Ah this got commented again in the next commit. Whoops.

---

_@BurntSushi requested changes on 2024-02-21 17:42_

w00t! So much Windows API. Nice work.

I do think there is at least one thing worth changing here before merging, which is the logic for reading the file path from the end of the binary. The main issue I _think_ with the current implementation is that it trusts the path length, which could lead to the program allocating a huge amount of memory if something went wrong (whether intentional or not).

---

_@MichaReiser reviewed on 2024-02-21 18:03_

---

_Review comment by @MichaReiser on `crates/uv-trampoline/src/bounce.rs`:275 on 2024-02-21 18:03_

Thanks for the thorough review. I'll carefully go over the implementation again tomorrow. 

2. shouldn't be a problem because [we open the file](https://github.com/astral-sh/puffin/blob/e772c05b88019319f57cd52b084666291c2b262e/crates/uv-trampoline/src/bounce.rs#L147-L155) with [`FILE_SHARE_READ`](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-createfilea#parameters) only, prohibiting other processes from mutating or deleting the file while we're using it. 

Not trusting `path_len` makes sense as well does limiting. I'll probably go with a higher default, e.g. even URl have an [upper limit of 2MB](https://chromium.googlesource.com/chromium/src/+/master/docs/security/url_display_guidelines/url_display_guidelines.md#URL-Length). Agreed, URLs may contain application data which is different but I don't think it hurts to go somewhat higher than 4KB. 



---

_@BurntSushi reviewed on 2024-02-21 18:09_

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:275 on 2024-02-21 18:09_

Ah yeah good catch on (2). Thanks!

---

_@MichaReiser reviewed on 2024-02-21 21:33_

---

_Review comment by @MichaReiser on `crates/uv-trampoline/src/bounce.rs`:109 on 2024-02-21 21:33_

That would be nice. I would prefer to keep this PR with no_std. I can look into using std separately 

---

_Comment by @MichaReiser on 2024-02-21 21:34_

@charliermarsh / @zanieb If I want to add a test case as outlined in the test plan, how would I go about it? Can I install any python package with a binary script as part of the test or do we have a stub package that has a script entry point that I can use?

---

_Comment by @zanieb on 2024-02-21 21:38_

@MichaReiser you could generate a stub package and add it to the repository or use some "small" real world package with a pinned version

---

_Comment by @MichaReiser on 2024-02-21 21:42_

Do you have a link with some resources on how I would do that?

---

_@MichaReiser reviewed on 2024-02-22 08:59_

---

_Review comment by @MichaReiser on `scripts/wheels/simple_launcher-0.1.0-py3-none-any.whl`:1 on 2024-02-22 08:59_

Thanks @konstin for providing me with a test wheel!

---

_@konstin reviewed on 2024-02-22 09:21_

---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:275 on 2024-02-22 09:21_

From https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=registry:

> The Windows API has many functions that also have Unicode versions to permit an extended-length path for a maximum total path length of 32,767 characters.
> The maximum path of 32,767 characters is approximate, because the "\\?\" prefix may be expanded to a longer string by the system at run time, and this expansion applies to the total length.

---

_@konstin reviewed on 2024-02-22 09:28_

---

_Review comment by @konstin on `crates/uv/tests/pip_install.rs`:1491 on 2024-02-22 09:28_

```suggestion
fn entrypoint() -> Result<()> {
```

Or `launcher` maybe, i wouldn't call them scripts here since that therm is overloaded

---

_@BurntSushi reviewed on 2024-02-22 13:04_

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:109 on 2024-02-22 13:04_

Oh yeah absolutely, totally cool with this PR staying with `no_std`.

---

_@BurntSushi reviewed on 2024-02-22 13:06_

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:275 on 2024-02-22 13:06_

So my read of that doc is that there isn't a pre-defined upper bound on how long a path can be. Does that match your understanding? If that's true, I still think we need to place some kind of reasonable upper bound on what we're willing to accept. I think 4KB is probably sufficient, but 32KB would be fine with me too.

---

_@MichaReiser reviewed on 2024-02-22 13:20_

---

_Review comment by @MichaReiser on `crates/uv-trampoline/src/bounce.rs`:275 on 2024-02-22 13:20_

I limited it to 32KB

---

_Review requested from @BurntSushi by @MichaReiser on 2024-02-22 13:24_

---

_Review comment by @BurntSushi on `crates/uv-trampoline/.cargo/config.toml`:3 on 2024-02-22 13:32_

Did you mean to include this in this PR?

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:270 on 2024-02-22 13:42_

At first I was unsure if this was limiting heap memory since `bytes_to_read` was still being calculated based on `path_len`, but I see above that `path_len` is limited to a maximum. So I think that's right.

---

_@BurntSushi approved on 2024-02-22 13:43_

Nice, thank you!

---

_@MichaReiser reviewed on 2024-02-22 15:09_

---

_Review comment by @MichaReiser on `crates/uv-trampoline/.cargo/config.toml`:3 on 2024-02-22 15:09_

Yes that's intentional, considering that I won't land my `std` branch anytime soon. It requires me to only type `cargo build --release --target x86_64-pc-windows-msvc` instead of that plus the `-Z` compiler flags.

---

_Merged by @MichaReiser on 2024-02-22 15:10_

---

_Closed by @MichaReiser on 2024-02-22 15:10_

---

_Branch deleted on 2024-02-22 15:10_

---
