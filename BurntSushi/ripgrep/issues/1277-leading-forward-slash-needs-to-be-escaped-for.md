```yaml
number: 1277
title: leading forward slash needs to be escaped for unknown reason
type: issue
state: closed
author: jspashett
labels:
  - bug
  - wontfix
assignees: []
created_at: 2019-05-09T07:46:35Z
updated_at: 2020-09-12T09:41:03Z
url: https://github.com/BurntSushi/ripgrep/issues/1277
synced_at: 2026-01-12T16:13:23Z
```

# leading forward slash needs to be escaped for unknown reason

---

_@jspashett_

#### What version of ripgrep are you using?

C:\opt\bin>rg --version
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

Microsoft Windows [Version 10.0.17134.590]

#### Describe your question, feature request, or bug.

executing rg "/exchangecommshub"
against any files where a url component is /exchangecommshub does not match for reasons unknown. I assume ripgrep things its a path? This is in contrast to grep, and is surprising. 

The leading path separator must be "escaped":
rg "[/]exchangecommshub"

#### If this is a bug, what are the steps to reproduce the behavior?

Corpus line:
https://redacted.com/redacted/redacted/v1/004022555784/exchangecommshub
Rg:
rg "/exchangecommshub"

#### If this is a bug, what is the actual behavior?
```
your
output
goes
here
```

#### If this is a bug, what is the expected behavior?

It should have found all files and lines within which contain the literal "/exchangecommshub" in the same way that grep behaves, or it should say why it is/will surprisingly differ from the grep output


---

_Comment by @alexeyten on 2019-05-09 08:43_

In DOS/Windows forward slash always was a start of command parameter, e.g. `dir /w`

Not sure if it’s related to the question…

---

_Comment by @jspashett on 2019-05-09 08:56_

Yes, that's may be the reason. Although ripgrep on windows accepts "-" as option specifiers, too, so perhaps it should not accept / it's hard to say. It appears that all the powershell stuff now uses the dash format inplace of the slashes. 

The thing is too, that an argument of "/exchangecommshub" generates no error, and you get no results, so it's _not at all obvous_ that you get no results for that reason.

---

_Comment by @BurntSushi on 2019-05-09 10:46_

Thanks for the bug report! This does indeed sound strange. It "smells" like a shell issue on Windows, but there isn't enough data for this bug report to be actionable unfortunately. My _guess_ is that you're using cygwin, which will (I think) automatically do path translation on arguments. I've requested more details below:

Your link is broken. I get a 404: https://redacted.com/redacted/redacted/v1/004022555784/exchangecommshub

Please provide some input that can be searched that reproduces your problem.

Please also show the _actual_ output of the command that you ran.

Please also show the equivalent grep commands and its output.

Please also state which shell on Windows you're using and confirm that you're using the same shell for both ripgrep and grep.

---

_Label `question` added by @BurntSushi on 2019-05-09 10:46_

---

_Comment by @jspashett on 2019-05-09 11:50_

I think I have identified the problem. The problem exists when running rg in a msys2 shell. And it appears that becuase rg.exe is installed ourside of the msys installation, it converts paths by adding a prefix.  

First, The package was installed via cargo install ripgrep in a windows shell. (cargo install ripgrep)

**msys2 shell**
```
DESKTOP-2GTA575+user@DESKTOP-2GTA575 MINGW64 /c/temp
$ cat test.txt
anystringhere/findthis/example

DESKTOP-2GTA575+user@DESKTOP-2GTA575 MINGW64 /c/temp
$ rg "/example" test.txt
```
The process command line here was: 
`C:\opt\bin\rg.exe C:/msys64/example


**Windows Shell**
```
C:\temp>rg "/example" test.txt
1:anystringhere/findthis/example
```
The procces command line here was:
`rg  "/example"`

rg works correctly in the Windows subsystem shell, thereforce it appears to be the msys shell remapping mechanism that is causing the problem. 


---

_Comment by @BurntSushi on 2019-05-09 11:53_

Please show the output of `rg --trace "/example" test.txt` in both shells.

---

_Comment by @BurntSushi on 2019-05-09 11:55_

In particular, there should be a line in the trace output that contains `final regex: "..."` which shows the actual regex being used to search.

---

_Comment by @jspashett on 2019-05-09 13:03_

**Msys2 shell:**
"DESKTOP-2GTA575+user@DESKTOP-2GTA575 MINGW64 /c/temp
$ rg --trace "/example" test.txt
DEBUG|grep_regex::literal|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-regex-0.1.3\src\literal.rs:59: literal prefixes detected: Literals { lits: [Complete(C:/msys64/example)], limit_size: 250, limit_class: 10 }
TRACE|grep_regex::matcher|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-regex-0.1.3\src\matcher.rs:56: final regex: "C:/msys64/example"
DEBUG|globset|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\globset-0.4.3\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|grep_searcher::searcher|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-searcher-0.1.4\src\searcher\mod.rs:663: Some("test.txt"): searching via memory map
TRACE|grep_searcher::searcher|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-searcher-0.1.4\src\searcher\mod.rs:753: slice reader: searching via slice-by-line strategy
TRACE|grep_searcher::searcher::core|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-searcher-0.1.4\src\searcher\core.rs:61: searcher core: will use fast line searcher"

**Windows (CMD) shell**
"C:\temp>rg --trace "/example" test.txt
DEBUG|grep_regex::literal|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-regex-0.1.3\src\literal.rs:59: literal prefixes detected: Literals { lits: [Complete(/example)], limit_size: 250, limit_class: 10 }
TRACE|grep_regex::matcher|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-regex-0.1.3\src\matcher.rs:56: final regex: "/example"
DEBUG|globset|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\globset-0.4.3\src\lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|grep_searcher::searcher|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-searcher-0.1.4\src\searcher\mod.rs:663: Some("test.txt"): searching via memory map
TRACE|grep_searcher::searcher|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-searcher-0.1.4\src\searcher\mod.rs:753: slice reader: searching via slice-by-line strategy
TRACE|grep_searcher::searcher::core|C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-searcher-0.1.4\src\searcher\core.rs:61: searcher core: will use fast line searcher
1:anystringhere/findthis/example"

---

_Comment by @BurntSushi on 2019-05-09 13:20_

Yeah, you can see that when you run in the MSYS shell, that the _actual_ regex that ripgrep searches is `C:/msys64/example`, despite the fact that you typed in `/example`.

As far as I'm concerned, this is just crazy behavior from MSYS in assuming that any argument given to a command line program is a file path, and I don't see any reasonable way for ripgrep to fix this.

It is interesting to me that grep doesn't exhibit the same behavior, but your version of grep is likely bundled with MSYS in a way that makes it play nice with this automatic path translation behavior. I don't see an easy way for ripgrep to play along here.

Unfortunate, but this is a `wontfix`. As a workaround, I believe you can use `//example` as a way to "escape" the slash.

---

_Closed by @BurntSushi on 2019-05-09 13:20_

---

_Label `question` removed by @BurntSushi on 2019-05-09 13:20_

---

_Label `bug` added by @BurntSushi on 2019-05-09 13:20_

---

_Label `wontfix` added by @BurntSushi on 2019-05-09 13:20_

---

_Comment by @jspashett on 2019-05-09 14:02_

I agree. I can see why msys does this, so that it can correctly mapp it's view of the current directory for external programs. msys guessing that the parameter is a path from a leading slash is dubious, but perhaps there isn't another solution, if there is one I may propose a change to msys. I used [/] at the begining of the regex to get around this problem. 

---

_Comment by @Pyker on 2020-09-12 09:41_

I just ran across this issue, and just want to add Git for Windows's explanation and workarounds on this (https://github.com/git-for-windows/build-extra/blob/main/ReleaseNotes.md#known-issues):

> If you specify command-line options starting with a slash, POSIX-to-Windows path conversion will kick in converting e.g. "`/usr/bin/bash.exe`" to "`C:\Program Files\Git\usr\bin\bash.exe`". When that is not desired -- e.g. "`--upload-pack=/opt/git/bin/git-upload-pack`" or "`-L/regex/`" -- you need to set the environment variable `MSYS_NO_PATHCONV` temporarily, like so:
>
>  > `MSYS_NO_PATHCONV=1 git blame -L/pathconv/ msys2_path_conv.cc`
>
>  Alternatively, you can double the first slash to avoid POSIX-to-Windows path conversion, e.g. "`//usr/bin/bash.exe`".

---
