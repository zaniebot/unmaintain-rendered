```yaml
number: 1072
title: "[feature request] search text inside .docx  "
type: issue
state: closed
author: WiliTest
labels:
  - invalid
  - question
assignees: []
created_at: 2018-09-30T07:28:42Z
updated_at: 2022-07-03T20:08:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1072
synced_at: 2026-01-12T16:13:22Z
```

# [feature request] search text inside .docx  

---

_@WiliTest_

MS word .docx are made to store text on windows. It would be great to be able to search inside these files.

Thanks for your great work! (But I'm still looking for an app that could do that)

---

_Comment by @BurntSushi on 2018-09-30 14:19_

ripgrep is primarily a tool for searching plain text or otherwise UTF-8 encoded data. It is *not* a tool that knows how to decode and read arbitrary files. It will **never** be such a tool.

While ripgrep does have some support for understanding different types of files, such as compressed files, the support is limited and only intended as a low-effort-but-big-gain-in-many-cases kind of feature.

If you have a tool that can convert docx files to plain text, then you should be able to search them pretty easily by using the `--pre` flag.

---

_Closed by @BurntSushi on 2018-09-30 14:19_

---

_Label `invalid` added by @BurntSushi on 2018-09-30 14:19_

---

_Label `question` added by @BurntSushi on 2018-09-30 14:19_

---

_Comment by @WiliTest on 2018-09-30 15:22_

Thanks a lot for your detailed answer!

---

_Comment by @brileyd on 2019-07-11 03:01_

Is the some way to tell rg that a docx file is just a zip?  I attempted to use the -z option, but rg doesn't reconize docx as a zip.

---

_Comment by @BurntSushi on 2019-07-11 09:31_

The `-z` flat only recognizes a fixed set of file extensions. You can either unzip the file yourself and search it, it use the `--pre` flag.

---

_Comment by @ding-dang-do on 2022-07-03 20:08_

Long time ago, but anyway. Maybe someone will find these two pre-processor projects useful.
[RGPipe](https://github.com/ColonelBuendia/rgpipe)
[RipGrep-All](https://github.com/phiresky/ripgrep-all)

---
