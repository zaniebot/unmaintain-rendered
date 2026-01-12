```yaml
number: 1818
title: "`--hidden` doesn't show in zsh completion"
type: issue
state: closed
author: fvictorio
labels:
  - invalid
assignees: []
created_at: 2021-03-11T14:17:01Z
updated_at: 2021-03-11T15:03:07Z
url: https://github.com/BurntSushi/ripgrep/issues/1818
synced_at: 2026-01-12T16:13:24Z
```

# `--hidden` doesn't show in zsh completion

---

_@fvictorio_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1 (rev a6d05475fb)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Local build

#### What operating system are you using ripgrep on?

Ubuntu 20.04

#### Describe your bug.

I added the `_rg` completion script to my `$fpath`. When I do `rg --h<tab>`, the only suggestions I get are `--heading` and `--help`, no `--hidden`.

#### What is the expected behavior?

`--hidden` should show up.

---

I think that the fact that the option being hidden is... hidden is not a coincidence. I tried making this change:

```diff
     + '(hidden)' # Hidden-file options
-    '--hidden[search hidden files and directories]'
+    '--\hidden[search hidden files and directories]'
     $no"--no-hidden[don't search hidden files and directories]"
```

And the problem went away. But this was pure guesswork! I can't find a proper reference in zsh docs, nor I understand enough of zsh completion to explain why this fixes it.

---

_Comment by @BurntSushi on 2021-03-11 14:21_

What version of zsh are you using? I can't reproduce your problem. `--hidden` shows up as a completion option for me. My zsh version:

```
$ zsh --version
zsh 5.8 (x86_64-pc-linux-gnu)
```

cc @okdana 

---

_Comment by @fvictorio on 2021-03-11 14:52_

Same one:

```
zsh 5.8 (x86_64-ubuntu-linux-gnu)
```

This is my `setopt` just in case:

```
alwaystoend
autocd
autopushd
completeinword
extendedglob
extendedhistory
noflowcontrol
histexpiredupsfirst
histignoredups
histignorespace
histverify
incappendhistory
interactive
interactivecomments
longlistjobs
monitor
promptsubst
pushdignoredups
pushdminus
shinstdin
zle
```

---

_Comment by @fvictorio on 2021-03-11 14:55_

Tried installing `zsh` in a docker container and I cannot reproduce the issue. Weird. I will assume this has something to do with my setup and close this. I will comment here if by any chance I find the reason.

---

_Closed by @fvictorio on 2021-03-11 14:55_

---

_Comment by @fvictorio on 2021-03-11 14:58_

Ugh, I'm such an idiot, I already had aliased `rg` to `rg -S --hidden`. Sorry for wasting your time :sweat_smile: 

---

_Comment by @BurntSushi on 2021-03-11 15:03_

Ah nice. Thanks for sharing the solution!

---

_Label `invalid` added by @BurntSushi on 2021-03-11 15:03_

---
