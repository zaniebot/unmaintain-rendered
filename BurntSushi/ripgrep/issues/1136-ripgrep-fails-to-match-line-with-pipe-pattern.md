```yaml
number: 1136
title: ripgrep fails to match line with pipe pattern
type: issue
state: closed
author: kevinji
labels:
  - duplicate
assignees: []
created_at: 2018-12-07T22:57:38Z
updated_at: 2018-12-07T23:43:14Z
url: https://github.com/BurntSushi/ripgrep/issues/1136
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep fails to match line with pipe pattern

---

_@kevinji_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Probably from the repo.

#### What operating system are you using ripgrep on?

CentOS 7.5

#### Describe your question, feature request, or bug.

rg fails to match this line with a pipe (that `grep` matches).

#### If this is a bug, what are the steps to reproduce the behavior?

1. Create file.txt with `1|t|1234|asdf`
2. Run `rg '^[^|]*\|t\|(?:[^|]*\|)' file.txt`

#### If this is a bug, what is the actual behavior?

No results.

#### If this is a bug, what is the expected behavior?

Should have matched on the `1|t|1234|`.

`grep '^[^|]*\|t\|(?:[^|]*\|)' file.txt` works fine.

Oddly, the following work:
- `rg '^[^|]*\|t\|(?:[^|]*\|?)' file.txt`
- `rg '^[^|]*\|t\|(?:[^|]*)' file.txt`
- `rg '^[^|]*\|t\|[^|]*\|' file.txt`

But this does not:
- `rg '^[^|]*\|t\|(?:[^|]*\|+)' file.txt`

---

_Renamed from "ripgrep fails to match line with pipe" to "ripgrep fails to match line with pipe pattern" by @kevinji on 2018-12-07 22:57_

---

_Comment by @okdana on 2018-12-07 23:07_

I think this is #1064 again, it's fixed in `master`

---

_Comment by @BurntSushi on 2018-12-07 23:43_

Yup, confirmed that this is fixed in master.

---

_Closed by @BurntSushi on 2018-12-07 23:43_

---

_Label `duplicate` added by @BurntSushi on 2018-12-07 23:43_

---
