```yaml
number: 1450
title: "suggestion: simplify options"
type: issue
state: closed
author: dicktyr
labels:
  - invalid
assignees: []
created_at: 2019-12-19T22:48:54Z
updated_at: 2019-12-19T23:00:00Z
url: https://github.com/BurntSushi/ripgrep/issues/1450
synced_at: 2026-01-12T16:13:23Z
```

# suggestion: simplify options

---

_@dicktyr_

it would be useful if options `-l` and `-v` worked together
to be effectively identical to `--files-without-match`
and so redefining `-l`: print file name(s) instead of file content(s)

there's one example
but there may be further opportunity to abstract/simplify/reduce options
hence the generic title

in any case
thank you for sharing your software! :}

---

_Comment by @BurntSushi on 2019-12-19 22:59_

Thanks for the thoughts, but this isn't quite right and I would rather not leave non-actionable tickets open. `-lv` in particular is _not_ the same as `--files-without-match`. The former shows any file that contains at least one non-matching line, where as the latter shows any file that contains zero matching lines.

---

_Closed by @BurntSushi on 2019-12-19 22:59_

---

_Label `invalid` added by @BurntSushi on 2019-12-19 23:00_

---
