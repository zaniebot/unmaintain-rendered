```yaml
number: 1923
title: Supports excluding only a specific ignore file
type: issue
state: closed
author: maple3142
labels:
  - wontfix
assignees: []
created_at: 2021-07-01T18:41:04Z
updated_at: 2021-07-01T19:35:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1923
synced_at: 2026-01-12T16:13:24Z
```

# Supports excluding only a specific ignore file

---

_@maple3142_

#### Describe your feature request

My problem is that I use my home directory as a git repo to manage my dotfiles, so there is a `~/.gitignore` that ignores everything except some files. The problem is `rg` won't work by default in this situation. I use `--no-ignore-parent` currently as most of my files are organized into subdirectories, but it still make `rg` not useful in home directory.

I wish ripgrep could have a flag like `--no-ignore-file` to only ignore a specific ignore file. For example, `rg --no-ignore-file ~/.gitignore` only ignores `~/.gitignore` but still respect other `.gitignore` files in a repo.

Similar issue: https://github.com/BurntSushi/ripgrep/issues/372


---

_Comment by @BurntSushi on 2021-07-01 18:51_

Can you say why none of the work-arounds in #372 work for you?

Also, FWIW, a while ago I bit the bullet and just enumerated everything I didn't want in my `~/.gitignore`: https://github.com/BurntSushi/dotfiles/blob/master/.gitignore. Not necessarily because of ripgrep, but because I found myself with files that I thought I had added to the source control, but didn't actually, because they wouldn't show up in `git status`.

---

_Label `question` added by @BurntSushi on 2021-07-01 18:54_

---

_Comment by @maple3142 on 2021-07-01 19:03_

> Can you say why none of the work-arounds in #372 work for you?

My `~/.gitignore` is like this:

```gitignore
*
!.gitignore
!.zshrc
# others
```

And my home directory have folders like `projects`, `workspace`...

My current solution is aliasing `rg` to `rg --no-ignore-parent`, so that `rg [PATTERN]` just works if I am in `projects` or `workspace`. The downside is `rg [PATTERN]` still won't work in home directory.

If `rg` could have a `--no-ignore-file`, aliasing `rg` to `rg --no-ignore-file ~/.gitignore` solves this problem perfectly.

> Also, FWIW, a while ago I bit the bullet and just enumerated everything I didn't want in my `~/.gitignore`: [https://github.com/BurntSushi/dotfiles/blob/master/.gitignore](https://github.com/BurntSushi/dotfiles/blob/master/.gitignore?rgh-link-date=2021-07-01T18%3A51%3A53Z). Not necessarily because of ripgrep, but because I found myself with files that I thought I had added to the source control, but didn't actually, because they wouldn't show up in `git status`.

It looks like it is not really different from what I did: exclude everything then include only what I want


---

_Comment by @BurntSushi on 2021-07-01 19:05_

You didn't address any of the work-arounds in #372, and my `.gitignore` is _not at all_ like yours. It has zero `!` rules, for example.

---

_Comment by @maple3142 on 2021-07-01 19:25_

A problem with `rg [PATTERN] *` is that it also follows a symlink automatically. Since I am using WSL2, I have a symlink `winhome` to my Windows home directory. But I/O performance across systems is pretty bad, so it should be avoided.
(It really follows symlink even if I didn't use `--follow`, not sure if it is correct behavior.)

Adding `![!.]*` to `~/.ignore` works, but isn't it more simpler to just ignore a specific ignore file?

As for `--no-ignore-parent`, I did address this work-around in previous comment.

---

_Comment by @BurntSushi on 2021-07-01 19:35_

> Adding ![!.]* to ~/.ignore works, but isn't it more simpler to just ignore a specific ignore file?

I don't think so. There are already way too many ignore related flags. Everyone wants one for their own use case. It's likely a UX failure on my part, but the answer can't be to just keep adding more flags.

Since using a `~/.ignore` file works for you, then I'm going to close this. Indeed, one of the main purposes of .ignore files is to override .gitignore when there is an impedance mismatch between what's tracked by git and what you want to search.

---

_Closed by @BurntSushi on 2021-07-01 19:35_

---

_Label `question` removed by @BurntSushi on 2021-07-01 19:35_

---

_Label `wontfix` added by @BurntSushi on 2021-07-01 19:35_

---
