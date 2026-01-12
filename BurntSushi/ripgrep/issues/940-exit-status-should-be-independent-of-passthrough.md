```yaml
number: 940
title: Exit status should be independent of --passthrough
type: issue
state: closed
author: jzinn
labels:
  - bug
  - question
assignees: []
created_at: 2018-06-06T08:29:36Z
updated_at: 2018-08-20T11:10:22Z
url: https://github.com/BurntSushi/ripgrep/issues/940
synced_at: 2026-01-12T16:13:22Z
```

# Exit status should be independent of --passthrough

---

_@jzinn_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 0.8.1
-SIMD -AVX
```

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS 10.13.4

#### Describe your question, feature request, or bug.

The command
```
rg --passthrough searchterm file.txt
```
should exit with the same code as when the command is run without the flag.

Both `ack` and `ag` work this way.

#### If this is a bug, what are the steps to reproduce the behavior?

```
$ echo hi > file
$ rg asdf file
$ echo $?
1
$ rg --passthrough asdf file
1:hi
$ echo $?
0
```

#### If this is a bug, what is the actual behavior?

`ripgrep --passthrough` always exits with `0` (unless the file is empty).

```
$ rg --passthrough --debug asdf file
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Alternate(
    [
        Literal {
            chars: [
                'a',
                's',
                'd',
                'f'
            ],
            casei: false
        },
        StartText
    ]
)
DEBUG/globset/globset/src/lib.rs:401: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
1:hi
```

#### If this is a bug, what is the expected behavior?

To exit with the same status as without the flag.


---

_Comment by @BurntSushi on 2018-06-06 11:39_

This is an interesting bug. I will see what I can do about it, but this may require too much implementation complexity to fix.

> Both ack and ag work this way.

It does appear that ack does, but I can't even get ag's passthrough option to work. In general, ag should not be used as a model for correctness.

---

_Label `bug` added by @BurntSushi on 2018-06-06 11:39_

---

_Label `question` added by @BurntSushi on 2018-06-06 11:39_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
