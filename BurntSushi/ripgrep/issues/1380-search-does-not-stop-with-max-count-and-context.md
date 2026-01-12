```yaml
number: 1380
title: Search does not stop with --max-count and --context with matches in the context lines
type: issue
state: closed
author: dsvf
labels:
  - bug
  - rollup
assignees: []
created_at: 2019-09-16T07:32:55Z
updated_at: 2021-06-01T01:51:25Z
url: https://github.com/BurntSushi/ripgrep/issues/1380
synced_at: 2026-01-12T16:13:23Z
```

# Search does not stop with --max-count and --context with matches in the context lines

---

_@dsvf_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

Github binary release

#### What operating system are you using ripgrep on?

Linux, RHEL 7.7

#### Describe your question, feature request, or bug.

Unexpected behavior when combining --max-count with --context, if there are additional matches in the context. This is not really a bug, more like inconsistent behavior.

I am searching for the first match in one file and want to print the next _n_ lines.
If there are no additional matches in the context, then the search output stops after the first result and prints the next _n_ lines.
If there is an additional match within the next _n_ lines, then (it seems like) rg treats this as a match and restarts its internal context line counter, printing an additional _n_ lines. 

I am aware that --max-count is a limit on per-line matches. But it _works_ for some input files and it does not work for some other files, which is unexpected. Also, it appears somewhat similar to #402.

#### If this is a bug, what are the steps to reproduce the behavior?
```
> cat rgtest.txt
a
b
c
d
e
d
e
d
e
d
e
```

#### If this is a bug, what is the actual behavior?

Working as expected: Print first result and next line:
```
> rg -m 1 -A 1 d rgtest.txt
4:d
5-e
```
Working as expected: Do "print first result and next line" twice:
```
> rg -m 2 -A 1 d rgtest.txt
4:d
5-e
6:d
7-e
```

Unexpected: Print first result and next two lines
```
> rg -m 1 -A 2 d rgtest.txt
4:d
5-e
6:d
7-e
8:d
9-e
10:d
11-e
12-
```
Expected result:
```
4:d
5-e
6:d
```

#### If this is a bug, what is the expected behavior?
Not a bug, but a wish for consistency (i.e. _please fix it for my specific use case_)

---

_Comment by @BurntSushi on 2019-09-16 11:42_

This does actually look like a bug to me. Thanks!

---

_Label `bug` added by @BurntSushi on 2019-09-16 11:42_

---

_Comment by @jakubadamw on 2019-09-22 09:58_

@BurntSushi, I was looking at what it would take to sensibly fix this and have come up with a rather invasive change that would involve making the `matched` family of methods in grep-searcher's `Sink` and `Core` return an enum of `{Quit, MatchAndContext, ContextOnly}` (naming to be bikeshedded as needed) instead of a `bool`.

https://github.com/BurntSushi/ripgrep/blob/8cb7271b647f63420530ac04bc13ed8ea7353690/grep-searcher/src/sink.rs#L121-L125

Does that sound reasonable?

---

_Comment by @BurntSushi on 2019-09-22 12:50_

I haven't investigated this yet, so I'm not sure if it's reasonable or not. I'd certainly prefer to avoid breaking changes, and in particular, avoid complicating the API like this. But as I said, I haven't dug into this so I'm not sure.

---

_Label `rollup` added by @BurntSushi on 2021-05-31 00:03_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
