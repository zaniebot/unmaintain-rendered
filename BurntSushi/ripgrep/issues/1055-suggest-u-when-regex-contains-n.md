```yaml
number: 1055
title: Suggest -U when regex contains \n
type: issue
state: closed
author: dtolnay
labels:
  - enhancement
assignees: []
created_at: 2018-09-15T16:59:16Z
updated_at: 2018-09-25T20:56:05Z
url: https://github.com/BurntSushi/ripgrep/issues/1055
synced_at: 2026-01-12T16:13:22Z
```

# Suggest -U when regex contains \n

---

_@dtolnay_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

cargo install

#### Describe your question, feature request, or bug.

I tried this regex today: `rg '=> \{\n *break;'`. The output is:

```console
the literal '"\n"' is not allowed in a regex
```

I would expect this error to suggest passing `-U` to enable multiline mode, in which that pattern would be allowed.

---

_Comment by @BurntSushi on 2018-09-15 17:12_

This seems like a good idea! We already do the same with the `-P` flag if look-around or backreferences are used.

---

_Label `enhancement` added by @BurntSushi on 2018-09-15 17:12_

---

_Closed by @BurntSushi on 2018-09-25 20:56_

---
