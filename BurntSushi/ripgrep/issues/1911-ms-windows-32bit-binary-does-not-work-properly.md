```yaml
number: 1911
title: MS Windows 32bit binary does not work properly for files bigger than 4 gigabyte
type: issue
state: closed
author: datatraveller1
labels: []
assignees: []
created_at: 2021-06-26T08:58:19Z
updated_at: 2021-09-23T07:56:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1911
synced_at: 2026-01-12T16:13:24Z
```

# MS Windows 32bit binary does not work properly for files bigger than 4 gigabyte

---

_@datatraveller1_

#### What version of ripgrep are you using?

ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)

#### How did you install ripgrep?

I use the MS Windows binary rg.exe from ripgrep-13.0.0-i686-pc-windows-msvc.zip

#### What operating system are you using ripgrep on?

MS Windows 10 32bit

#### Describe your bug.

For files bigger than 4 gigabyte result lines are missing.
Only result lines from the upper part of the file or no lines are printed.

#### What are the steps to reproduce the behavior?

Search e.g. in a 5 gigabyte file for content of the last line.

#### What is the actual behavior?

rg "string_in_last_line_of_big_file" bigfile.txt

```
no output
```

#### What is the expected behavior?

The last line of bigfile.txt containing "string_in_last_line_of_big_file" should be printed.


---

_Comment by @BurntSushi on 2021-06-26 09:35_

Does `--no-mmap` resolve this?

---

_Comment by @datatraveller1 on 2021-06-26 14:09_

Yes, `rg --no-mmap "string_in_last_line_of_big_file" bigfile.txt` solves the issue with the 32bit version!
The missing result lines are printed with the **--no-mmap** option.

However, the previous 32bit version ripgrep-12.1.1-i686-pc-windows-msvc.zip works correctly _without_ the --no-map option.
Some interesting hints about this observation:
**ripgrep 12.1.1 (rev 7cb211378a)**
`rg "string_in_last_line_of_big_file" bigfile.txt --debug`
=>
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(string_in_last_line_of_big_file)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
**DEBUG|grep_searcher::searcher::mmap|crates\searcher\src\searcher\mmap.rs:86: bigfile.txt: failed to open memory map: memory map length overflows usize**

**ripgrep 13.0.0 (rev af6b6c543b)**
`rg "string_in_last_line_of_big_file" bigfile.txt --debug`
=>
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(string_in_last_line_of_big_file)], limit_size: 250, limit_class: 10 }
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH

I somehow assume `--no-mmap` should be the default for the 32bit version to avoid wrong results.

---

_Closed by @BurntSushi on 2021-06-26 16:55_

---

_Comment by @BurntSushi on 2021-06-26 16:56_

Yup. The next release will have mmap disabled in all non-64-bit systems.

I don't know why this used to work. The only semi-related change in ripgrep 13 that I can think of is that it is now statically linking vcruntime: https://github.com/BurntSushi/ripgrep/pull/1613

I'm not sure why or how that would change things here, but it's simple enough to just disable mmap on 32-bit.

---

_Comment by @datatraveller1 on 2021-06-27 10:58_

Thank you very much! I'll add a few more comments just to ensure there are no misunderstandings with the fix:
I plan to use the MS Windows 32bit binary release both for my PC (32bit) and my laptop (64bit) like it used to work with version 12.1.1.
Like on Windows 32bit, the current 13.0.0 32bit binary release does not work correctly on Windows 64bit unless the --no-mmap option is used. Only the current 13.0.0 64bit binary release works correctly on Windows 64bit for big files.
So, the patch (disable mmap) should impact the target rust 32bit binary release and not the Windows version currently used by the user (32bit or 64bit). I'm not a Rust programmer but it seems to me the fix takes care of that.


---

_Comment by @BurntSushi on 2021-06-27 11:11_

Thank you for copying your comment from email. :-) I'll copy my response as well:

The patch I pushed should impact the target used to build the binary.
A `cfg!` is conditional compilation.

I don't use Windows, so it's a bit annoying to test this patch myself. I have a Windows laptop somewhere in my house,
but it's always a big production to pull it out because Windows
insists on spending hours running Windows Update.

If anyone has an easy way to build ripgrep's master branch for 32-bit Windows and test it on a >4GB file with a match at the end, that would be most appreciated!

---

_Comment by @ghost on 2021-09-22 07:20_

I think the underlying reason is https://github.com/RazrFalcon/memmap2-rs/commit/5e271224c8411c89b42060294f9393cfc7b12a2a introduced into `memmap2` version 0.3.0 used here as of https://github.com/BurntSushi/ripgrep/blob/9b01a8f9ae53ebcd05c27ec21843758c2c1e823f/Cargo.lock#L343 and fixed by https://github.com/RazrFalcon/memmap2-rs/commit/9aa838aed99a4879d8357ff295a0ca1c98ba1ae5 which is part of version 0.3.1 (but version 0.5.0 is current).

---

_Comment by @ghost on 2021-09-22 07:22_

> If anyone has an easy way to build ripgrep's master branch for 32-bit Windows and test it on a >4GB file with a match at the end, that would be most appreciated!

I am not sure if that would have sufficed to catch this, but when working on the `memmap2` code, I have repeatedly resorted to cross building using MinGW and then testing the result under Wine. I am wary of the testing power of the `memmap2` to Win32 to POSIX API conversion though...

---

_Comment by @adamreichold on 2021-09-23 07:56_

> I am not sure if that would have sufficed to catch this, but when working on the memmap2 code, I have repeatedly resorted to cross building using MinGW and then testing the result under Wine. I am wary of the testing power of the memmap2 to Win32 to POSIX API conversion though...

I seems it would suffice, but there is a small wrinkle to this for cross building for 32-bit Windows: Due to LLVM/Rust assuming SEH-style unwinding but most Linux distributions 32-bit MinGW toolchains using SjLj-style unwinding, one will run into linker errors related to `_Unwind_Resume`, c.f. https://github.com/rust-lang/rust/issues/12859 Some distributions like Fedora since version 32 however ship cross toolchains using SEH-style unwinding and using such a distribution seems to work as expected, c.f. #2000 

---
