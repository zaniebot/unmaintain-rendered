```yaml
number: 1838
title: Byte pattern detection works inconsistently
type: issue
state: closed
author: bdlmt
labels:
  - question
  - doc
  - rollup
assignees: []
created_at: 2021-03-31T01:44:49Z
updated_at: 2023-11-25T20:04:00Z
url: https://github.com/BurntSushi/ripgrep/issues/1838
synced_at: 2026-01-12T16:13:24Z
```

# Byte pattern detection works inconsistently

---

_@bdlmt_

#### What version of ripgrep are you using?

>ripgrep 12.1.1 (rev 7cb211378a)
>-SIMD -AVX (compiled)
>+SIMD +AVX (runtime)

Initially, I found the bug while using 11.0.2:
>ripgrep 11.0.2
>-SIMD -AVX (compiled)
>+SIMD +AVX (runtime)

#### How did you install ripgrep?

> sudo dpkg -i ./ripgrep_12.1.1_amd64.deb

#### What operating system are you using ripgrep on?

Ubuntu 20.04.2 LTS
> Linux local 5.8.0-45-generic #51~20.04.1-Ubuntu SMP Tue Feb 23 13:46:31 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux

#### Describe your bug.

Using command line mode, escaping from Unicode mode in order to scan for bytes doesn't seem to work as documented.

#### What are the steps to reproduce the behavior?

Simple to reproduce. A scan of /usr/bin will suffice.

#### What is the actual behavior?

An rg scan of /usr/bin for ELF magic values returns 0 results, while there should be over 1000 matching files:
```
user@local:~$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin/
user@local:~$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin/*
user@local:~$
```
Using the same byte pattern, a direct scan of individual files works as expected:
```
user@local:~$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin/ls
Binary file matches (found "\u{0}" byte around offset 7)
user@local:~$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin/ed
Binary file matches (found "\u{0}" byte around offset 7)
user@local:~$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin/file
Binary file matches (found "\u{0}" byte around offset 7)
user@local:~$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin/find
Binary file matches (found "\u{0}" byte around offset 7)
```
Reducing the set of files scanned seems to change the results when using a filename wildcard, but not when just just specifying the directory:
```
user@local:~$ mkdir test
user@local:~/test$ cd test
user@local:~/test$ cp /usr/bin/ls .
user@local:~/test$ cp /usr/bin/ed . 
user@local:~/test$ cp /usr/bin/file .
user@local:~/test$ cp /usr/bin/find .
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' ./*
Binary file ./ls matches (found "\u{0}" byte around offset 7)

Binary file ./find matches (found "\u{0}" byte around offset 7)

Binary file ./file matches (found "\u{0}" byte around offset 7)

Binary file ./ed matches (found "\u{0}" byte around offset 7)
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' ./
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' ./ls 
Binary file matches (found "\u{0}" byte around offset 7)
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' ./ed
Binary file matches (found "\u{0}" byte around offset 7)
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' ./file 
Binary file matches (found "\u{0}" byte around offset 7)
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' ./find 
Binary file matches (found "\u{0}" byte around offset 7)
```
Removing the \x00 bytes from the tail of the pattern changes ripgrep's behavior, and seems to work as expected:
```
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin/ | wc -l
0
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01' /usr/bin/ | wc -l
1092
```
Here are some debug outputs from the 4-file ~/test directory:
```
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' ./ --debug
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(ELF)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```
```
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01' ./* --debug
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(ELF)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
Binary file ./ls matches (found "\u{0}" byte around offset 7)
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes

Binary file ./find matches (found "\u{0}" byte around offset 7)

Binary file ./file matches (found "\u{0}" byte around offset 7)

Binary file ./ed matches (found "\u{0}" byte around offset 7)
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```
```
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01' ./ --debug
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(ELF)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
Binary file ./ls matches (found "\u{0}" byte around offset 7)
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes

Binary file ./find matches (found "\u{0}" byte around offset 7)

Binary file ./file matches (found "\u{0}" byte around offset 7)

Binary file ./ed matches (found "\u{0}" byte around offset 7)
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```
```
user@local:~/test$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01' ./ls --debug
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(ELF)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
Binary file matches (found "\u{0}" byte around offset 7)
```

#### What is the expected behavior?

ripgrep should consistently match on files in a search path which contain a specified byte pattern, using the documented method.


---

_Comment by @bdlmt on 2021-03-31 01:58_

Further test results, when copying all of /usr/bin into a local test directory. 

Somehow, there are more pattern hits after a copy:
```
user@local:~$ mkdir test2
user@local:~$ cd test2/
user@local:~/test2$ cp /usr/bin/* .
cp: -r not specified; omitting directory '/usr/bin/X11'
user@local:~/test2$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01' /usr/bin | wc -l
1092
user@local:~/test2$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01' | wc -l
1468
```
The full byte pattern search still doesn't match any files, unless individual files are targeted:
```
user@local:~/test2$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' | wc -l
0
user@local:~/test2$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' ./ls | wc -l
1
```
And here's some more --debug output:
```
user@local:~/test2$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' --debug
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(ELF)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

---

_Comment by @BurntSushi on 2021-03-31 02:16_

At first, I thought this might just be some confusion at how ripgrep handles binary files. The guide's section on binary data tries to explain the different modes. But since you are enabling binary mode via `-uuu` in all of your searches, that _should_ cause things to behave consistently.

On mobile at the moment, but I suspect there may be a bug in how ripgrep is detecting and handling binary data specifically when a match occurs. I'll take a look when I'm back on my workstation.

Out of curiosity, do your results change if you add the `--no-mmap` flag? What about `--mmap`?

---

_Comment by @BurntSushi on 2021-03-31 02:18_

(I believe there were changes in this area between the 11 and 12 releases, so please make sure you're doing all your testing with the latest release.)

---

_Comment by @bdlmt on 2021-03-31 04:45_

> (I believe there were changes in this area between the 11 and 12 releases, so please make sure you're doing all your testing with the latest release.)

All reported test results have been with latest.
(I was using 11.0.2 initially, but replicated all results in 12.1.1 for the bug report.)

> At first, I thought this might just be some confusion at how ripgrep handles binary files. The guide's section on binary data tries to explain the different modes. But since you are enabling binary mode via `-uuu` in all of your searches, that _should_ cause things to behave consistently.
> 
> On mobile at the moment, but I suspect there may be a bug in how ripgrep is detecting and handling binary data specifically when a match occurs. I'll take a look when I'm back on my workstation.
> 
> Out of curiosity, do your results change if you add the `--no-mmap` flag? What about `--mmap`?

The issue seems unchanged by '--no-mmap':
```
user@local:~/test2$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' --no-mmap | wc -l
0
```
But the results do change with '--mmap':
```
user@local:~/test2$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' --mmap | wc -l
1447
```
1447 seems to be the correct number of matches for that directory.

I tested using '--mmap' with a much larger set of files as well, and again got the expected number of matches.
(Though, slower than I expected, but maybe that's a side effect of escaping out of Unicode mode to do byte searches with regex.)

Here's a debug run with '--mmap' over the test dir with only 4 files:
```
$ rg -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' --mmap --debug
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(ELF)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexesBinary file ls matches (found "\u{0}" byte around offset 7)


Binary file file matches (found "\u{0}" byte around offset 7)

Binary file find matches (found "\u{0}" byte around offset 7)

Binary file ed matches (found "\u{0}" byte around offset 7)
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```


---

_Comment by @BurntSushi on 2021-03-31 12:17_

> (Though, slower than I expected, but maybe that's a side effect of escaping out of Unicode mode to do byte searches with regex.)

Yeah, forcing memory maps when searching a bunch of tiny files actually results in some pretty serious overhead that slows down the entire enterprise.

In any case, I am able to reproduce this on my own `/usr/bin` directory, so I'll investigate this as soon as I can. Thanks for the great bug report!

---

_Comment by @bdlmt on 2021-03-31 15:33_

> In any case, I am able to reproduce this on my own `/usr/bin` directory, so I'll investigate this as soon as I can. 

If I were to attempt to investigate and fix the issue, where's your best guess to start looking?
I'm still new to Rust, but willing to dig in.

> Thanks for the great bug report!

No problem; thank you for creating and maintaining a great project.



---

_Comment by @BurntSushi on 2021-03-31 16:36_

OK, so I took a look at this, and yeesh, this is actually behaving as _intended_. Which is kind of scary, because the UX here is just totally abysmal. This is a perfect storm, basically.

TL;DR - If you put `--no-mmap` in your ripgrep config file (or alias or whatever), then you'll get consistent behavior. For the specific case of searching for explicit binary data, you almost always want the `-a` flag. Otherwise, a `\x00` byte will never match anything, by construction.

So the top-level issue here is that binary detection/handling fundamentally works differently depending on whether memory maps are used or not.

The underlying motivation for binary handling is a desire for ripgrep to treat files as "text." Of course, ripgrep, like any grep, can also search binary files. The main problem is that you generally don't want to _accidentally_ print out results from a binary file because it can muck with your terminal via escape sequences and what not. On top of that, there is the issue that "text" files are typically line oriented where as binary data isn't. That means a single "line" in a binary data (typically a meaningless concept) can actually be quite large. Since ripgrep requires that each line be able to fit in memory, it uses a trick: it converts `\x00` bytes into line terminators in an effort to make binary data "look" like text. When this happens, the file is also marked as "binary." If a match occurs, you get the "match found but it's a binary file so we aren't going to print anything" message.

Only looking at the above, we can explain why `rg -uuu 'foo\x00\x00'` finds nothing and why `rg -uuu 'foo'` finds something:

* The third `-u` flag is synonymous with `--binary`. This causes ripgrep to disable auto-filtering of binary data. The main UX concern here is that if no match is found, then one should be able to conclude that no matches exist.
* Since the `-a/--text` flag is _not_ enabled, ripgrep does not treat everything as if it were "text." Instead, it tries to do binary detection. As stated above, this has a dual purpose: to prevent dumping arbitrary binary data to your terminal, but also to avoid using gratuitous amounts of memory for long lines in binary files. Thus, **all `\x00` bytes in every file searched are rewritten to `\n` bytes**. This is what explains why `foo\x00` won't match: `\x00` can never match because all such bytes are re-mapped to line terminators. But `foo` contains no NUL bytes, and thus can match.

All of that behavior was lifted _exactly_ from GNU grep. Including the NUL byte rewriting. However, GNU grep makes it much harder to search for NUL bytes. It doesn't recognize things like `\x00`, and inserting a NUL byte into a shell string is weird.

If that weren't confusing enough as it is, there's another catch here. When you search a single file directly, ripgrep will typically switch over to using memory maps to read the file since it tends to be faster in that case. The issue with memory maps is that we can't apply the above technique in the same way without destroying the performance benefit gained by using memory maps in the first place. In particular:

1. We can't search the entire file for `\x00` without adverse consequences. e.g., In particularly big files, in the worst case, this could require reading the entire file twice. When it comes to memory maps, they aren't searched in blocks, but rather, as a single contiguous slice. Thus binary detection is itself different for memory maps.
2. When using memory maps, we never copy the data from the actual file just for searching. Therefore, there is no real opportunity to rewrite `\x00` bytes as line terminators, even if we could find all such `\x00` bytes.

So, with memory maps, we do binary detection slightly differently:

1. We look for a `\x00` byte in the first `N` bytes of a file.
2. When printing matches or contextual lines, we search only those specific lines for a `\x00` byte. In this way, we prevent dumping binary data to the terminal.

Thus, when you search a file directly, you get matches because ripgrep is using memory maps for searching and thus doesn't rewrite `\x00` bytes as line terminators. So your pattern finds a hit.

So what could we do differently here? I don't think documentation is going to really help much here. It's such an oddball case. We could add to the relevant section on the guide with this particular case study. And in particular, we could say something like, "add `--no-mmap` to your config file to make binary data handling consistent." The performance improvement from memory maps isn't _that_ great, after all.

In terms of behavior changes, here are some things I can think of in no particular order. But I also include reasons why they problematic:

1. We could force the `--no-mmap` strategy to have the same binary handling logic as the `--mmap` strategy. e.g., By only looking for binary data in the first N bytes and otherwise only searching matching lines. This would at least avoid dumping binary data to your terminal. However, this gives up on the heuristic for avoiding large heap usages due to long "lines" in binary data. Memory maps don't have this problem since the OS handles paging things in and out of memory transparently. Having the heap explode when searching binary data seems unfortunate.
2. We could get rid of memory maps entirely. The problem here is that they do have some nice perf benefits. They also enable [certain configurations to run very very fast](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#pcre2-slow) that wouldn't otherwise be possible without memory maps.
3. We could somehow force the memory map binary handling to match the non-memory map handling. The only way I can think to do this would be to chunk up the memory map, copy data from it and rewrite its bytes like we do in the non-mmap case. But this is adding a lot of extra costs to the memory map handling that we can safely ignore precisely because of the nature of memory maps.
4. We could forcefully disable the use of memory maps whenever `--binary` mode is enabled. But this is enabled by default when searching a specific file, so this would effectively shut off memory maps completely.

---

_Label `doc` added by @BurntSushi on 2021-03-31 16:41_

---

_Label `question` added by @BurntSushi on 2021-03-31 16:41_

---

_Comment by @bdlmt on 2021-03-31 18:30_

Thank you for your detailed explanation!

> For the specific case of searching for explicit binary data, you almost always want the -a flag. Otherwise, a \x00 byte will never match anything, by construction.

Aha! I missed that in the docs. 

> That means a single "line" in a binary data (typically a meaningless concept) can actually be quite large. Since ripgrep requires that each line be able to fit in memory, it uses a trick: it converts \x00 bytes into line terminators in an effort to make binary data "look" like text. 

Okay, got it.

> The third -u flag is synonymous with --binary. 

After reading through your response and the man page, I'm not able to rectify this behavior:
```
user@local:~$ rg -a --binary '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin | wc -l
0
user@local:~$ rg -a -uuu '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin | wc -l
1081
user@local:~$ rg -a --binary --mmap '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin | wc -l
1080
user@local:~$ rg -a -uuu --mmap '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin | wc -l
1081
user@local:~$ rg -a --binary --no-mmap '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin | wc -l
0
user@local:~$ rg -a -uuu --no-mmap '(?-u)\x7f\x45\x4c\x46\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00' /usr/bin | wc -l
1081
```
`-uuu` and `--binary` seem to have different behavior when combined with `-a`, `--mmap`, and `--no-mmap`.

Assuming I understand your explanation correctly, the `-uuu` cases are behaving as expected, while the `--binary` cases don't seem to be.

(Edit: rearranged the test cases to make more sense side-by-side.)

---

_Comment by @bdlmt on 2021-03-31 19:21_

> In terms of behavior changes, here are some things I can think of in no particular order. But I also include reasons why they problematic:
> 
> 1. We could force the --no-mmap strategy to have the same binary handling logic as the --mmap strategy. e.g., By only looking for binary data in the first N bytes and otherwise only searching matching lines. This would at least avoid dumping binary data to your terminal. However, this gives up on the heuristic for avoiding large heap usages due to long "lines" in binary data. Memory maps don't have this problem since the OS handles paging things in and out of memory transparently. Having the heap explode when searching binary data seems unfortunate.
> 2. We could get rid of memory maps entirely. The problem here is that they do have some nice perf benefits. They also enable certain configurations to run very very fast that wouldn't otherwise be possible without memory maps.
> 3. We could somehow force the memory map binary handling to match the non-memory map handling. The only way I can think to do this would be to chunk up the memory map, copy data from it and rewrite its bytes like we do in the non-mmap case. But this is adding a lot of extra costs to the memory map handling that we can safely ignore precisely because of the nature of memory maps.
> 4. We could forcefully disable the use of memory maps whenever --binary mode is enabled. But this is enabled by default when searching a specific file, so this would effectively shut off memory maps completely.

Would it make sense to have this as an option as well?
5. Use memory map mode whenever --binary mode is enabled. Treat a binary file as a long line and let the OS handle paging. No \x00 replacement overhead would be required, and the heap wouldn't explode on large binaries.

---

_Comment by @BurntSushi on 2021-03-31 19:36_

@bdlmt The `-a/--text` and `--binary` flag are mutually exclusive things. They actually override each other. So `-a --binary` enables binary searching while `--binary -a` makes everything searched as if it were text. Of course, the former is equivalent to `--binary` without `-a` and the latter is equivalent to `-a` without `--binary`.

> 5. Use memory map mode whenever --binary mode is enabled. Treat a binary file as a long line and let the OS handle paging. No `\x00` replacement overhead would be required, and the heap wouldn't explode on large binaries.

It wouldn't. Aside from the downside of this seriously regressing performance when `--binary` is enabled, it's also just plain impossible. Memory maps can only be used in a subset of cases. They can't be used on streams and certain files (like `/proc/cpuinfo`) can't be memory mapped. If it was just a matter of performance regressing, then it would make sense to list it for completeness. But it's actually impossible. Standard `read` syscalls are the only universal way to search data.

---

_Comment by @bdlmt on 2021-03-31 19:58_

> The `-a/--text` and `--binary` flag are mutually exclusive things. They actually override each other. So `-a --binary` enables binary searching while `--binary -a` makes everything searched as if it were text. Of course, the former is equivalent to `--binary` without `-a` and the latter is equivalent to `-a` without `--binary`.

I see now. I read the man page incorrectly. I misunderstood this line under `--binary`:
```
This flag can be disabled with --no-binary. It overrides the -a/--text flag.
```

I thought "It" referred to `--no-binary`, not `--binary`. I should have realized I had it wrong, based on the intended nature of `-a/--text` and `--binary`.

> They can't be used on streams and certain files (like /proc/cpuinfo) can't be memory mapped.

Good point; I wasn't thinking far enough outside my recent use case.



---

_Comment by @bdlmt on 2021-04-01 18:48_

> 1. We could force the --no-mmap strategy to have the same binary handling logic as the --mmap strategy. e.g., By only looking for binary data in the first N bytes and otherwise only searching matching lines. This would at least avoid dumping binary data to your terminal. However, this gives up on the heuristic for avoiding large heap usages due to long "lines" in binary data. Memory maps don't have this problem since the OS handles paging things in and out of memory transparently. Having the heap explode when searching binary data seems unfortunate.

From a UX point of view, behavior change option **1** seems best. Mixing `-a/--text` with `-uuu` to enable text mode during a binary search in order to successfully match on raw `\x00` bytes is a bit awkward and not intuitive. The change would also address the unexpected disparity between using `-a/--text` with `--binary` (mutually exclusive) vs. using `-a/--text` with `-uuu` (text mode enabled on binaries), because `-a/--text` would no longer be necessary when matching on `\x00` bytes.

From a performance point of view, it seems like leaving it as-is would be best, unless there's a way to split a binary file into multiple "lines" without replacing bytes.

As a user, I like the sound of improving the UX, but obviously not if there's a significant performance hit.



---

_Label `rollup` added by @BurntSushi on 2023-11-25 15:40_

---

_Closed by @BurntSushi on 2023-11-25 20:04_

---
