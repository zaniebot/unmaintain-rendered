```yaml
number: 1744
title: "Fails to look for \"-1\", \"->\", etc."
type: issue
state: closed
author: westandskif
labels:
  - invalid
assignees: []
created_at: 2020-11-24T14:27:59Z
updated_at: 2020-11-24T14:53:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1744
synced_at: 2026-01-12T16:13:24Z
```

# Fails to look for "-1", "->", etc.

---

_@westandskif_

#### What version of ripgrep are you using?
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
`sudo pacman -S ripgrep`

#### What operating system are you using ripgrep on?
Arch Linux 5.9.10-arch1-1

#### Describe your bug.
rg fails to look for "-1", "->", etc.

this one works:
`rg 'idea' .gitignore` 

this one doesn't:
`rg '-1' .gitignore`
_error: Found argument '-1' which wasn't expected, or isn't valid in this context_

#### What are the steps to reproduce the behavior?
Please see above.

#### What is the actual behavior?
The pattern is considered as an argument.

#### What is the expected behavior?
Looking for a pattern provided.


---

_Comment by @westandskif on 2020-11-24 14:36_

thanks to the colleague of mine, this opt parsing issue is not an rg issue

---

_Closed by @westandskif on 2020-11-24 14:36_

---

_Comment by @BurntSushi on 2020-11-24 14:41_

Right, and indeed, from the man page:

```
       PATTERN
           A regular expression used for searching. To match a pattern beginning with a dash, use the
           -e/--regexp option.
```

---

_Label `invalid` added by @BurntSushi on 2020-11-24 14:41_

---

_Comment by @westandskif on 2020-11-24 14:48_

Thanks! I was having a problem which is being resolved like this:
`rg -F -- '-1'`

---

_Comment by @BurntSushi on 2020-11-24 14:53_

Yes, that also works, because `--` indicates the end of all non-positional arguments. Therefore, anything after `--` is treated as a positional argument. Otherwise, when you use `-x`, there is ambiguity between whether `-x` is a flag or a positional argument.

Also, the `-F` flag is unnecessary in this case. `rg -- -1` is equivalent. As is `rg -e -1`.

These are standard problems in Unix command line tools. The `--` trick is the most common work-around. The `-e` trick is more specific to grep-like tools, but does work with GNU grep as well.

---
