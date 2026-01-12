```yaml
number: 1369
title: Prevent duplicate entries in file types list (--type-list)
type: issue
state: closed
author: StaticPH
labels: []
assignees: []
created_at: 2019-09-07T23:11:38Z
updated_at: 2019-09-08T00:09:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1369
synced_at: 2026-01-12T16:13:23Z
```

# Prevent duplicate entries in file types list (--type-list)

---

_@StaticPH_

#### What version of ripgrep are you using?

```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Installed with cargo

#### What operating system are you using ripgrep on?

Windows 8.1 Pro (x86_64)
If it's relevant, I'm also using the latest version of msys2-x86_64

#### Describe your question, feature request, or bug.

I want to avoid having a type with multiple of the same glob, as I don't know what problems that could potentially lead to. That also means not including a glob more than once with `--type-add 'TYPE:include:TYPE'`
I add more than a handful of file type globs to ripgrep, so it becomes kind of a hassle to keep track of which type-glob pairs are already part of the default, and which have already been included into one type from another. 

#### Example of current behavior:
```bash
StaticPH ~ $> rg --type-list | egrep "vi(m|):"
vim: *.vim 
StaticPH ~ $> rg --type-add 'vi: *.vim, *.vi, *.vim' --type-add 'vim:include:vi' --type-list | egrep "vi(m|):"
vi:  *.vim, *.vi, *.vim
vim:  *.vim, *.vi, *.vim, *.vim
```
#### What I'd like to see instead:
```bash
StaticPH ~ $> rg --type-list | egrep "vi(m|):"
vim: *.vim
StaticPH ~ $> rg --type-add 'vim: *.vim, *.vi, *.vim' --type-add 'vim:include:vi' --type-list | egrep "vi(m|):"
vi: *.vim, *.vi
vim: *.vim, *.vi
```

---

_Comment by @BurntSushi on 2019-09-08 00:09_

I don't think ripgrep should be deduplicating these. I'm also not sure what adverse effects can become of this, other than perhaps some small performance overhead. If you don't want duplicates then just don't add them.

---

_Closed by @BurntSushi on 2019-09-08 00:09_

---
