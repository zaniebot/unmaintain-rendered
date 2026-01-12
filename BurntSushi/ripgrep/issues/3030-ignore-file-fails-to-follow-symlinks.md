```yaml
number: 3030
title: "--ignore-file fails to follow symlinks"
type: issue
state: closed
author: ssbarnea
labels:
  - question
assignees: []
created_at: 2025-04-11T13:44:13Z
updated_at: 2025-10-11T01:11:44Z
url: https://github.com/BurntSushi/ripgrep/issues/3030
synced_at: 2026-01-12T16:13:25Z
```

# --ignore-file fails to follow symlinks

---

_@ssbarnea_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

features:+pcre2
simd(compile):+NEON
simd(runtime):+NEON

### How did you install ripgrep?

brew

### What operating system are you using ripgrep on?

macos

### Describe your bug.

It seems that `--ignore-file=valid-symlink-to-file` generates a runtime error instead of loading the symlinked file:
```
rg: tags:: IO error for operation on tags:: No such file or directory (os error 2)
```

This is problematic for users trying to use dotfiles with something like [stow](https://www.gnu.org/software/stow/) which uses symlinks to track config files with git. ripgrep was the first tool I found that seems to not be symlink friendly

### What are the steps to reproduce the behavior?

Create symlink to the ignore file and pass it via --ignore-file

### What is the actual behavior?

```
rg: tags:: IO error for operation on tags:: No such file or directory (os error 2)
```

### What is the expected behavior?

Read symlinks ignore files if they resolve and exit with error if they do not.

---

_Comment by @BurntSushi on 2025-04-11 13:55_

Please provide a real MRE. I cannot reproduce based on your prose:

```
$ mkdir -p /tmp/rg3030
$ cd /tmp/rg3030
$ touch foo bar quux
$ echo 'bar' > tags
$ rg --files
foo
bar
quux
tags
$ rg --files --ignore-file tags
foo
quux
tags
$ ln -s tags tags-link
$ rg --files
foo
bar
quux
tags
$ rg --files --ignore-file tags-link
foo
quux
tags
```

---

_Label `question` added by @BurntSushi on 2025-04-11 13:55_

---

_Closed by @BurntSushi on 2025-10-11 01:11_

---
