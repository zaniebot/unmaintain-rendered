```yaml
number: 2838
title: rg.exe does not complete in bash.exe(WSL2) - Incorrectly assumes stdin
type: issue
state: closed
author: sceneq
labels: []
assignees: []
created_at: 2024-06-19T00:46:00Z
updated_at: 2024-06-19T11:58:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2838
synced_at: 2026-01-12T16:13:25Z
```

# rg.exe does not complete in bash.exe(WSL2) - Incorrectly assumes stdin

---

_@sceneq_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0 (rev c9ebcbd8ab)

### How did you install ripgrep?

winget, and cargo.exe

### What operating system are you using ripgrep on?

WSL2 Ubuntu 22.04.4 LTS

### Describe your bug.

The reason for using `rg.exe` on bash.exe: https://github.com/microsoft/WSL/issues/4197


I ran `rg.exe aa -l > list.txt` in bash.exe, but the command never completed.
It stopped at this line `rdr.read(...) `
The type of `rdr` was `encoding_rs_io::DecodeReaderBytes<&mut &mut std::io::stdio::StdinLock, &mut alloc::vec::Vec<u8>>`.
https://github.com/BurntSushi/ripgrep/blob/c9ebcbd8abe48c8336fb4826df7e9b6fb179de03/crates/searcher/src/line_buffer.rs#L418

The search target was mistakenly determined to be Stdin because `grep::cli::is_readable_stdin` returned `true`. process is as follows.

- false if `std::io::stdin().is_terminal()`
- true if `is_disk` (Windows)
- true if `is_pipe` (Windows)

I wrote the following program and ran it with bash.exe and cmd.exe.
```rust
fn main() {
    let is_terminal = std::io::stdin().is_terminal();

    let stdin = winapi_util::HandleRef::stdin();
    let typ = winapi_util::file::typ(stdin).unwrap();
    let is_disk = typ.is_disk();
    let is_pipe = typ.is_pipe();
    eprintln!("is_terminal={is_terminal}, is_disk={is_disk}, is_pipe={is_pipe}");
}
```

Result:
bash.exe
```
> ./target/debug/stdintest.exe
is_terminal=true , is_disk=false, is_pipe=false

> ./target/debug/stdintest.exe > f
is_terminal=false, is_disk=false, is_pipe=true

> echo | ./target/debug/stdintest.exe
is_terminal=false, is_disk=false, is_pipe=true
```

cmd.exe
```
> .\target\debug\stdintest.exe
is_terminal=true , is_disk=false, is_pipe=false

> .\target\debug\stdintest.exe > f
is_terminal=true , is_disk=false, is_pipe=false

> echo | .\target\debug\stdintest.exe
is_terminal=false, is_disk=false, is_pipe=true
```

It seems that `is_readable_stdin` is also true because the redirect on bash.exe was determined to be `is_pipe`.
The implementation of `is_pipe` is simple, but I don't know why it is so.

The Rust `is_terminal` implementation is this
https://github.com/rust-lang/rust/blob/737e42308c6e957575692965d73b17937f936f28/library/std/src/sys/pal/windows/io.rs#L83

I've looked into it, but I can't identify whether the problem is with winapi-util, Rust std, WSL2, or some other cause. Apologies, but I need ripgrep, so I'm submitting this issue here.

Note: `rg.exe aa . -l > f`, this works fine.

### What are the steps to reproduce the behavior?

`rg.exe aaa -l > f` on bash.exe

### What is the actual behavior?

`rg aaaaa --debug -l > a`
```
rg: DEBUG|rg::flags::parse|crates/core\flags\parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(FilesWithMatches))
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1112: heuristic chose to search stdin
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1260: found hostname for hyperlink configuration: PC030
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:174: using 1 thread(s)
rg: DEBUG|grep_regex::config|crates\regex\src\config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: gzip: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: gzip: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: bzip2: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: bzip2: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: xz: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: xz: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: lz4: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: xz: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: brotli: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: zstd: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: zstd: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: uncompress: could not find executable in PATH
```
rg.exe stops here.

### What is the expected behavior?

rg.exe should scan under the current directory rather than waiting for stdin input, even without a path specification.

---

_Comment by @BurntSushi on 2024-06-19 00:54_

ripgrep fundamentally has to rely on a heuristic to determine whether to search stdin or to search the current working directory. Something about your environment is advertising a readable stdin. _That_ is what needs to be fixed. The only alternative available to you is to disable ripgrep's heuristic by always providing a path to search.

---

_Closed by @BurntSushi on 2024-06-19 00:54_

---

_Comment by @ltrzesniewski on 2024-06-19 11:58_

Are you using a Windows build of ripgrep from WSL2? You should be using a Linux build, which will integrate much better with the rest of the environment.

---
