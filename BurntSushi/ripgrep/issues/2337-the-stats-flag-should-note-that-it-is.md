```yaml
number: 2337
title: "the `--stats` flag should note that it is unconditionally enabled when `--json` is used"
type: issue
state: closed
author: cstyles
labels:
  - doc
  - rollup
assignees: []
created_at: 2022-10-24T06:25:14Z
updated_at: 2023-11-25T20:03:58Z
url: https://github.com/BurntSushi/ripgrep/issues/2337
synced_at: 2026-01-12T16:13:24Z
```

# the `--stats` flag should note that it is unconditionally enabled when `--json` is used

---

_@cstyles_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS 12.6

#### Describe your bug.

When ripgrep is invoked with `--json`, the final line is always a stats summary. It seems like this is included implicitly whenever `--json` is used. However, I would expect that explicitly including `--no-stats` would disable the summary.

#### What are the steps to reproduce the behavior?

Invoke ripgrep with both the `--json` and `--no-stats` flags.

#### What is the actual behavior?

```
$ rg --debug --json --no-stats pattern file.txt
DEBUG|rg::config|crates/core/config.rs:40: /Users/cstyles/.ripgreprc: arguments loaded from config file: ["--smart-case", "--max-columns=300", "--max-columns-preview"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--smart-case", "--max-columns=300", "--max-columns-preview", "--debug", "--json", "--no-stats", "pattern", "file.txt"]
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:109: required literals found: [Cut(PATTE), Cut(pATTE), Cut(PaTTE), Cut(paTTE), Cut(PAtTE), Cut(pAtTE), Cut(PatTE), Cut(patTE), Cut(PATtE), Cut(pATtE), Cut(PaTtE), Cut(paTtE), Cut(PAttE), Cut(pAttE), Cut(PattE), Cut(pattE), Cut(PATTe), Cut(pATTe), Cut(PaTTe), Cut(paTTe), Cut(PAtTe), Cut(pAtTe), Cut(PatTe), Cut(patTe), Cut(PATte), Cut(pATte), Cut(PaTte), Cut(paTte), Cut(PAtte), Cut(pAtte), Cut(Patte), Cut(patte)]
DEBUG|grep_regex::matcher|crates/regex/src/matcher.rs:50: extracted fast line regex: (?-u:PATTE|pATTE|PaTTE|paTTE|PAtTE|pAtTE|PatTE|patTE|PATtE|pATtE|PaTtE|paTtE|PAttE|pAttE|PattE|pattE|PATTe|pATTe|PaTTe|paTTe|PAtTe|pAtTe|PatTe|patTe|PATte|pATte|PaTte|paTte|PAtte|pAtte|Patte|patte)
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 4 basenames, 2 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
{"type":"begin","data":{"path":{"text":"file.txt"}}}
{"type":"match","data":{"path":{"text":"file.txt"},"lines":{"text":"pattern\n"},"line_number":2,"absolute_offset":7,"submatches":[{"match":{"text":"pattern"},"start":0,"end":7}]}}
{"type":"end","data":{"path":{"text":"file.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":42958,"human":"0.000043s"},"searches":1,"searches_with_match":1,"bytes_searched":21,"bytes_printed":233,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.001521s","nanos":1521416,"secs":0},"stats":{"bytes_printed":233,"bytes_searched":21,"elapsed":{"human":"0.000043s","nanos":42958,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

#### What is the expected behavior?

Including the `--no-stats` flag should disable the stats summary.


---

_Comment by @BurntSushi on 2022-10-24 12:10_

This is intended behavior at the moment: https://github.com/BurntSushi/ripgrep/blob/eab044d829e1f5bcd1d3fd6a6f49ff732240da09/crates/core/args.rs#L1558-L1566

I'm not inclined to change this. But the `--stats` flag could use an extra sentence noting that it appears in the JSON output unconditionally and can't be disabled. There is no real benefit to disabling it. It only uses one line, and the JSON format itself requires the work needed to compute the stats anyway. So there's really no extra cost.

---

_Label `doc` added by @BurntSushi on 2022-10-24 12:10_

---

_Renamed from "When invoked with `--json`, ripgrep does not honor the `--no-stats` flag" to "the `--stats` flag should note that it is unconditionally enabled when `--json` is used" by @BurntSushi on 2022-10-24 12:10_

---

_Comment by @cstyles on 2022-10-24 18:23_

> So there's really no extra cost.

The cost comes in the form of complexity in whatever consumes the JSON output. Even if I don't care about the statistics, I still need to add logic to either handle or ignore them. It's not a huge deal but being able to exclude them would be nice and, IMO, more intuitive and consistent. Ultimately it's your call though.

---

_Comment by @BurntSushi on 2022-10-24 18:27_

I acknowledge there is an increase in complexity, but one that is extremely small IMO. Small enough to keep it unconditionally enabled.

---

_Comment by @cstyles on 2022-10-25 23:29_

True. My thought was that the required change to ripgrep / grep-printer would also be extremely small. That said, if you disagree I won't be offended. Feel free to close this.

---

_Label `rollup` added by @BurntSushi on 2023-11-24 19:10_

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---
