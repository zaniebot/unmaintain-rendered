```yaml
number: 1132
title: "Ripgrep fails with 'PCRE2: error matching: JIT stack limit reached'"
type: issue
state: closed
author: vineelkovvuri
labels: []
assignees: []
created_at: 2018-12-06T19:26:01Z
updated_at: 2018-12-06T20:42:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1132
synced_at: 2026-01-12T16:13:23Z
```

# Ripgrep fails with 'PCRE2: error matching: JIT stack limit reached'

---

_@vineelkovvuri_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

I am using Github binaries and this bug is not related to file permissions

#### What operating system are you using ripgrep on?

Microsoft Windows [Version 10.0.17134.468]

#### Describe your question, feature request, or bug.

This is a bug.

#### If this is a bug, what are the steps to reproduce the behavior?

I am trying to find strings like (new Object(...)) in my source files. As mentioned here https://stackoverflow.com/q/53656081/2407966 and based on the comments, I finalized on following regex pattern ``new(?:(?s).(?<!new))*?ccon``. When I run the command for small files([small.txt](https://github.com/BurntSushi/ripgrep/files/2654475/small.txt)) it works but for other files([aa.txt](https://github.com/BurntSushi/ripgrep/files/2654458/aa.txt))(hardly 120 lines) the command fails with ``PCRE2: error matching: JIT stack limit reached`` 

``rg -P --regex-size-limit 1G --dfa-size-limit 1G -U "new(?:(?s).(?<!new))*?ccon" -g aa.txt --debug --trace``



#### If this is a bug, what is the actual behavior?

```
D:\Temp\rip>rg -P --regex-size-limit 1G --dfa-size-limit 1G -U "new(?:(?s).(?<!new))*?ccon" -g aa.txt --debug --trace
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore\src\walk.rs:1614: whitelisting ./aa.txt: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "aa.txt", actual: "**/aa.txt", is_whitelist: false, is_only_dir: false })))))
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|grep_searcher::searcher|grep-searcher\src\searcher\mod.rs:629: Some("aa.txt"): reading entire file on to heap for mulitline
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|grep_searcher::searcher|grep-searcher\src\searcher\mod.rs:631: Some("aa.txt"): searching via multiline strategy
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
aa.txt: PCRE2: error matching: JIT stack limit reached
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```
[aa.txt](https://github.com/BurntSushi/ripgrep/files/2654458/aa.txt)

#### If this is a bug, what is the expected behavior?

If the string is not found the command should output nothing else print the matching strings. Same regex via perl ``perl -n000le "print $& while /new(?:(?s).(?<!new))*?OSF_CCONNECTION/mg" aa.txt`` works. Sorry I had to obfuscate aa.txt(it still repros the issue) 



---

_Comment by @BurntSushi on 2018-12-06 20:42_

This doesn't look like a bug to me. ripgrep is giving you an error from PCRE2. It's a limitation of using a backtracking engine in your search. You should find another way to run your search.

---

_Closed by @BurntSushi on 2018-12-06 20:42_

---
