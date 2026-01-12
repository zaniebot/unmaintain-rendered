```yaml
number: 2573
title: Add column end number for vimgrep
type: issue
state: closed
author: 0keke
labels:
  - wontfix
assignees: []
created_at: 2023-07-30T05:38:59Z
updated_at: 2023-07-30T15:55:34Z
url: https://github.com/BurntSushi/ripgrep/issues/2573
synced_at: 2026-01-12T16:13:24Z
```

# Add column end number for vimgrep

---

_@0keke_

## In-short
"vimgrep", which is built-in grep command in vim shows both column start number and column end number, while ripgrep only shows column start number. Ripgrep should also show column end number.

### as-is
`ripgrep --vimgrep` only shows column start number.
<img width="588" alt="スクリーンショット 2023-07-30 14 27 32" src="https://github.com/BurntSushi/ripgrep/assets/61049444/a34278de-db1d-49d0-8246-fbfaa6cdd243">
<img width="398" alt="スクリーンショット 2023-07-30 14 28 30" src="https://github.com/BurntSushi/ripgrep/assets/61049444/4263064a-10cd-4a3a-ab5c-57e9b719ab0c">

### to-be
~~`ripgrep --vimgrep`~~ `ripgrep --vimgre---new-option`  shows both column start number and column end number.
<img width="504" alt="スクリーンショット 2023-07-30 14 29 37" src="https://github.com/BurntSushi/ripgrep/assets/61049444/baaa4b57-8643-4037-ad8d-1ac15f66d8cc">

### motivation
Vim can highlight the matched words using column number information, but currently we cannot get correct result for lacking column end number.  


---

_Comment by @BurntSushi on 2023-07-30 10:03_

Where is the vimgrep specification documenting the format supported?

And in particular, why have you not considered the impact of what is ostensibly a breaking change?

---

_Comment by @0keke on 2023-07-30 14:14_


> Where is the vimgrep specification documenting the format supported?

I am sorry; my writing might have confused you. I fixed my first comment.

> Where is the vimgrep specification documenting the format supported?

You can check vimgrep specification in vim [help](https://github.com/vim/vim/blob/4c0089d696b8d1d5dc40568f25ea5738fa5bbffb/runtime/doc/quickfix.txt#L1033), but it's not helpful to solve this problem IMO.
Vim treats std output from grep-tool using options such as `goreformat`, `errorformat` as you can see below.

------
### NOTE
#### as-is

`rg <target word> --vimgrep`
output
```
<filename>:<line number>:<column number>:<error message>
```
→ `set grepformat=%f:%l:%c:%m`

`:grep <target word>`

#### to-be(for example)
`rg <target word> --vimgrep --some-option`
output
```
<filename>:<line number>:<column number>:<end colmun number>:<error message>
```
→ `set grepformat=%f:%l:%c:%k:%m`


---

_Comment by @0keke on 2023-07-30 14:18_

Thanks to [thinca](https://github.com/thinca) in vim-jp, I have solved the problem.
We don't have to add new option to ripgrep, using `jq` command.


Thx for your reply. I'll close this request.


[config example](https://github.com/thinca/config/blob/14be02dea5aab1bb5eaa667c9a86de595bba7bba/dotfiles/dot.vim/vimrc#L601-L604)
```vim
if executable('rg') && executable('jq')
    let &grepprg = 'rg --json $* \| jq -r ''select(.type=="match")\|.data as $data\|$data.submatches[]\|"\($data.path.text):\($data.line_number):\(.start+1):\(.end+1):\($data.lines.text//""\|sub("\n$";""))"'''
    set grepformat=%f:%l:%c:%k:%m
endif
```

---

_Comment by @BurntSushi on 2023-07-30 15:55_

Sounds good.

---

_Closed by @BurntSushi on 2023-07-30 15:55_

---

_Label `wontfix` added by @BurntSushi on 2023-07-30 15:55_

---
