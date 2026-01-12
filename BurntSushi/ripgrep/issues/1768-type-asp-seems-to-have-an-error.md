```yaml
number: 1768
title: type asp seems to have an error
type: issue
state: closed
author: NetMage
labels: []
assignees: []
created_at: 2020-12-23T19:45:13Z
updated_at: 2020-12-24T11:28:37Z
url: https://github.com/BurntSushi/ripgrep/issues/1768
synced_at: 2026-01-12T16:13:24Z
```

# type asp seems to have an error

---

_@NetMage_

#### What version of ripgrep are you using?

ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Using Scoop

#### What operating system are you using ripgrep on?

Windows 10 v1909

#### Describe your bug.

In the output of  rg --type-list, the file listing for asp has a duplicate *.aspx.cs - was the second one intended to be *.aspx.vb?

#### What are the steps to reproduce the behavior?

rg --type-list

#### What is the actual behavior?
...
```
asp: *.ascx, *.ascx.cs, *.ascx.vb, *.aspx, *.aspx.cs, *.aspx.cs
```
...

#### What is the expected behavior?

```
asp: *.ascx, *.ascx.cs, *.ascx.vb, *.aspx, *.aspx.cs, *.aspx.vb
```


---

_Comment by @BurntSushi on 2020-12-23 19:54_

I don't use ASP. I rely on contributors to maintain the file type list. If there's an error, please submit a PR with links to any relevant supporting material. Thanks.

---

_Closed by @BurntSushi on 2020-12-23 19:54_

---

_Comment by @NetMage on 2020-12-24 04:30_

Really, two duplicate entries aren't enough indication there is an error?

---

_Comment by @BurntSushi on 2020-12-24 11:24_

Please file a PR. And reporting a duplicate name is not the  only thing in your issue. I'm not questioning the existence of an error. 

---
