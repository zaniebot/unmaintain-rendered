```yaml
number: 2836
title: "Some directories in .ignore aren't ignored"
type: issue
state: closed
author: jknoos
labels:
  - bug
  - rollup
  - gitignore
assignees: []
created_at: 2024-06-08T22:31:25Z
updated_at: 2025-09-20T01:08:22Z
url: https://github.com/BurntSushi/ripgrep/issues/2836
synced_at: 2026-01-12T16:13:25Z
```

# Some directories in .ignore aren't ignored

---

_@jknoos_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

### How did you install ripgrep?

pacman

### What operating system are you using ripgrep on?

Arch Linux

### Describe your bug.

Some directories in ~/.ignore aren't ignored correctly.

### What are the steps to reproduce the behavior?

Run this script `~/test` under `~/testdir`:

```
#!/bin/bash

mkdir -p ~/testdir/sub/sub2
cd ~/testdir
chmod -r sub/sub2
echo 'sub/sub2' >> ~/.ignore
echo 'sub/sub2/' >> ~/.ignore
echo '/testdir/sub/sub2' >> ~/.ignore
echo '/testdir/sub/sub2/' >> ~/.ignore
rg --debug test
```

### What is the actual behavior?

```
[~/testdir]$ ~/test
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1109: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: t480s
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 8 thread(s)
rg: DEBUG|grep_regex::config|crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 8 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: ./sub/sub2: Permission denied (os error 13)
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
[~/testdir]$ 
```

### What is the expected behavior?

`~/testdir/sub/sub2` should be ignored, i.e. the `rg: ./sub/sub2: Permission denied (os error 13)` error is unexpected.

---

_Comment by @BurntSushi on 2024-06-09 11:54_

Thanks for the bug report. I can reproduce this. Although here is something much simpler that doesn't rely on cluttering your `$HOME` directory:

```
$ mkdir -p /tmp/rg2836/testdir/sub/sub2
$ chmod -r /tmp/rg2836/testdir/sub/sub2
$ echo '/testdir/sub/sub2/' > /tmp/rg2836/.ignore
```

And then:

```
$ cd /tmp/rg2836
$ rg --files
create
$ cd testdir/
$ rg --files
rg: ./sub/sub2: Permission denied (os error 13)
```

Moving into the sub-directory should still result in the `.ignore` file being respected. But there's probably an issue dealing with stripping the right prefix of candidate paths with respect to the directory of the `.ignore` file.

This may also be related to #829 and #278, although not necessarily so.

---

_Label `bug` added by @BurntSushi on 2024-06-09 11:55_

---

_Label `gitignore` added by @BurntSushi on 2024-06-09 11:55_

---

_Comment by @tmccombs on 2024-08-08 05:45_

I think https://github.com/sharkdp/fd/issues/1591 may be a case of this.

---

_Comment by @gstokkink on 2024-08-20 14:16_

@BurntSushi https://github.com/BurntSushi/ripgrep/issues/2770 might also be related

---

_Label `rollup` added by @BurntSushi on 2025-07-11 16:41_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
