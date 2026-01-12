```yaml
number: 2884
title: Alternation match regression
type: issue
state: closed
author: codido
labels:
  - bug
assignees: []
created_at: 2024-09-09T01:05:45Z
updated_at: 2024-09-09T04:18:05Z
url: https://github.com/BurntSushi/ripgrep/issues/2884
synced_at: 2026-01-12T16:13:25Z
```

# Alternation match regression

---

_@codido_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

14.1.0

### How did you install ripgrep?

Cargo

### What operating system are you using ripgrep on?

Fedora 40

### Describe your bug.

ripgrep fails to return some matches when case is ignored. This only happens with very particular matches and specific characters.

A git bisect seems to suggest this is a regression introduced by
ca740d9ace9065103036c101ca973b79e59ae85a.

### What are the steps to reproduce the behavior?

Run the following:
```
echo -n "e-x" | rg -i "e.x|ex"
```


### What is the actual behavior?

```
$ echo -n "e-x" | rg --debug -i "e.x|ex"
rg: DEBUG|rg::flags::config|/home/ido/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/config.rs:41: /home/ido/.ripgreprc: arguments loaded from config file: []
rg: DEBUG|rg::flags::parse|/home/ido/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|/home/ido/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|/home/ido/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1112: heuristic chose to search stdin
rg: DEBUG|rg::flags::hiargs|/home/ido/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: sourcerer
rg: DEBUG|rg::flags::hiargs|/home/ido/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|/home/ido/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:174: using 1 thread(s)
rg: DEBUG|grep_regex::literal|/home/ido/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-regex-0.1.12/src/literal.rs:117: extracted fast line regex: "(?:(?:EX)|(?:Ex)|(?:eX)|(?:ex))"
rg: DEBUG|globset|/home/ido/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
$
```

### What is the expected behavior?

ripgrep should have returned `e-x` instead of returning no matches.
The same occurs when `e` is replaced with `k`, `s`, or `t`, but doesn't with any other alphabetic character, or if case isn't ignored.

---

_Label `bug` added by @BurntSushi on 2024-09-09 01:44_

---

_Comment by @BurntSushi on 2024-09-09 02:00_

Excellent find. When you find weird behaviors like this where small perturbations to the regex make the bug go away, it almost always points toward literal extraction. In this case, the bug was in ripgrep's inner literal extractor (which isn't in the regex engine), and that only runs when the regex engine believes its own optimizations "aren't great." And there are tons of layers here. In this case, multiple optimizations and heuristics have to come together to produce an incorrect result.

This is also made clearer in ripgrep's trace logging, which shows the literals it extracted which are clearly incorrect:

```
$ echo "e-x" | rg-14.1.0 "(?i:e.x|ex)" --trace
rg: DEBUG|rg::flags::config|crates/core/flags/config.rs:41: /home/andrew/.ripgreprc: arguments loaded from config file: ["--max-columns-preview", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:0xff,0xff,0xff", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go"]
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1112: heuristic chose to search stdin
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: duff
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
rg: TRACE|grep_regex::matcher|crates/regex/src/matcher.rs:66: final regex: "(?:[Ee](?:(?:[\0-\t\u{b}-\u{10ffff}][Xx])|[Xx]))"
rg: TRACE|grep_regex::literal|crates/regex/src/literal.rs:154: extracted inner literals: Seq[E("EX"), E("Ex"), E("EX"), E("Ex"), E("eX"), E("ex"), E("eX"), E("ex")]
rg: TRACE|grep_regex::literal|crates/regex/src/literal.rs:156: extracted inner literals after optimization: Seq[E("EX"), E("Ex"), E("eX"), E("ex")]
rg: DEBUG|grep_regex::literal|crates/regex/src/literal.rs:117: extracted fast line regex: "(?:(?:EX)|(?:Ex)|(?:eX)|(?:ex))"
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: TRACE|rg::search|crates/core/search.rs:254: <stdin>: binary detection: BinaryDetection(Convert(0))
rg: TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:743: generic reader: searching via roll buffer strategy
rg: TRACE|grep_searcher::searcher::core|crates/searcher/src/searcher/core.rs:65: searcher core: will use fast line searcher
```

Specifically, this bit:

```
extracted fast line regex: "(?:(?:EX)|(?:Ex)|(?:eX)|(?:ex))"
```

Clearly, this regex won't match the input, and thus ripgrep reports a false negative.

I fixed this in #2885.

---

_Closed by @BurntSushi on 2024-09-09 02:00_

---

_Closed by @BurntSushi on 2024-09-09 02:00_

---

_Comment by @BurntSushi on 2024-09-09 02:32_

This should be fixed in the [14.1.1 release](https://github.com/BurntSushi/ripgrep/releases/tag/14.1.1).

---

_Comment by @codido on 2024-09-09 04:18_

Thank you @BurntSushi!

---
