```yaml
number: 2852
title: "--count --multiline doesn't behave as documented"
type: issue
state: closed
author: jeanas
labels:
  - bug
  - doc
assignees: []
created_at: 2024-07-14T14:04:26Z
updated_at: 2025-09-23T02:02:07Z
url: https://github.com/BurntSushi/ripgrep/issues/2852
synced_at: 2026-01-12T16:13:25Z
```

# --count --multiline doesn't behave as documented

---

_@jeanas_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

14.1.0

### How did you install ripgrep?

Reproduces with distro package and `cargo install`ed version.

### What operating system are you using ripgrep on?

Fedora 40

### Describe your bug.

From the `--help` output:

```
    -c, --count
        This flag suppresses normal output and shows the number of lines that
        match the given patterns for each file searched. Each file containing a
        match has its path and count printed on each line. Note that unless
        -U/--multiline is enabled, this reports the number of lines that match
        and not the total number of matches. In multiline mode, -c/--count is
        equivalent to --count-matches.
```

However, the behavior I'm seeing is that `--count` still behaves as "count the number matching lines" and not as "count the number of matches" even under multiline mode.

### What are the steps to reproduce the behavior?

```
$ cat file.txt 
match match match match
match match match match
```


### What is the actual behavior?

```
$ rg --count match file.txt
2

$ rg --count-matches match file.txt
8

$ rg --count --multiline match file.txt
2
```

### What is the expected behavior?

The last command should print 8 (or the documentation should be changed).

---

_Comment by @JOSBEAK on 2024-07-14 17:34_

I would like to work on this issue.. Can anyone help me how should I get started ?


---

_Comment by @jeanas on 2024-07-14 22:32_

I haven't looked much at the code, but I think I get what's happening.

The help says

```
           WARNING: Because of how the underlying regex  engine  works,  multiline
           searches may be slower than normal line-oriented searches, and they may
           also  use  more  memory. In particular, when multiline mode is enabled,
           ripgrep requires that each file it searches is laid out contiguously in
           memory (either by reading it onto the heap or  by  memory-mapping  it).
           Things  that  cannot  be memory-mapped (such as stdin) will be consumed
           until EOF before searching can begin. In general, ripgrep will only  do
           these  things when necessary.  Specifically, if the -U/--multiline flag
           is provided but the regex does not contain patterns that would match \n
           characters, then ripgrep will automatically  avoid  reading  each  file
           into  memory before searching it.  Nevertheless, if you only care about
           matches spanning at most one line, then it is always better to  disable
           multiline mode.
```

And sure enough:

```
$ cat file.txt 
start end
start end

$ rg --count-matches "start|end" file.txt 
4

$ rg --count "start|end" file.txt 
2

$ rg --count --multiline "start|end" file.txt 
2

$ rg --count --multiline "^start|end$" file.txt 
4
```

In words: when the regex doesn't contain `^` or `$`, ripgrep notices that multiline mode is useless and runs the normal, non-multiline mode, but then this changes the semantics of `--count`.

---

_Comment by @meedstrom on 2024-08-21 19:26_

Could it be related to #2779?

---

_Label `bug` added by @BurntSushi on 2025-09-23 01:53_

---

_Label `doc` added by @BurntSushi on 2025-09-23 01:59_

---

_Closed by @BurntSushi on 2025-09-23 02:02_

---
