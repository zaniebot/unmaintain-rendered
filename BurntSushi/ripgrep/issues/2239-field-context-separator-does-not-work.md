```yaml
number: 2239
title: "--field-context-separator does not work"
type: issue
state: closed
author: 95440b97d
labels: []
assignees: []
created_at: 2022-06-19T06:36:34Z
updated_at: 2022-06-19T19:46:10Z
url: https://github.com/BurntSushi/ripgrep/issues/2239
synced_at: 2026-01-12T16:13:24Z
```

# --field-context-separator does not work

---

_@95440b97d_

#### What version of ripgrep are you using?
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
pacman -S ripgrep

#### What operating system are you using ripgrep on?
Arch Linux

#### Describe your bug.
seq 3 | rg -n --field-context-separator=% 2
2:2

Option --field-context-separator is ignored.  The expected output of the above program is '2%2'.


---

_Comment by @BurntSushi on 2022-06-19 18:37_

From the man page, emphasis mine:

> Set the field context separator, which is used to delimit file paths, line numbers, columns and the context itself, **when printing contextual lines.**

There are zero contextual lines in your example, so it's unclear to me how you're concluding that `--field-context-separator` does not work.

Here's an example that demonstrates it working as intended:

```
$ seq 3 | rg -n --field-context-separator=% 2 -C1
1%1
2:2
3%3
```

I suspect this example may also be useful to you:

```
$ seq 3 | rg -n --field-context-separator=% --field-match-separator=# 2 -C1
1%1
2#2
3%3
```

---

_Comment by @95440b97d on 2022-06-19 19:46_

Yes I see. The mistake was on my side. Sorry for the noise we can close this.

---

_Closed by @95440b97d on 2022-06-19 19:46_

---
