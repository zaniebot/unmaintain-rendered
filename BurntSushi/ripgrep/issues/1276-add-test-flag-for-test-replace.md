```yaml
number: 1276
title: add --test flag for test replace
type: issue
state: closed
author: woodgear
labels:
  - invalid
assignees: []
created_at: 2019-05-09T02:22:45Z
updated_at: 2019-05-09T10:42:53Z
url: https://github.com/BurntSushi/ripgrep/issues/1276
synced_at: 2026-01-12T16:13:23Z
```

# add --test flag for test replace

---

_@woodgear_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
#### How did you install ripgrep?

cargo 
#### What operating system are you using ripgrep on?
windows

#### Describe your question, feature request, or bug.
 feature request
add ability to test replace.
i want to check the replace result and then replace it in real.
for example 
rg fast README.md -ort FAST
if you add flag -t which will not replace in real but for test

---

_Closed by @woodgear on 2019-05-09 02:35_

---

_Label `invalid` added by @BurntSushi on 2019-05-09 10:42_

---

_Comment by @BurntSushi on 2019-05-09 10:42_

ripgrep doesn't execute the replacement in files. Indeed, ripgrep will never write to any file. All it does is read and search.

---
