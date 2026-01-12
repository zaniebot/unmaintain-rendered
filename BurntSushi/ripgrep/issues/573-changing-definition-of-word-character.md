```yaml
number: 573
title: Changing definition of word character
type: issue
state: closed
author: pesterhazy
labels:
  - question
assignees: []
created_at: 2017-08-09T09:50:58Z
updated_at: 2017-08-09T11:31:43Z
url: https://github.com/BurntSushi/ripgrep/issues/573
synced_at: 2026-01-12T16:13:22Z
```

# Changing definition of word character

---

_@pesterhazy_

Is it possible to change what is considered a _word character_ when using `-w`? Take the lisp/clojure definition of symbols for example, which allows `-` as part of a function name. Say I want to search for `time` but exclude `start-time`, `time-period`. That is, I want to classify `-` as a word character.

`(?:^|[^\w-])time(?:[^\w-]|$)` seems to do the job. But is there a better way?



---

_Comment by @okdana on 2017-08-09 10:03_

The `-w` option just surrounds the pattern with `\b`s, which is hard-coded behaviour. There is some discussion of possibly changing it to more closely replicate the way `grep` works, but that wouldn't help you here either. So i can't think of a better option than the pattern you mentioned

---

_Comment by @BurntSushi on 2017-08-09 11:31_

@okdana Has it right. The definition of `\b` is set in stone.

---

_Closed by @BurntSushi on 2017-08-09 11:31_

---

_Label `question` added by @BurntSushi on 2017-08-09 11:31_

---
