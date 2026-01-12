```yaml
number: 2473
title: Inconsistent behavior when replacing newlines
type: issue
state: closed
author: misaki-web
labels:
  - duplicate
assignees: []
created_at: 2023-03-22T21:40:08Z
updated_at: 2023-03-22T21:56:25Z
url: https://github.com/BurntSushi/ripgrep/issues/2473
synced_at: 2026-01-12T16:13:24Z
```

# Inconsistent behavior when replacing newlines

---

_@misaki-web_

Using ripgrep 13.0.0 on Ubuntu 22.10, the last line of data is sometimes duplicated in the output when replacing newlines.

Example 1 (no issue):

```bash
$ echo -en '1\t2\n3\t4' | rg --debug --trace --passthru -U -e '\n|\t' -r '*'
TRACE|grep_regex::matcher|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-regex-0.1.9/src/matcher.rs:54: final regex: "\n|\t"
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: <stdin>: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-searcher-0.1.8/src/searcher/mod.rs:719: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-searcher-0.1.8/src/searcher/mod.rs:723: generic reader: searching via multiline strategy
1*2*3*4
```

Example 2 (issue):

```bash
$ echo -en '1\t2\n3\t4' | rg --debug --trace --passthru -U -e '\n' -r '*'
TRACE|grep_regex::matcher|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-regex-0.1.9/src/matcher.rs:54: final regex: "\n"
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: <stdin>: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-searcher-0.1.8/src/searcher/mod.rs:719: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-searcher-0.1.8/src/searcher/mod.rs:723: generic reader: searching via multiline strategy
1	2*3	4
3	4
```

The example 2 is solved by removing `--passthru`:

```bash
$ echo -en '1\t2\n3\t4' | rg --debug --trace -U -e '\n' -r '*'
TRACE|grep_regex::matcher|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-regex-0.1.9/src/matcher.rs:54: final regex: "\n"
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:334: <stdin>: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-searcher-0.1.8/src/searcher/mod.rs:719: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/grep-searcher-0.1.8/src/searcher/mod.rs:723: generic reader: searching via multiline strategy
1	2*3	4
```


---

_Comment by @BurntSushi on 2023-03-22 21:56_

Duplicate of #2095 and #2208 and #2438. Was fixed in 91afd4214a9ada15e475bba02694f4fb50079cbc, which is on current master but has not yet been released.

---

_Closed by @BurntSushi on 2023-03-22 21:56_

---

_Label `duplicate` added by @BurntSushi on 2023-03-22 21:56_

---
