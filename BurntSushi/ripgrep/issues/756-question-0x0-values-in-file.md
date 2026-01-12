```yaml
number: 756
title: Question - 0x0 values in file
type: issue
state: closed
author: stej
labels: []
assignees: []
created_at: 2018-01-24T13:04:06Z
updated_at: 2018-01-24T13:37:46Z
url: https://github.com/BurntSushi/ripgrep/issues/756
synced_at: 2026-01-12T16:13:22Z
```

# Question - 0x0 values in file

---

_@stej_

I have a question about handling files with 0x0 values inside. 
Some tools log these values and that causes that rg doesn't parse the files correctly.

Reproduction:
* Unzip test file.
  Te test file looks like this
  ![image](https://user-images.githubusercontent.com/253560/35333468-2ad1da52-010f-11e8-9f8c-2da4e540c7cf.png)
* `rg -g *testfile* "."` -- doesn't find anything.

* if the 0x0 character is removed, if works as expected.

Version:
```
>rg --version
ripgrep 0.7.1
-AVX -SIMD
```
[testfile.zip](https://github.com/BurntSushi/ripgrep/files/1659889/testfile.zip)

**Questions:**
Is that expected behaviour? 
Is there any workaround? 

---

_Comment by @BurntSushi on 2018-01-24 13:11_

A NUL byte trips ripgrep's binary file detection, which causes it to skip over the file. This is the same heuristic that GNU grep uses. The difference is that ripgrep won't say that it's skipping over the file. See #306.

As is standard with these tools, you can use the `-a/--text` flag to override this behavior. Basically, `-a/--text` suppresses the binary file detection.

---

_Comment by @stej on 2018-01-24 13:37_

Thank you a lot for your quick reply!

---

_Closed by @stej on 2018-01-24 13:37_

---
