```yaml
number: 2979
title: ripgrep fails to match inside a specific binary
type: issue
state: closed
author: benjarobin
labels: []
assignees: []
created_at: 2025-01-25T19:11:53Z
updated_at: 2025-01-25T19:30:25Z
url: https://github.com/BurntSushi/ripgrep/issues/2979
synced_at: 2026-01-12T16:13:25Z
```

# ripgrep fails to match inside a specific binary

---

_@benjarobin_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)
```

### How did you install ripgrep?

Arch Linux package:
```
pacman -Q ripgrep
ripgrep 14.1.1-1
```

### What operating system are you using ripgrep on?

Arch Linux up to date

### Describe your bug.

ripgrep fail to find a match inside attached binary.
ripgrep find the same fix string pattern inside others various binary. But not inside this specific one...


### What are the steps to reproduce the behavior?

 -  Decompress attached [test.bin.gz](https://github.com/user-attachments/files/18547550/test.bin.gz) in order to obtain `test.bin` file
```
gzip -d test.bin.gz
```
 - Search for fixed string `Hello custom0 board` inside the attached binary.



### What is the actual behavior?

 - `strings test.bin | grep "Hello custom0 board"`
```
&Hello custom0 board (U-boot)
```

- `grep -a --files-with-matches -F "Hello custom0 board" test.bin`
```
test.bin
```

- `rg --trace --debug --binary --files-with-matches -F "Hello custom0 board" test.bin`
```
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? true
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: benjarobin-fixe
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
rg: DEBUG|grep_regex::config|/usr/src/debug/ripgrep/ripgrep-14.1.1/crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

### What is the expected behavior?

rg finds a match, and prints the filename, which is `test.bin`

---

_Closed by @BurntSushi on 2025-01-25 19:27_

---

_Comment by @BurntSushi on 2025-01-25 19:29_

This was a tricky one. It looks like a bug, but your binary file is not actually binary. ripgrep interprets it as UTF-16BE because it has a BOM indicating as such. This causes ripgrep to transcode the file as UTF-16BE. That in turn means standard ASCII patterns won't behave as expected. And thus you don't get a match.

For cases like this, you actually need to disable transcoding entirely:

```
$ rg --trace "Hello" test.bin
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? true
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: duff
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
rg: DEBUG|grep_regex::config|/home/andrew/code/rust/ripgrep/crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: TRACE|grep_regex::matcher|/home/andrew/code/rust/ripgrep/crates/regex/src/matcher.rs:66: final regex: "(?:Hello)"
rg: TRACE|grep_regex::literal|crates/regex/src/literal.rs:75: skipping inner literal extraction, existing regex is believed to already be accelerated
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: TRACE|rg::search|crates/core/search.rs:254: test.bin: binary detection: BinaryDetection(Convert(0))
rg: TRACE|grep_searcher::searcher|/home/andrew/code/rust/ripgrep/crates/searcher/src/searcher/mod.rs:670: Some("test.bin"): searching via memory map
rg: TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:1007: found byte-order mark (BOM) for encoding Encoding { UTF-16BE }
rg: TRACE|grep_searcher::searcher|/home/andrew/code/rust/ripgrep/crates/searcher/src/searcher/mod.rs:763: slice reader: needs transcoding, using generic reader
rg: TRACE|grep_searcher::searcher|/home/andrew/code/rust/ripgrep/crates/searcher/src/searcher/mod.rs:742: generic reader: searching via roll buffer strategy
rg: TRACE|grep_searcher::searcher::core|/home/andrew/code/rust/ripgrep/crates/searcher/src/searcher/core.rs:65: searcher core: will use fast line searcher

$ rg -E none "Hello" test.bin
binary file matches (found "\0" byte around offset 40)

$ rg -E none -a -M100 "Hello" test.bin
13701:[Omitted long line with 1 matches]

$ rg -E none -a -o ".{0,50}Hello.{0,50}" test.bin
13701:lnx,zynq-7000&Hello custom0 board (U-boot)optionsu-boot
```

I've added an extra TRACE-level log message that might help diagnose this in the future (as shown above as well):

```
rg: TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:1007: found byte-order mark (BOM) for encoding Encoding { UTF-16BE }
```


---
