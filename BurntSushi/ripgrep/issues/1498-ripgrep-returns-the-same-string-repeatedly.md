```yaml
number: 1498
title: Ripgrep returns the same string repeatedly
type: issue
state: closed
author: mishari
labels:
  - invalid
assignees: []
created_at: 2020-02-25T08:13:44Z
updated_at: 2020-02-25T10:37:49Z
url: https://github.com/BurntSushi/ripgrep/issues/1498
synced_at: 2026-01-12T16:13:23Z
```

# Ripgrep returns the same string repeatedly

---

_@mishari_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

OSX Mojave 10.14.6 (18G3020)

#### Describe your question, feature request, or bug.

Ripgrep returns the string used for querying as the result repeatedly, even tho the file doesn't contain such string.

#### If this is a bug, what are the steps to reproduce the behavior?

Take the file from: https://gist.githubusercontent.com/mishari/85a73923ce42410a06a6ca22d529e083/raw/9639d6f5aa87e94eb416ad272efb005d5541da3a/f3430896.txt put in directory by itself
run:
rg -r Blah .

#### If this is a bug, what is the actual behavior?
rg Blah .

```
recup_dir.1/f3430896.txt:BlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlah
recup_dir.1/f3430896.txt:BlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlah
recup_dir.1/f3430896.txt:BlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlahBlah
```

If I do:

rg Blah recup_dir.1/f3430896.txt

nothing returns as it should be, neither if I do

rg -r -e Blah .
or
rg -r --debug Blah .


---

_Closed by @mishari on 2020-02-25 08:19_

---

_Comment by @mishari on 2020-02-25 08:20_

My bad, I thought -r was recursive. Closing.

Best Regards
Mishari

---

_Label `invalid` added by @BurntSushi on 2020-02-25 10:37_

---

_Renamed from "Ripgrep" to "Ripgrep returns the same string repeatedly" by @BurntSushi on 2020-02-25 10:37_

---
