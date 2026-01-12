```yaml
number: 3244
title: problem(s) with binary search and raw bytes
type: issue
state: closed
author: fenugrec
labels: []
assignees: []
created_at: 2025-12-13T02:51:58Z
updated_at: 2025-12-13T02:58:50Z
url: https://github.com/BurntSushi/ripgrep/issues/3244
synced_at: 2026-01-12T16:13:25Z
```

# problem(s) with binary search and raw bytes

---

_@fenugrec_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

15.1.0

### How did you install ripgrep?

archlinux package

### What operating system are you using ripgrep on?

archlinux

### Describe your bug.

rg has issues with searching for raw bytes in binary files

### What are the steps to reproduce the behavior?

1.  create a file with some choice bytes, `echo "ed 15 ff 00 55" | xxd -p -r > test.bin`
2. try a few searches , even the simplest
```
rg --binary '\xED' test.bin --trace

rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:954: read CWD from environment: ~
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1092: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1103: is_one_file? true
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1278: found hostname for hyperlink configuration: 
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1288: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:175: using 1 thread(s)
rg: DEBUG|ignore::gitignore|crates/ignore/src/gitignore.rs:398: opened gitignore file: ~/.gitignore
rg: DEBUG|globset|crates/globset/src/lib.rs:515: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: TRACE|grep_regex::matcher|/usr/src/debug/ripgrep/ripgrep-15.1.0/crates/regex/src/matcher.rs:66: final regex: "í"
rg: TRACE|grep_regex::literal|crates/regex/src/literal.rs:74: skipping inner literal extraction, existing regex is believed to already be accelerated
rg: TRACE|rg::search|crates/core/search.rs:255: test.bin: binary detection: BinaryDetection(Convert(0))
rg: TRACE|grep_searcher::searcher|/usr/src/debug/ripgrep/ripgrep-15.1.0/crates/searcher/src/searcher/mod.rs:690: Some("test.bin"): searching via memory map
rg: TRACE|grep_searcher::searcher|/usr/src/debug/ripgrep/ripgrep-15.1.0/crates/searcher/src/searcher/mod.rs:792: slice reader: searching via slice-by-line strategy
rg: TRACE|grep_searcher::searcher::core|/usr/src/debug/ripgrep/ripgrep-15.1.0/crates/searcher/src/searcher/core.rs:67: searcher core: will use fast line searcher
```

no hits. So this is not a utf BOM issue, but I wonder... is something converting my 0xED into a possibly-multibyte UTF-8 sequence to represent that `final regex: "í"` character ?


### What is the actual behavior?

No hits when I know there are some

### What is the expected behavior?

Keep intact the raw bytes I provided

---

_Locked by @ghost on 2025-12-13 02:58_

---
