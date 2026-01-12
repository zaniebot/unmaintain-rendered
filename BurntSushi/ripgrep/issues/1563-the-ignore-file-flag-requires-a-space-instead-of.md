```yaml
number: 1563
title: "The `--ignore-file` flag requires a space instead of equals with `~`"
type: issue
state: closed
author: texastoland
labels:
  - invalid
assignees: []
created_at: 2020-04-28T08:08:50Z
updated_at: 2020-04-28T15:56:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1563
synced_at: 2026-01-12T16:13:23Z
```

# The `--ignore-file` flag requires a space instead of equals with `~`

---

_@texastoland_

#### What version of ripgrep are you using?

12.0.1

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS 10.15.4

#### Describe your bug.

`--ignore-file ~/.config/rg/ignore` works but `--ignore-file=~/.config/rg/ignore` produces the error:

`file: No such file or directory (os error 2)`

`fd` which is a dependent produces an identical error CC @sharkdp.

<!--
#### What are the steps to reproduce the behavior?

If possible, please include both your search patterns and the corpus on which
you are searching. Unless the bug is very obvious, then it is unlikely that it
will be fixed if the ripgrep maintainers cannot reproduce it.

If the corpus is too big and you cannot decrease its size, file the bug anyway
and the ripgrep maintainers will help figure out next steps.

#### What is the actual behavior?

Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

If the output is large, put it in a gist: https://gist.github.com/

If the output is small, put it in code fences:

```
your
output
goes
here
```

#### What is the expected behavior?

What do you think ripgrep should have done?
-->

---

_Comment by @sharkdp on 2020-04-28 08:34_

I can not reproduce this, neither with `rg`, nor with `fd`.

What are the exact commands that you are running? Have you aliased `rg`/`fd` to something else?
```
❯ echo foo > a
❯ echo foo > b
❯ rg foo
b
1:foo

a
1:foo
❯ echo b > file
❯ rg foo --ignore-file=file
a
1:foo
❯ rg foo --ignore-file file
a
1:foo
```

---

_Comment by @texastoland on 2020-04-28 08:52_

Oh you're right! I'm using `--ignore-file=~/.config/rg/ignore`. I just tried it with `set -x`. The shell is doing the expansion with space but not for equals. Works as intended?

---

_Renamed from "The --ignore-file flag requires a space instead of equals (--ignore-file=file)" to "The --ignore-file flag requires a space instead of equals with `~`" by @texastoland on 2020-04-28 08:57_

---

_Renamed from "The --ignore-file flag requires a space instead of equals with `~`" to "The `--ignore-file` flag requires a space instead of equals with `~`" by @texastoland on 2020-04-28 08:57_

---

_Comment by @sharkdp on 2020-04-28 09:19_

That is the expected shell behavior:
```
$ echo --ignore-file ~/.something
--ignore-file /home/shark/.something
$ echo --ignore-file=~/.something
--ignore-file=~/.something
```
You would get a similar error if you (single- or double-) quote the `~`.

To my knowledge, it is uncommon for programs to do an additional expansion of `~`. It's a shell feature, not some fundamental Unix path.

I tend to avoid `~`, especially in scripts. I use `$HOME` instead.

---

_Comment by @texastoland on 2020-04-28 09:23_

Thanks for the help! Makes sense to me. I'll close unless @BurntSushi reopens it.

---

_Closed by @texastoland on 2020-04-28 09:23_

---

_Label `invalid` added by @BurntSushi on 2020-04-28 11:49_

---

_Comment by @okdana on 2020-04-28 15:56_

If you're using zsh (the default on macOS now), you can enable the `MAGIC_EQUAL_SUBST` option to make it work the way you expected:

```
% echo --ignore-file=~/.config/rg/ignore
--ignore-file=~/.config/rg/ignore
% setopt magic_equal_subst
% echo --ignore-file=~/.config/rg/ignore
--ignore-file=/Users/dana/.config/rg/ignore
```

Obviously you'll need to quote/escape when you don't want it to expand, same as with parameter assignments. The effect is explained in the [manual](http://zsh.sourceforge.net/Doc/Release/Options.html)

---
