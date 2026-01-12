```yaml
number: 268
title: Raise an error on regex with backreference
type: issue
state: closed
author: anntzer
labels:
  - question
assignees: []
created_at: 2016-12-05T00:09:32Z
updated_at: 2018-03-14T02:55:40Z
url: https://github.com/BurntSushi/ripgrep/issues/268
synced_at: 2026-01-12T16:13:21Z
```

# Raise an error on regex with backreference

---

_@anntzer_

I understand why rg's design fundamentally prevents support for backreferences, but it'd be nice if rg at least printed an error (warning?) message when a regex with a backreference is passed.

---

_Comment by @BurntSushi on 2016-12-05 00:13_

Doesn't it print an error today?

---

_Comment by @anntzer on 2016-12-05 00:29_

`rg '(foo) \1'` prints no error as of 0.3.1 (Arch Linux AUR).

---

_Comment by @BurntSushi on 2016-12-05 01:03_

Ah right. Unfortunately, `\1` is actually an [octal escape sequence](https://doc.rust-lang.org/regex/regex/index.html#escape-sequences), so I don't think that can change at the `regex` level. We could adopt a heuristic that emits a warning message from within ripgrep, but if someone intended to type an octal escape (which seems quite rare), then the warning would be wrong.

---

_Label `question` added by @BurntSushi on 2016-12-05 01:03_

---

_Comment by @anntzer on 2016-12-05 01:04_

My guess is that more people are going to get burnt from trying a backreference, but I don't feel strongly about it either.

---

_Closed by @BurntSushi on 2016-12-12 12:03_

---

_Comment by @BurntSushi on 2017-06-01 11:12_

In light of https://github.com/Microsoft/vscode/issues/27802, I think I should re-open this. I've been thinking a lot about this as I rewrite the regex parser, and I regret that the regex engine supports octal escapes at all. It seems like such an incredibly niche feature that it would be much better to just not support them at all in favor of emitting a better failure mode when someone tries to use backreferences.

With that said, ripgrep could certainly decide not to support octal escapes, even if the underlying regex engine does. The current parser doesn't support this kind of introspection, but my rewrite will. Once that's ready for primetime, I'll disable octal escapes and add a better error message inside ripgrep only.

---

_Reopened by @BurntSushi on 2017-06-01 11:12_

---

_Closed by @BurntSushi on 2018-03-14 02:55_

---
