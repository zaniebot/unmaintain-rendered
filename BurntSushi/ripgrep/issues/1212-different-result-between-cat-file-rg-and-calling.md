```yaml
number: 1212
title: "different result between `cat file | rg` and calling rg directly"
type: issue
state: closed
author: vrischmann
labels:
  - question
assignees: []
created_at: 2019-03-08T15:39:52Z
updated_at: 2019-03-08T15:51:57Z
url: https://github.com/BurntSushi/ripgrep/issues/1212
synced_at: 2026-01-12T16:13:23Z
```

# different result between `cat file | rg` and calling rg directly

---

_@vrischmann_

#### What version of ripgrep are you using?

    ripgrep 0.10.0 (rev 8a7db1a918)
    -SIMD -AVX (compiled)
    +SIMD +AVX (runtime)

#### How did you install ripgrep?

From the deb file from GitHub.

#### What operating system are you using ripgrep on?

Debian 9.8, kernel 4.9

#### If this is a bug, what are the steps to reproduce the behavior?

First of all I'm not sure it's a bug directly with ripgrep.

I have a file of size `34359738196` (32GB ~).

I'm seeing a difference when calling ripgrep via a pipe, like this:

    $ cat /data/vincent/x32 | rg -N --no-filename --stats foobar
    DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(foobar)], limit_size: 250, limit_class: 10 }
    DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    
    0 matches
    0 matched lines
    0 files contained matches
    1 files searched
    0 bytes printed
    4403625046 bytes searched
    3.140673 seconds spent searching
    3.143315 seconds


and calling ripgrep directly with the file, like this:

    $ rg -N --no-filename --stats foobar /data/vincent/x32
    DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(foobar)], limit_size: 250, limit_class: 10 }
    DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
    
    0 matches
    0 matched lines
    0 files contained matches
    1 files searched
    0 bytes printed
    34359738196 bytes searched
    12.918476 seconds spent searching
    13.652422 seconds

Unfortunately I can't upload the file, it's full of sensitive information. I could generate an equivalent file of the same size and test that. 

I also tested on another server with Debian 9.6, kernel 4.9 and I get the exact same bytes searched result.

#### If this is a bug, what is the expected behavior?

I would expect piping to ripgrep to produce equivalent results.

---

_Comment by @BurntSushi on 2019-03-08 15:45_

It's almost certainly because the file contains a `NUL` byte, and therefore, binary detection kicks in. Unfortunately, binary detection depends on the method of searching. In your first example, ripgrep is forced to use a roll buffer. In the latter example, ripgrep is probably memory mapping it. To confirm, use the `-a/--text` flag to force searching (just like you would with `grep`).

There are a couple other tickets open for improving this state of affairs with respect to binary data.

If this doesn't fix your problem, then please show the output with the `--trace` flag for each. It would also help if there was a way to minimize the input. But try the `-a` flag first.

---

_Label `question` added by @BurntSushi on 2019-03-08 15:45_

---

_Comment by @vrischmann on 2019-03-08 15:51_

Thanks, that was it.

I actually thought about that and used `hexdump -s 4403625046 -n 100` to take a look at the data but didn't see a NUL byte anywhere so I assumed that wasn't it, but the byte is probably somewhere before or after then. 

---

_Closed by @vrischmann on 2019-03-08 15:51_

---
