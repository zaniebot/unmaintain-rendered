```yaml
number: 2676
title: Search Failure Inside PowerShell Session
type: issue
state: closed
author: mattcargile
labels:
  - invalid
assignees: []
created_at: 2023-12-05T18:57:17Z
updated_at: 2024-01-08T16:01:02Z
url: https://github.com/BurntSushi/ripgrep/issues/2676
synced_at: 2026-01-12T16:13:24Z
```

# Search Failure Inside PowerShell Session

---

_@mattcargile_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

### How did you install ripgrep?

```powershell
choco install ripgrep
```

### What operating system are you using ripgrep on?

_Microsoft Windows Server 2019 Standard_

### Describe your bug.

_ripgrep_ won't return matches inside of a PowerShell WinRM ( e.g. WSMAN ) Session. It fails with error code 1.

### What are the steps to reproduce the behavior?

```powershell
Enter-PSSession remoteserver
new-item -itemtype directory rgtemp | cd
new-item t.txt -value 'hello world'
rg hello
# Check work with
ls *txt | select-string hello
```

### What is the actual behavior?

_ripgrep_ doesn't return any results.
```
[remoteserver]: PS C:\users\user\Documents\rgtemp\> rg --debug hello
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(hello)], limit_size: 250, limit_class: 10 }
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
```

### What is the expected behavior?

_ripgrep_ should return a match for `hello`. `rg` does work on the server when I use `ssh user@remoteserver`.

---

_Comment by @BurntSushi on 2023-12-05 19:25_

I can't easily attempt to reproduce this because I can't seem to get Powershell to install on my Linux system. (And even then, it's not clear it would have the same behavior as on a Windows machine.) I don't have an easily available Windows machine to test with.

Usually these types of things are environment or user errors. My first instinct would be to suggest that you have some ignore rules being applied here, but if your `--debug` output is complete, that  doesn't seem to be the case.

Please upgrade to ripgrep 14 and try again, but with `--trace` instead of `--debug`. Please post the _entire_ output here.

---

_Comment by @BurntSushi on 2024-01-06 19:08_

Closing due to inactivity and the likely fact that this is an environment or user error of some kind.

---

_Closed by @BurntSushi on 2024-01-06 19:08_

---

_Label `invalid` added by @BurntSushi on 2024-01-06 19:08_

---

_Comment by @mattcargile on 2024-01-08 15:36_

Here is the `rg --trace hello` output for what it is worth. It still won't find the text and doesn't output the expected `hello` text. Even doing `Enter-PSSession localhost` won't work for _PowerShell_ v7.

```powershell
DEBUG|rg::flags::parse|crates/core\flags\parse.rs:97: no extra arguments found from configuration file
DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1112: heuristic chose to search stdin
DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1260: found hostname for hyperlink configuration: RemoteServer
DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1270: hyperlink format: ""
DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:174: using 1 thread(s)
DEBUG|grep_regex::config|crates\regex\src\config.rs:175: assembling HIR from 1 fixed string literals
TRACE|grep_regex::matcher|crates\regex\src\matcher.rs:66: final regex: "(?:hello)"
TRACE|grep_regex::literal|crates\regex\src\literal.rs:75: skipping inner literal extraction, existing regex is believed to already be accelerated
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: xz: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: uncompress: could not find executable in PATH
TRACE|rg::search|crates/core\search.rs:254: <stdin>: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates\searcher\src\searcher\mod.rs:743: generic reader: searching via roll buffer strategy
TRACE|grep_searcher::searcher::core|crates\searcher\src\searcher\core.rs:65: searcher core: will use fast line searcher
```

---

_Comment by @BurntSushi on 2024-01-08 15:43_

The answer appears to be right there in the 2nd and 3rd log lines:

```
DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1112: heuristic chose to search stdin
```

I have no clue what a "PowerShell WinRM ( e.g. WSMAN ) Session" is, but whatever it is, it appears to be making it look like there is something on stdin. So ripgrep searches that.

Your chosen environment likely means you cannot rely on ripgrep's automatic detection of stdin. So you'll need to pass an a directory to search. So for example, `rg hello .` and not `rg hello`.

---

_Comment by @mattcargile on 2024-01-08 15:54_

No kidding! Yeah that did fix it. 

I'm surprised someone hasn't hit this before. _PowerShell WinRM_ is _Microsoft's_ iteration of an "ssh-like" remoting tech based on _SOAP_ and _HTTP_. It is the primary way to interface with remote _Windows_ machines from a _Windows_ client ( though that is changing ).

Not wanting to explore how the heuristic is working in this config, I suppose?

---

_Comment by @BurntSushi on 2024-01-08 16:01_

In theory if there's a way to adjust the heuristic such that it has no regressions, then I'd be okay with it. But, there is a very very very high barrier to me accepting such a change. For several reasons:

1. It is basically impossible for me to test. There are lots of different environments and I don't even know about all of them. And I don't have an easy way to test on Windows right now.
2. I have changed this heuristic in the past, and it breaks people. This leads to pain and frustration. In some cases, the break was intentional because there is literally no way to do it correctly in all cases.
3. From what I can tell, your _environment_ is what should be fixed. Not ripgrep. This might sound crazy, but it's actually pretty common in my experience for environments to senselessly try to attach something on stdin even though nothing is there. This almost certainly breaks things other than ripgrep. Even projects like neovim [alter or make available new functionality to fix such things](https://github.com/neovim/neovim/pull/14812).

If you're curious, this is where the implementation of the heuristic lives:

https://github.com/BurntSushi/ripgrep/blob/648a65f1976cc3b7eb66425024649d71d5befe1e/crates/cli/src/lib.rs#L152-L201

---
