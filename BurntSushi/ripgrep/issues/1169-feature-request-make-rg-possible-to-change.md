```yaml
number: 1169
title: "Feature request: Make rg possible to change delimiter from colon"
type: issue
state: closed
author: yantene
labels:
  - question
assignees: []
created_at: 2019-01-20T14:42:27Z
updated_at: 2019-01-20T16:01:25Z
url: https://github.com/BurntSushi/ripgrep/issues/1169
synced_at: 2026-01-12T16:13:23Z
```

# Feature request: Make rg possible to change delimiter from colon

---

_@yantene_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

I install ripgrep from the arch official repos.

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your question, feature request, or bug.

When I execute the following command, the file path, line number, contents are displayed in a colon delimiter.

```
$ rg --no-heading --line-number .
test.txt:1:hogehoge
test.txt:2:fugafuga
test.txt:3:piyopiyo
```

I'd like to change the delimiter to anything.

Replacing by `sed` will fail if the file path contains some colons.
So I want to specify an other delimiter character in ripgrep.

Thank you.


---

_Comment by @BurntSushi on 2019-01-20 15:38_

> Replacing by sed will fail if the file path contains some colons.
So I want to specify an other delimiter character in ripgrep.

This is basically what the `-0/--null` flag is for, because the NUL byte is the only byte that is illegal in a file path on Unix.

Otherwise, if you need a more structured format for parsing, you might consider using `--json`.

---

_Closed by @BurntSushi on 2019-01-20 15:38_

---

_Label `question` added by @BurntSushi on 2019-01-20 15:38_

---

_Comment by @yantene on 2019-01-20 16:01_

`-0/--null` is exactly what I needed!
Thank you very much!

---
