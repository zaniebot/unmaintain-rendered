```yaml
number: 2510
title: Encoding issue when piping rg output
type: issue
state: closed
author: cesaryuan
labels: []
assignees: []
created_at: 2023-05-09T14:08:17Z
updated_at: 2023-05-09T14:33:30Z
url: https://github.com/BurntSushi/ripgrep/issues/2510
synced_at: 2026-01-12T16:13:24Z
```

# Encoding issue when piping rg output

---

_@cesaryuan_

#### What version of ripgrep are you using?

ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Scoop

#### What operating system are you using ripgrep on?

Windows 10 19045.2846
PowerShell 7.3.4

#### Describe your bug.

There seems to be an encoding issue when piping the output of a ripgrep search. The chinese characters `测试` (and probably others, those are just the ones I've tested with) are converted to wrong characters(`娴嬭瘯`) when being piped to anything.

Passing stdout to a file with > is also with this problem.

This is what I used to test.

```
PS D:\0-Download> echo "测试Chinese Test" | rg -o "测试"
测试
PS D:\0-Download> echo "测试Chinese Test" | rg -o "测试" | echo
娴嬭瘯
PS D:\0-Download> echo "测试Chinese Test" | rg -o "测试" > test.txt
PS D:\0-Download> cat .\test.txt
娴嬭瘯
```

![image](https://github.com/BurntSushi/ripgrep/assets/35998162/eaa37a24-9800-4862-9f15-5189cab8900e)


#### What are the steps to reproduce the behavior?

```
PS D:\0-Download> echo "测试Chinese Test" | rg -o "测试"
PS D:\0-Download> echo "测试Chinese Test" | rg -o "测试" | echo
PS D:\0-Download> echo "测试Chinese Test" | rg -o "测试" > test.txt
PS D:\0-Download> cat .\test.txt
```

#### What is the actual behavior?

```
PS D:\0-Download> echo "测试Chinese Test" | rg -o --debug "测试" | echo
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(测试)], limit_size: 250, limit_class: 10 }
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: gzip: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: bzip2: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: brotli: could not find executable in PATH
DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:482: uncompress: could not find executable in PATH
DEBUG|globset|crates\globset\src\lib.rs:421: built glob set; 0 literals, 0 basenames, 5 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
娴嬭瘯
```

#### What is the expected behavior?

```
PS D:\0-Download> echo "测试Chinese Test" | rg -o "测试" | echo
测试
```


---

_Comment by @cesaryuan on 2023-05-09 14:25_

https://github.com/BurntSushi/ripgrep/blob/041544853c86dde91c49983e5ddd0aa799bd2831/FAQ.md?plain=1#L810-L814

Tried this. Now works!

---

_Closed by @cesaryuan on 2023-05-09 14:25_

---

_Comment by @BurntSushi on 2023-05-09 14:33_

Nice, glad you got it figured out and thank you for posting the solution!

---

_Locked by @ghost on 2023-05-09 14:33_

---
