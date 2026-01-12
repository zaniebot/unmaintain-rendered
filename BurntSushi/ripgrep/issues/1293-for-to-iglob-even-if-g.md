```yaml
number: 1293
title: For to --iglob even if -g
type: issue
state: closed
author: sumonto
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2019-06-05T22:26:35Z
updated_at: 2019-08-01T21:28:27Z
url: https://github.com/BurntSushi/ripgrep/issues/1293
synced_at: 2026-01-12T16:13:23Z
```

# For to --iglob even if -g

---

_@sumonto_

#### What version of ripgrep are you using?

ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


#### How did you install ripgrep?

brew install

#### What operating system are you using ripgrep on?

Mac OSx lastest

#### Describe your question, feature request, or bug.

How do I force --iglob for all my searches even if I specify -g ..
I would like to avoid having to write a custom function/wrapper

Thanks

---

_Comment by @BurntSushi on 2019-06-05 22:46_

You can't. I suppose we could add a flag, `--glob-case-insensitive`, that is like `--ignore-file-case-insensitive`, and then you could put that in an alias or a config file.

---

_Label `enhancement` added by @BurntSushi on 2019-06-05 22:47_

---

_Comment by @sumonto on 2019-06-06 17:05_

can you please add --ignore-file-case-insensitive, so that I can put the same in the config file
appreciate your help

---

_Comment by @BurntSushi on 2019-06-06 17:07_

`--ignore-file-case-insensitive` already exists. Otherwise, I marked this as an `enhancement` and left it open to add `--glob-case-insensitive`.

I've also added a `help wanted` label, since I don't plan on working on this any time soon myself.

---

_Label `help wanted` added by @BurntSushi on 2019-06-06 17:07_

---

_Closed by @BurntSushi on 2019-08-01 21:08_

---

_Comment by @sumonto on 2019-08-01 21:28_

Thanks @BurntSushi @okdana 

---
