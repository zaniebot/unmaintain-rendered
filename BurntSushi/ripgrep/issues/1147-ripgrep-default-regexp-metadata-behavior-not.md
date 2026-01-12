```yaml
number: 1147
title: "ripgrep default `|` regexp metadata behavior not compitable with gnu grep"
type: issue
state: closed
author: zw963
labels:
  - invalid
assignees: []
created_at: 2018-12-21T02:38:36Z
updated_at: 2018-12-26T14:25:25Z
url: https://github.com/BurntSushi/ripgrep/issues/1147
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep default `|` regexp metadata behavior not compitable with gnu grep

---

_@zw963_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

download compiled version.

#### What operating system are you using ripgrep on?

Deepin linux

#### Describe your question, feature request, or bug.

When want to replace default grep with ripgrep, i found some not compatible behavior.

default grep:

```sh
 ╰─ $ 1  echo 'hello world' |grep 'hello\|world'
hello world       # => this is expected behavior for grep
```

ripgrep 

```sh
╰─ $ echo 'hello world' |rg 'hello\|world'  # => not output
 ╰─ $ echo 'hello world' |rg 'hello|world'
hello world       # => must replace  \| to |, to resolve this.
```

i guess ripgrep default behavior same with grep `-E`, but i think
this maybe not a good idea, anywhy,  we have `-P` to perl regex.

I don't know this is a bug or just expected behavior, Thanks


---

_Comment by @BurntSushi on 2018-12-21 02:45_

This is expected behavior because ripgrep does not support BREs. Please see https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#posix4ever

---

_Closed by @BurntSushi on 2018-12-21 02:45_

---

_Label `invalid` added by @BurntSushi on 2018-12-26 14:25_

---
