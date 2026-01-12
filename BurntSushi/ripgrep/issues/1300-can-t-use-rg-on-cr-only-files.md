```yaml
number: 1300
title: "Can't use rg on CR-only files"
type: issue
state: closed
author: marrub--
labels:
  - enhancement
  - wontfix
assignees: []
created_at: 2019-06-16T10:12:15Z
updated_at: 2019-06-16T12:01:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1300
synced_at: 2026-01-12T16:13:23Z
```

# Can't use rg on CR-only files

---

_@marrub--_

#### What version of ripgrep are you using?

```
% rg --version
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

#### How did you install ripgrep?

Arch Linux's latest official package.

#### What operating system are you using ripgrep on?

```
% uname -a
Linux rain 5.1.8-arch1-1-ARCH #1 SMP PREEMPT Sun Jun 9 20:28:28 UTC 2019 x86_64 GNU/Linux
```

#### Describe your question, feature request, or bug.

This is potentially a feature request and not really a bug. `rg` can't be used with files that use *only* carriage returns (`\r`) as line endings. This isn't normally a problem because only files for [a certain 19 year old operating system](https://en.wikipedia.org/wiki/Mac_OS_9) actually tend to use these, but unfortunately I use ripgrep and also reverse-engineer extremely old games made for said OS. My use case is perhaps less ridiculous than it seems, there are a myriad megabytes of source code I have to search through to find very particular identifiers in and `rg` is a great tool for that.

#### If this is a bug, what are the steps to reproduce the behavior?

Easy:

```
% echo -n abc\rdef > file
% rg abc file
```

#### If this is a bug, what is the actual behavior?

The program outputs the `\r` literally, so it ends up just giving a messy output that doesn't make any sense.

```
% rg abc file
defbc
```

#### If this is a bug, what is the expected behavior?

Perhaps a flag similar to `--crlf`:

```
% rg --cr abc file
1:abc
```

---

_Comment by @BurntSushi on 2019-06-16 12:00_

Sorry, but this isn't going to happen. I'd suggest converting your files to use LF style line endings. Supporting this use case is, IMO, not worth the effort.

---

_Closed by @BurntSushi on 2019-06-16 12:00_

---

_Label `enhancement` added by @BurntSushi on 2019-06-16 12:01_

---

_Label `wontfix` added by @BurntSushi on 2019-06-16 12:01_

---
