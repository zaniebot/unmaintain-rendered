```yaml
number: 372
title: "Home as git repo: troubles with ignore files"
type: issue
state: closed
author: mllg
labels:
  - question
assignees: []
created_at: 2017-02-21T09:54:21Z
updated_at: 2017-02-21T12:38:42Z
url: https://github.com/BurntSushi/ripgrep/issues/372
synced_at: 2026-01-12T16:13:21Z
```

# Home as git repo: troubles with ignore files

---

_@mllg_

I've set up my $HOME as git repository to manage my dotfiles. Thus, I ignore all files via `~/.gitignore` and force-add the config files I want to track with git. 

This now causes some troubles:

1. If I am in a project directory with its own gitignore/ignore files, I obviously want to use them, so I just call `rg [pattern]` which then works nicely out of the box.
2. If I am in a directory without any ignore files, the parent ignore file in $HOME is parsed and I have to use `rg -u [pattern]` to avoid this. If rg now descents down into subdirectories with ignore files, these are also not parsed.
3. If I un-ignore all files via `!*` in `~/.ignore`, all files are searched, including hidden files (which I can filter out by ignoring them again with `.*` in `~/.ignore`) and binary files (which I can not filter out).

I was hoping to find a way to have `rg` act identical in all directories, i.e. the same way as `~/.gitignore` would not exist. I've tried adding `^.gitignore` into `~/.ignore` but this did not help.

Is there an option I've overlooked to achieve this behavior?

---

_Comment by @BurntSushi on 2017-02-21 11:55_

> If I am in a directory without any ignore files, the parent ignore file in $HOME is parsed and I have to use rg -u [pattern] to avoid this. If rg now descents down into subdirectories with ignore files, these are also not parsed.

Yup, this is intended behavior. This isn't something that ripgrep imposes. These are the semantics of `gitignore` matching as described in `man gitignore`.

A very simple work-around is to run `rg [pattern] *`, which will override your `$HOME/.gitignore` because the paths are explicitly provided.

> If I un-ignore all files via `!*` in ~/.ignore, all files are searched, including hidden files (which I can filter out by ignoring them again with `.*` in ~/.ignore) and binary files (which I can not filter out).

You might try something like `![!.]*` in your `$HOME/.ignore` to whitelist all non-hidden files, although I suppose this large achieves the same thing as adding `!*` and `.*`.

Binary files should be automatically filtered out even if they aren't in your `.gitignore`. Are you seeing something different?

> I was hoping to find a way to have rg act identical in all directories, i.e. the same way as ~/.gitignore would not exist. I've tried adding ^.gitignore into ~/.ignore but this did not help.

You could try using `--no-ignore-parent`, which specifically won't acknowledge `.gitignore` files in parent directories. The option isn't exactly short, so you'd probably want to create an alias of some sort.

FWIW, I also have a similar setup as you. I don't often search in my `$HOME` outside of other git directories, but when I do, I just use `rg [pattern] *`.

---

_Label `question` added by @BurntSushi on 2017-02-21 11:55_

---

_Comment by @mllg on 2017-02-21 12:32_

> A very simple work-around is to run rg [pattern] *, which will override your $HOME/.gitignore because the paths are explicitly provided.

This is probably what I was looking for! :smile:

> Binary files should be automatically filtered out even if they aren't in your .gitignore. Are you seeing something different?

You' re right, I should have read the output more carefully. I got tons of additional output complaining about illegal `.gitignore` files ("invalid use of **; must be one path component") which were in a large repository in a subdir.


> You could try using --no-ignore-parent, which specifically won't acknowledge .gitignore files in parent directories. The option isn't exactly short, so you'd probably want to create an alias of some sort.
> 
> FWIW, I also have a similar setup as you. I don't often search in my $HOME outside of other git directories, but when I do, I just use rg [pattern] *.

Works perfectly for me. Thanks for your answer and thanks for ripgrep!

---

_Closed by @mllg on 2017-02-21 12:32_

---

_Comment by @BurntSushi on 2017-02-21 12:38_

> I got tons of additional output complaining about illegal .gitignore files ("invalid use of **; must be one path component") which were in a large repository in a subdir.

Ah hmm. Unless there's a bug in ripgrep, that means you have some incorrect rules in your `.gitignore`. :-) I guess `git` is probably just not telling you.

Glad it all worked out!

---
