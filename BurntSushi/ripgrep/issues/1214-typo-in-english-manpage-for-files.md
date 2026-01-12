```yaml
number: 1214
title: Typo in English manpage for --files
type: issue
state: closed
author: warmsocks
labels:
  - doc
assignees: []
created_at: 2019-03-08T18:58:53Z
updated_at: 2019-03-08T19:04:57Z
url: https://github.com/BurntSushi/ripgrep/issues/1214
synced_at: 2026-01-12T16:13:23Z
```

# Typo in English manpage for --files

---

_@warmsocks_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Via yum from repo carlwgeorge-ripgrep

#### What operating system are you using ripgrep on?

CentOS 7.6

#### Describe your question, feature request, or bug.

The text in the --files section is grammatically incorrect.

#### If this is a bug, what are the steps to reproduce the behavior?

From a shell, do

man rg

#### If this is a bug, what is the actual behavior?

man rg

```
       --files
           Print each file that would be searched without actually performing the search. This is useful to determine whether a particular file is
           being search or not.

```

#### If this is a bug, what is the expected behavior?

The output text should be

```
       --files
           Print each file that would be searched without actually performing the search. This is useful to determine whether a particular file is
           being searched or not.

```



---

_Comment by @BurntSushi on 2019-03-08 19:04_

Thanks, I'm not inclined to clutter up the issue tracker with minor typos or grammatical errors. It's probably less work to just submit a PR. 

---

_Closed by @BurntSushi on 2019-03-08 19:04_

---

_Label `doc` added by @BurntSushi on 2019-03-08 19:04_

---
