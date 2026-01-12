```yaml
number: 1274
title: Question about vimgrep
type: issue
state: closed
author: slmjkdbtl
labels: []
assignees: []
created_at: 2019-05-05T01:44:55Z
updated_at: 2019-05-05T13:42:35Z
url: https://github.com/BurntSushi/ripgrep/issues/1274
synced_at: 2026-01-12T16:13:23Z
```

# Question about vimgrep

---

_@slmjkdbtl_

#### What version of ripgrep are you using?

ripgrep 11.0.1

#### How did you install ripgrep?

homebrew

#### What operating system are you using ripgrep on?

macOS Mojave 10.14.5

#### Describe your question, feature request, or bug.

When using with vim's quickfix list and `vimgrep`, the entries provided by `getqflist()` doesn't have `.col` property, instead the column number is in the `.text` field (my rg and vim setup is pretty much like https://github.com/jremmen/vim-ripgrep)
So the entries I got are like
```
line: 16
col: 0
vcol: 0 
text: '8:struct export_config {'
```
Haven't worked with vimgrep and quickfix list before, is this how it's supposed to be? 

neovim v0.3.5


---

_Comment by @BurntSushi on 2019-05-05 02:06_

Sorry, but this bug report isn't actionable. There aren't enough details for me to reproduce the issue you're having. Please clarify exactly which command you're running, its input, its actual output and its expected output **separate from any particular text editor**.

---

_Comment by @slmjkdbtl on 2019-05-05 03:00_

Ok, here's more detail:

directory:
```
.
└── test.txt
```

test.txt:
```
1yo1
yo11
```

running ripgrep
```
$ rg "yo" --vimgrep
test.txt:1:2:1yo1
test.txt:2:1:yo11
```

trying to work with vimgrep:
```viml
let &grepprg = 'rg --vimgrep'
grep! 'yo'
for d in getqflist()
    echom d.lnum ':' d.col '   ' d.text
endfor
```
expected behavior (output correct column and text):
```
1 : 1     1yo1
1 : 4     1yo1
2 : 3     yo11
2 : 4     yo11
```
actual behavior (the column number is inside text):
```
1 : 0     1:1yo1
1 : 0     4:1yo1
2 : 0     3:yo11
2 : 0     4:yo11
```
not sure if it's a vim parsing problem

---

_Comment by @BurntSushi on 2019-05-05 13:42_

This isn't a ripgrep problem. You linked to this repo https://github.com/jremmen/vim-ripgrep, and near the top of the README, it tells you what to use for `grepformat`.

For example:

```vim
let &grepprg = 'rg --vimgrep'
let &grepformat = '%f:%l:%c:%m'
grep! 'yo'
for d in getqflist()
    echom d.lnum ':' d.col '   ' d.text
endfor
```

In general, if you can't reproduce the problem inside a shell *outside of a text editor*, then the issue probably doesn't belong here.

---

_Closed by @BurntSushi on 2019-05-05 13:42_

---
