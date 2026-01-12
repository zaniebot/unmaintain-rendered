```yaml
number: 3131
title: rg -c omits files when a NUL byte is encountered after an earlier match
type: issue
state: closed
author: ejh3
labels:
  - wontfix
  - doc
assignees: []
created_at: 2025-08-21T22:14:21Z
updated_at: 2025-09-23T00:24:54Z
url: https://github.com/BurntSushi/ripgrep/issues/3131
synced_at: 2026-01-12T16:13:25Z
```

# rg -c omits files when a NUL byte is encountered after an earlier match

---

_@ejh3_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?
```
ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,+SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)
```
### How did you install ripgrep?

```
> rpm -qf $(which rg) 2>/dev/null
ripgrep-14.1.1-1.el9.x86_64
```

### What operating system are you using ripgrep on?

```
> uname -a
Linux PLACEHOLDER 5.10.228-41577284.AroraKernel510.el7.x86_64 #1 SMP Tue Apr 15 16:42:53 PDT 2025 x86_64 x86_64 x86_64 GNU/Linux
```

### Describe your bug.

When a file contains a match before a NUL byte but also contains a NUL later, `rg -l` correctly lists the file, but `rg -c` omits it entirely. This is inconsistent: `-c` should report the same set of files as `-l` or ripgrep with no options, even if binary-file detection stops the scan early. Using `-ca` (treat as text) gives the expected results.

### What are the steps to reproduce the behavior?

```
ejhunter /tmp/rgBug > { echo "cat here"; yes "padding line" | head -n 150000; printf '\0'; } > file1.txt
ejhunter /tmp/rgBug > echo "cat here" > file2.txt

ejhunter /tmp/rgBug > rg 'cat' -l --no-config
file2.txt
file1.txt

ejhunter /tmp/rgBug > rg 'cat' -c --no-config
file2.txt:1

ejhunter /tmp/rgBug > rg 'cat' --no-config
file2.txt
1:cat here

file1.txt
1:cat here
file1.txt: WARNING: stopped searching binary file after match (found "\0" byte around offset 1950009)

ejhunter /tmp/rgBug > rg 'cat' -ca --no-config
file2.txt:1
file1.txt:1
```

### What is the actual behavior?

```
ejhunter /tmp/rgBug > rg 'cat' -c --no-config --debug
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:89: not reading config files because --no-config is present
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Count))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1118: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: REDACTED
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/usr/share/cargo/registry/grep-regex-0.1.13/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/usr/share/cargo/registry/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
file2.txt:1
rg: DEBUG|grep_printer::summary|/usr/share/cargo/registry/grep-printer-0.2.2/src/summary.rs:698: ignoring file1.txt: found binary data at offset 1950009
ejhunter /tmp/rgBug > rg 'cat' -l --no-config --debug
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:89: not reading config files because --no-config is present
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(FilesWithMatches))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1118: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: REDACTED
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 12 thread(s)
rg: DEBUG|grep_regex::config|/usr/share/cargo/registry/grep-regex-0.1.13/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|/usr/share/cargo/registry/globset-0.4.15/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
file2.txt
file1.txt
```

### What is the expected behavior?

The `--count` option should behave the same as having no options wrt how many matches it finds. If there's only 1 match found before early termination, `rg -c` should return that. Whether it spit out a warning or not, I don't have a strong opinion on.

---

_Comment by @shubhamDP-arch on 2025-09-22 19:45_

additionally, there's a related inconsistency where rg 'cat' -c and rg 'cat' -c file1.txt file2.txt produce different results 

the procedure of reproducing the behavior is same as mentioned above by the issue writer , with these additional commands

**Command:**
```bash
rg 'cat' -c .
```

**Output:**
```
file2.txt:1
```

**Command:**
```bash
rg 'cat' -c file1.txt file2.txt
```

**Output:**
```
file2.txt:1
file1.txt:1
```

---

_Comment by @BurntSushi on 2025-09-22 21:05_

This is unfortunately a wontfix bug. Specifically:

https://github.com/BurntSushi/ripgrep/blob/f05549d7be238c622c516bc574dd7aa174a87f3a/crates/printer/src/summary.rs#L756-L774

Basically what's happening is that ripgrep is trying to preserve the intended semantics of binary filtering. When you use `-l/--files-with-matches`, ripgrep can _immediately_ stop searching once a match is found. Thus, ripgrep never even reads the binary data. But with `-c/--count`, ripgrep has to search the entire thing, which means it will see the binary data.

> additionally, there's a related inconsistency where rg 'cat' -c and rg 'cat' -c file1.txt file2.txt produce different results
> 
> the procedure of reproducing the behavior is same as mentioned above by the issue writer , with these additional commands
> 
> **Command:**
> 
> rg 'cat' -c .
> 
> **Output:**
> 
> ```
> file2.txt:1
> ```
> 
> **Command:**
> 
> rg 'cat' -c file1.txt file2.txt
> 
> **Output:**
> 
> ```
> file2.txt:1
> file1.txt:1
> ```

This is also expected since whenever you pass files explicitly to ripgrep, all filtering, including binary filtering, is disabled. So in this case, ripgrep behaves _as if_ you had passed `--binary`.

---

_Label `wontfix` added by @BurntSushi on 2025-09-22 21:06_

---

_Label `doc` added by @BurntSushi on 2025-09-22 21:06_

---

_Comment by @BurntSushi on 2025-09-22 21:06_

#3157 documents this potentially surprising behavior.

---

_Comment by @ejh3 on 2025-09-22 23:09_

I can understand marking this as wontfix. From a UX POV, perhaps showing no matches at all for a file is more of a "fail fast" approach than showing an incorrect number of matches, so users might realize they need `-a` sooner. 

That said, I want try framing this another way and see if it sticks. The inconsistency is not only between `-l` and `-c`, it's also between `-c` and no options: when searching normally rg will output all the matches it found up until it detects binary but with `-c` the previously displayed match(es) not show up in the count. Seems then that `-c` is the odd one out with both `-l` and normal search acting as if the file were truncated past the first binary detection where `-c` acts as if the file doesn't exist at all. In my mind, `-c` is roughly like piping the output of a normal search through `wc -l` on a per-file basis, and preserving the current behavior breaks that model (maybe this warrants some tweaks to #3157 at least).

Whatever you decide, thanks for taking a look at this issue and for your work on this tool generally

---

_Comment by @BurntSushi on 2025-09-22 23:49_

I appreciate that, but the inconsistency there isn't really avoidable either. Namely, the binary data occurs late enough in the file that a match has already been printed. ripgrep can't just back up and avoid it. The `-c` flag is a "summary" output, which means nothing gets printed until the entire file has been searched. And yes indeed, this is a fail fast approach. That is, I think it's better to not emit anything than to emit what could be an incorrect count.

Notably, grep behaves the same as ripgrep here (ripgrep's behavior is like GNU grep's `-I` flag):

```
$ grep -r -c -I 'cat' ./
./file1.txt:0
./file2.txt:1
$ grep -r -l -I 'cat' ./
./file1.txt
./file2.txt
$ grep -r -I 'cat' ./
./file1.txt:cat here
./file2.txt:cat here
```

And grep's _default_ behavior is identical to `rg --binary`:

```
$ grep -r -c 'cat' ./
./file1.txt:1
./file2.txt:1
$ grep -r -l 'cat' ./
./file1.txt
./file2.txt
$ grep -r 'cat' ./
./file1.txt:cat here
./file2.txt:cat here
```

---

_Closed by @BurntSushi on 2025-09-23 00:24_

---
