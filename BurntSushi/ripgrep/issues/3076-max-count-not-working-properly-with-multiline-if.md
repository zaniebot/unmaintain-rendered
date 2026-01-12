```yaml
number: 3076
title: "`--max-count` not working properly with `--multiline` if matches are on consecutive lines"
type: issue
state: closed
author: underflow00
labels:
  - bug
  - rollup
assignees: []
created_at: 2025-06-24T15:44:23Z
updated_at: 2025-09-20T01:08:48Z
url: https://github.com/BurntSushi/ripgrep/issues/3076
synced_at: 2026-01-12T16:13:25Z
```

# `--max-count` not working properly with `--multiline` if matches are on consecutive lines

---

_@underflow00_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1 (rev 4649aa9700)

features:+pcre2

PCRE2 10.43 is available (JIT is unavailable)

### How did you install ripgrep?

Download from https://github.com/BurntSushi/ripgrep/releases/tag/14.1.1

### What operating system are you using ripgrep on?

Windows 11

### Describe your bug.

For the purpose of limiting count by `--max-count`, with the mutiline flag, the program seems to be counting matches on consecutive lines as a single match, resulting in extra matches.


### What are the steps to reproduce the behavior?

1. Create a file with contents
```
line 2
line 3 x
line 2
line 3
```

2. Run `rg --max-count=1 -U "line 2\nline 3" <file> --stats --debug`.


### What is the actual behavior?

```
rg: DEBUG|rg::flags::parse|crates/core\flags\parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1094: is_one_file? true
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1269: found hostname for hyperlink configuration: d1
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:174: using 1 thread(s)
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: gzip: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: gzip: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: bzip2: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: bzip2: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: xz: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: xz: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: lz4: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: xz: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: brotli: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: zstd: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: zstd: could not find executable in PATH
rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: uncompress: could not find executable in PATH
1:line 2
2:line 3 x
3:line 2
4:line 3

2 matches
4 matched lines
1 files contained matches
1 files searched
38 bytes printed
29 bytes searched
0.000346 seconds spent searching
0.014687 seconds
```

### What is the expected behavior?

Output only the first 2 lines (1 match).

---

_Label `bug` added by @BurntSushi on 2025-06-24 15:49_

---

_Comment by @fspv on 2025-07-06 16:33_

It seems like this is intentional: https://github.com/BurntSushi/ripgrep/blob/3b7fd442a6f3aa73f650e763d7cbb902c03d700e/crates/searcher/src/searcher/glue.rs#L237

Matches on the adjacent lines are grouped together and they're passed to the sink only after all the matches are found.

If we simply change this condition from ">=" to ">" it works as expected.

If we do that all the tests pass except for this one:

```
failures:

---- standard::tests::replacement_multi_line stdout ----

thread 'standard::tests::replacement_multi_line' panicked at crates/printer/src/standard.rs:3432:9:

printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:hello?world?

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:hello?
2:world?

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

This is the test introduced in the issue mentioned in the comment in the code: https://github.com/BurntSushi/ripgrep/issues/1311

So the fix is simple, but it will break that other use case.

---

_Comment by @fspv on 2025-07-06 17:03_

Just some more thoughts.

Seems like `--max-count`, which is later referred as `max_matches` in the code is only used in the sink. But if we merge matches together sink can't control how many matches will be passed to it and can only react to the sinked matches. So if we go over the limit, there is nothing that sink can do here.

One way to tackle this issue would be to expose `max_matches` to the `MultiLine` Searcher object, count matches there as well and stop early when we reached `max_matches` even though there might be some more matches on the next line.

This approach should work, but I'm not sure if it is a good idea from the design point of view. 

Example fix: https://github.com/BurntSushi/ripgrep/pull/3094

---

_Label `rollup` added by @BurntSushi on 2025-09-18 19:09_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
