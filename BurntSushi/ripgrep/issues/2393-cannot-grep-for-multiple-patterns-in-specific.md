```yaml
number: 2393
title: cannot grep for multiple patterns in specific file types
type: issue
state: closed
author: huynle
labels:
  - invalid
assignees: []
created_at: 2023-01-06T11:53:12Z
updated_at: 2023-11-22T21:56:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2393
synced_at: 2026-01-12T16:13:24Z
```

# cannot grep for multiple patterns in specific file types

---

_@huynle_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

both `sudo dfn install ripgrep` and downloading the binary release from github and using its binary

#### What operating system are you using ripgrep on?

Fedora 37

#### Describe your bug.

I am not able to grep for multiple expression with associated filetypes, e.g.
`rg -e pattern1 -t <filetype1> -t <filetype2> -e pattern2 -t <filetype3> -t <filetype4> <other options>`

#### What are the steps to reproduce the behavior?

a simple search would do, something like  `rg -e foo -t py -t js -e bar -t cc -t cpp`

#### What is the actual behavior?

results are showing for `foo` in filetype `cpp`

#### What is the expected behavior?

the expected outcome is pattern1 should only match to <filetype1> and <filetype2>, but it seems like all the specified filetypes are searched for, and I am getting results for pattern1 in <filetype4>


---

_Comment by @BurntSushi on 2023-01-06 12:17_

Could you please point me to the documentation that has led you to believe that this will behave in the way you've described?

---

_Comment by @BurntSushi on 2023-11-22 21:56_

Closing due to inactivity. And the lack of reproduction here prevents me from understanding exactly what the problem is here.

---

_Closed by @BurntSushi on 2023-11-22 21:56_

---

_Label `invalid` added by @BurntSushi on 2023-11-22 21:56_

---
