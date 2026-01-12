```yaml
number: 1386
title: Limit the characters included in the search result
type: issue
state: closed
author: thatreguy
labels:
  - invalid
assignees: []
created_at: 2019-09-24T06:54:50Z
updated_at: 2019-09-24T10:22:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1386
synced_at: 2026-01-12T16:13:23Z
```

# Limit the characters included in the search result

---

_@thatreguy_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (rev 8cb7271b64)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

snap

#### What operating system are you using ripgrep on?

Ubuntu 18.04.3 LTS

#### Describe your question, feature request, or bug.

Limit the output from a single line. A base64 encoded line outputs a huge text exhausting the terminal scrollback buffer.

---

_Comment by @okdana on 2019-09-24 10:14_

That is what `-M` (optionally combined with `--max-columns-preview`) does

(edit: mistyped the latter)

---

_Closed by @BurntSushi on 2019-09-24 10:22_

---

_Label `invalid` added by @BurntSushi on 2019-09-24 10:22_

---
