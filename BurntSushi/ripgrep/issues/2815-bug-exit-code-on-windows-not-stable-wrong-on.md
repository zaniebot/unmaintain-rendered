```yaml
number: 2815
title: "BUG: exit code on windows not stable (wrong on first run, then stable), seems to leak"
type: issue
state: closed
author: h-vetinari
labels:
  - invalid
assignees: []
created_at: 2024-05-23T14:49:31Z
updated_at: 2024-05-23T15:25:29Z
url: https://github.com/BurntSushi/ripgrep/issues/2815
synced_at: 2026-01-12T16:13:25Z
```

# BUG: exit code on windows not stable (wrong on first run, then stable), seems to leak

---

_@h-vetinari_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

```
>rg --version
ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)
```

### How did you install ripgrep?

Through [conda-forge](https://github.com/conda-forge/ripgrep-feedstock):
```
conda install -c conda-forge ripgrep
```

### What operating system are you using ripgrep on?

Windows

### Describe your bug.

I'm looking for a pattern where I'm expecting no matches, and want to use the exit code 1 (well-documented here acros several issues) as the indicator whether no matches were found.

However, I'm hitting something very strange - the exit code is inverted for the first call, and then fine for the second (and all later ones).
```
# file does not contain carriage returns (\r)
>type foo.ext | rg \r & echo %ERRORLEVEL%  # ❌ 
0
>type foo.ext | rg \r & echo %ERRORLEVEL%  # ✅ 
1
>type foo.ext | rg \r & echo %ERRORLEVEL%  # ✅ # and so on
1
```
Here I'm looking for a carriage return, but the same thing happens when file or the search pattern, in the sense that the previous exit code "leaks" into the first execution of the new query, and then stabilizes again.
```
# assuming file contains "found" & "also_found", but does not contain "not_found"
>type foo.ext | rg found & echo %ERRORLEVEL%      # ✅
0
>type foo.ext | rg found & echo %ERRORLEVEL%      # ✅ (etc.)
0
>type foo.ext | rg not_found & echo %ERRORLEVEL%  # ❌ LEAKS
0
>type foo.ext | rg not_found & echo %ERRORLEVEL%  # ✅ (etc.)
1
>type foo.ext | rg found & echo %ERRORLEVEL%      # ❌ LEAKS
1
>type foo.ext | rg found & echo %ERRORLEVEL%      # ✅ (etc.)
0
>type foo.ext | rg also_found & echo %ERRORLEVEL% # ✅ (unchanged exit code; appearance of no leak)
0
>type foo.ext | rg not_found & echo %ERRORLEVEL%  # ❌ LEAKS again
0
>type foo.ext | rg not_found & echo %ERRORLEVEL%  # ✅ (etc.)
1
```

### What are the steps to reproduce the behavior?

See above

### What is the actual behavior?

```
>type foo.ext | rg --debug found & echo %ERRORLEVEL%
rg: DEBUG|rg::flags::parse|crates/core\flags\parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1112: heuristic chose to search stdin
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1260: found hostname for hyperlink configuration: [xxx]
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:174: using 1 thread(s)
rg: DEBUG|grep_regex::config|crates\regex\src\config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: brotli: could not find executable in PATH
rg: DEBUG|globset|crates\globset\src\lib.rs:453: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
[...matching lines omitted...]
0

>type foo.ext | rg --debug not_found & echo %ERRORLEVEL%
rg: DEBUG|rg::flags::parse|crates/core\flags\parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1112: heuristic chose to search stdin
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1260: found hostname for hyperlink configuration: [xxx]
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:174: using 1 thread(s)
rg: DEBUG|grep_regex::config|crates\regex\src\config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: brotli: could not find executable in PATH
rg: DEBUG|globset|crates\globset\src\lib.rs:453: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
0

>type foo.ext | rg --debug not_found & echo %ERRORLEVEL%
rg: DEBUG|rg::flags::parse|crates/core\flags\parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1112: heuristic chose to search stdin
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1260: found hostname for hyperlink configuration: [xxx]
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:174: using 1 thread(s)
rg: DEBUG|grep_regex::config|crates\regex\src\config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: brotli: could not find executable in PATH
rg: DEBUG|globset|crates\globset\src\lib.rs:453: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
1
```

### What is the expected behavior?

Exit code should be correct already for first invocation; no leaks of exit codes between different invocations should occur.

---

_Comment by @BurntSushi on 2024-05-23 15:08_

Can you give a full reproduction? Like maybe it starts with `git clone ...` so that we search the same input. Your output here is confusing to follow because I'd expect some of the output to include matching lines. But you might have clipped it out. But that's relevant for other examples where an exit status of `0` is returned with no matching lines. So I'm overall just confused.

I'm also not that familiar with PowerShell (I assume that's what you're using), so this might be something you or someone else will need to debug. But the first step here is getting a complete reproduction.

---

_Comment by @h-vetinari on 2024-05-23 15:09_

Yes, I clipped the matching lines, because they're not relevant to the example and I didn't want to blow up the text much further.s

---

_Comment by @h-vetinari on 2024-05-23 15:10_

> I'm also not that familiar with PowerShell (I assume that's what you're using), so this might be something you or someone else will need to debug. But the first step here is getting a complete reproduction.

This is regular cmd.exe

---

_Comment by @ltrzesniewski on 2024-05-23 15:10_

This seems to be relevant: https://superuser.com/a/1618520/373907

---

_Comment by @BurntSushi on 2024-05-23 15:11_

> Yes, I clipped the matching lines, because they're not relevant to the example and I didn't want to blow up the text much further.s

Yes but it's confusing to me, _especially without a reproduction_, because I can't easily tell what's actually being emitted as output and what isn't. The best way is to not edit the output. And if the output is too big, then come up with a smaller input that gives a more reasonably sized output.

---

_Comment by @BurntSushi on 2024-05-23 15:12_

> This seems to be relevant: https://superuser.com/a/1618520/373907

Indeed. That looks like the issue here.

---

_Comment by @h-vetinari on 2024-05-23 15:13_

> Can you give a full reproduction?

With the following file:
```
found
also_found
```
as `foo.ext`, the results are
```
>type foo.ext | rg found & echo %ERRORLEVEL%
found
also_found
0

>type foo.ext | rg not_found & echo %ERRORLEVEL%
0

>type foo.ext | rg not_found & echo %ERRORLEVEL%
1

>type foo.ext | rg found & echo %ERRORLEVEL%
found
also_found
1

>type foo.ext | rg also_found & echo %ERRORLEVEL%
also_found
0

>type foo.ext | rg not_found & echo %ERRORLEVEL%
0

>type foo.ext | rg not_found & echo %ERRORLEVEL%
1
```

---

_Comment by @h-vetinari on 2024-05-23 15:15_

> This seems to be relevant: https://superuser.com/a/1618520/373907

Thanks for the link. I had a hunch about something like this (windows is a mess...). In this case, this is then not ripgreps fault, and I apologise for the noise...

And thanks a lot for the super-quick responses!

---

_Comment by @ltrzesniewski on 2024-05-23 15:20_

Yeah I have to agree, it's a mess 😅

Here's a workaround: https://ss64.com/nt/delayedexpansion.html

There's an explanation in [Raymon Chen's article](https://devblogs.microsoft.com/oldnewthing/20060823-00/?p=29993):

> Why is immediate expansion the default? Because prior to Windows NT, that was the only type of expansion supported by the command interpreter. Retaining immediate expansion as the default preserved backwards compatibility with existing batch files. (The original command interpreter was written in assembly language. You really didn’t want to be too clever or it would make your brain hurt trying to maintain the code. An interpreter loop of the form “Read a line, expand environment variables, evaluate” was therefore simple and effective.)

---

_Closed by @BurntSushi on 2024-05-23 15:25_

---

_Label `invalid` added by @BurntSushi on 2024-05-23 15:25_

---
