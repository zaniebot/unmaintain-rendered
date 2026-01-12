```yaml
number: 1533
title: Ignore a specific .gitignore
type: issue
state: closed
author: albertorestifo
labels:
  - duplicate
assignees: []
created_at: 2020-03-28T13:39:40Z
updated_at: 2020-03-28T13:43:47Z
url: https://github.com/BurntSushi/ripgrep/issues/1533
synced_at: 2026-01-12T16:13:23Z
```

# Ignore a specific .gitignore

---

_@albertorestifo_

#### What version of ripgrep are you using?

```
ripgrep 12.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

AUR

#### What operating system are you using ripgrep on?

Manjaro Linux

#### Describe your question

I recently started managing my dotfiles as [described in this blog post](https://drewdevault.com/2019/12/30/dotfiles.html). This means that I now have a `$HOME/.gitignore` file with the content:

```
*
```

As a consequence, ripgrep is now ignoring all files, regardless of the folder I'm in.

Reading trough the manual, I found that I could work around this issue by specifying the flag `--no-ignore-parent`, but this solution compromises on the usability inside nested folders.

Is there a way to tell rg to ignore specifically a `.gitignore` file, in my case located at `$HOME/.gitignore`?


---

_Comment by @BurntSushi on 2020-03-28 13:43_

Duplicate of #982.

Also related: #1494. 

---

_Closed by @BurntSushi on 2020-03-28 13:43_

---

_Label `duplicate` added by @BurntSushi on 2020-03-28 13:43_

---
