```yaml
number: 128
title: On pathological files, -n can give the wrong line numbers
type: issue
state: closed
author: zdanz
labels:
  - bug
assignees: []
created_at: 2016-09-28T17:38:10Z
updated_at: 2016-11-06T02:29:50Z
url: https://github.com/BurntSushi/ripgrep/issues/128
synced_at: 2026-01-12T18:23:11Z
```

# On pathological files, -n can give the wrong line numbers

---

_@zdanz_

The attached contains a text file with 16 characters, including 4 CRLF's, before an 'x'; and a modification where the 4 ^M's were changed into ^K's.  You can see that some of the ^K's were counted as extra LF's (using rg.exe from ripgrep-0.2.1-x86_64-pc-windows-gnu.zip):

```
>\Utils\grep\rg.exe -n x really_text.txt not_really_text.txt
not_really_text.txt
8:x

really_text.txt
5:x
```

I believe this problem can only happen on files with ^K in them.  There's a fix for a similar issue in the bytecount crate: https://github.com/llogiq/bytecount/commit/52b177d229533adb25b42b0563aff15333df7e66.  See also the discussion in https://github.com/google/xi-editor/pull/122 -- where I was unaware of the existing bytecount fix. :-)

[eol_counting.zip](https://github.com/BurntSushi/ripgrep/files/498835/eol_counting.zip)


---

_Comment by @BurntSushi on 2016-09-28 17:43_

@zdanz I was secretly hoping you'd trace this issue all the way through ripgrep. Excellent find. :-)

I am going to try hard to dig into the byte counting stuff this weekend and make sure we all have it right. I know @llogiq has written a couple blog posts that I need to digest. :-)


---

_Label `bug` added by @BurntSushi on 2016-09-29 01:12_

---

_Comment by @llogiq on 2016-09-29 05:22_

Sorry, but my blog doesn't touch the bug, you may however read Veedrac's issue on bytecount for a thorough explanation.

Bytecount has fixed the problem in the meantime, btw.


---

_Closed by @BurntSushi on 2016-11-06 02:29_

---
