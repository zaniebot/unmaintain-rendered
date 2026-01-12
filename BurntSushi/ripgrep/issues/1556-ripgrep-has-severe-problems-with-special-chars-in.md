```yaml
number: 1556
title: Ripgrep has severe problems with special chars (in this case umlauts)
type: issue
state: closed
author: nickbe
labels:
  - invalid
assignees: []
created_at: 2020-04-20T13:38:47Z
updated_at: 2020-04-20T13:46:28Z
url: https://github.com/BurntSushi/ripgrep/issues/1556
synced_at: 2026-01-12T16:13:23Z
```

# Ripgrep has severe problems with special chars (in this case umlauts)

---

_@nickbe_

ripgrep 12.0.0 (rev b9cd95faf1)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

Binary release
Windows 10

Even when simply searching a string which is only in the same line as an umlaut,
rg does not match the result. For example: Text line is "Lünen" - I'm searching for "nen". Result comes back empty though. 

Actually ripgrep would have to be at least able to cope with lines that contains special characters like umlauts, if not being able to search for them.


---

_Comment by @BurntSushi on 2020-04-20 13:46_

Closing, because I cannot reproduce:

```
$ echo 'Lünen' | rg 'nen'
Lünen
```

Assuming you're experiencing an issue, please consider providing more details. When opening a new bug report, there should be a template with a set of questions for you to answer. It's there for a reason. I'm not sure why you ignored it.

---

_Closed by @BurntSushi on 2020-04-20 13:46_

---

_Label `invalid` added by @BurntSushi on 2020-04-20 13:46_

---
