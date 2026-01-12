```yaml
number: 1004
title: match ignored files with ignore crate
type: issue
state: closed
author: DD5HT
labels:
  - question
assignees: []
created_at: 2018-08-06T13:48:59Z
updated_at: 2018-08-19T14:00:37Z
url: https://github.com/BurntSushi/ripgrep/issues/1004
synced_at: 2026-01-12T16:13:22Z
```

# match ignored files with ignore crate

---

_@DD5HT_

#### Describe your question, feature request, or bug.

Is there a way to reverse match the files with the **ignore crate** so it only returns all the files which are in my .ignore file?

Maybe even add a `.reverse()` method to the `WalkBuilder` ?

---

_Comment by @BurntSushi on 2018-08-06 13:52_

No, there is no way to do this. `ignore` files are intended for specifying things to ignore, with the possibility of whitelisting things using `!`.

I'm not particularly inclined to add this feature either. The [semantics of how ignore patterns are handled are complex](https://docs.rs/ignore/0.4.3/ignore/struct.WalkBuilder.html#ignore-rules), and adding a "reverse" feature will require rethinking and testing all of those cases. It's a fairly big feature and will probably have a fairly significant maintenance burden as well.

What problem are you trying to solve with this?

---

_Comment by @DD5HT on 2018-08-06 14:04_

I want to easily take all entries out of the ignore file and delete/modify these files. 

---

_Comment by @BurntSushi on 2018-08-06 14:17_

Oh, well that's easy:

```
$ comm -13 <(rg --files | sort -u) <(rg --files -u | sort -u)
```

---

_Label `question` added by @BurntSushi on 2018-08-06 14:17_

---

_Closed by @BurntSushi on 2018-08-19 14:00_

---
