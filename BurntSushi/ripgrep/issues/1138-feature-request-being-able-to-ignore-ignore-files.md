```yaml
number: 1138
title: "Feature request: being able to ignore .ignore files, but not .gitignore"
type: issue
state: closed
author: JelteF
labels:
  - question
assignees: []
created_at: 2018-12-11T11:12:43Z
updated_at: 2019-01-26T18:45:15Z
url: https://github.com/BurntSushi/ripgrep/issues/1138
synced_at: 2026-01-12T16:13:23Z
```

# Feature request: being able to ignore .ignore files, but not .gitignore

---

_@JelteF_

#### What version of ripgrep are you using?
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

pacman

#### What operating system are you using ripgrep on?

Arch linux

#### Describe your question, feature request, or bug.

I would like to have a flag that allows ignoring of .ignore files, but still ignore the files in .gitignore. The reason for me is that I have two main uses for `rg`. 
1. Searching files for content.
2. Very quickly creating a list of all files in the current directory that I might want to open. I pass the output of `rg --files --hidden --ignore-file ~/.vim/fzf_ignore` to to FZF in vim. This allows me to fuzzy search all the file names in the current repo that are not ignored by git. `~/.vim/fzf_ignore` contains a file with stuff like `.git/` and `.svn/`.

The problem I keep running into is that I add some files to the .ignore file that I don't want to search. But then I also cannot open them with the fuzzy search anymore. The reason I add them to .ignore is because they are big files that will often match my search, such as csv files, a dev database dump or a vendored minified js file. 

The addition of a `--no-ignore-dot-ignore` flag would solve this problem for me.

---

_Comment by @BurntSushi on 2018-12-11 12:19_

I think that the various "no ignore" flags are already too complex. We might be doomed to  support every possible combination, but I'd like to resist doing that for the time being. Meanwhile, would you consider using [fd](https://github.com/sharkdp/fd) instead for listing files? fd is, after all, designed for listing files.

---

_Label `question` added by @BurntSushi on 2018-12-11 12:19_

---

_Comment by @JelteF on 2018-12-11 16:23_

Yeah using `fd` would make sense indeed, but it has even less flags to change the behaviour. One other way that would work for me is to do make the following work:
```
rg --no-ignore --ignore-vcs
```

Right now it doesn't use the vcs ignores

---

_Closed by @BurntSushi on 2019-01-26 18:41_

---

_Comment by @BurntSushi on 2019-01-26 18:41_

Master now has the `--no-ignore-dot` flag. It will be in the next release.

---

_Comment by @JelteF on 2019-01-26 18:45_

Awesome :) 

---
