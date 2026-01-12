```yaml
number: 1672
title: "--glob=!*.pod not work at .ripgreprc but work at command line"
type: issue
state: closed
author: jiangjianshan
labels:
  - invalid
assignees: []
created_at: 2020-09-01T03:12:19Z
updated_at: 2020-09-01T10:32:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1672
synced_at: 2026-01-12T16:13:24Z
```

# --glob=!*.pod not work at .ripgreprc but work at command line

---

_@jiangjianshan_

Hello,

I have download ranger-1.9.3.tar.gz from https://github.com/ranger/ranger/archive/v1.9.3.tar.gz . And use ripgrep work with fzf to search word from files. Current I have found a issue about .ripgreprc. I want to exclude the *.pod file, e.g. doc/ranger.pod as below picture showing.

![image](https://user-images.githubusercontent.com/20179047/91790264-e740c600-ec42-11ea-8036-357b356ac08e.png)

My Vim version is 8.2.1531 and fzf version is 0.22.0. My ripgrep version information are following:
ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

Here is my setting for .ripgreprc

---------------------------------- .ripgreprc begin --------------------------------------
--max-columns=150
--max-columns-preview

--glob=!*.{1,md,pob,o,obj,exe,a,a2l,lst,map}
--glob=!LICENSE
--glob=!AUTHORS

--colors=line:fg:yellow
--colors=line:style:bold
--colors=path:fg:green
--colors=path:style:bold
--colors=match:fg:black
--colors=match:bg:yellow
--colors=match:style:nobold

--smart-case
----------------------------------- .ripgreprc end ---------------------------------------

Here is part of my _vimrc settings:

-------------------------------- _vimrc begin ------------------------------------
if has('win32')
    let $RIPGREP_CONFIG_PATH=expand("F:/SPB_Data/.ripgreprc")
else
    let $RIPGREP_CONFIG_PATH='~/.ripgreprc'
endif

" Get text in files with Rg
command! -bang -nargs=* Rg
  \ call fzf#vim#grep(
  \   'rg --column --line-number --no-heading --color=always --smart-case '.shellescape(<q-args>), 1,
  \   fzf#vim#with_preview(), <bang>0)
noremap <silent> <C-f> :Rg <C-R><C-W><CR>
-------------------------------- _vimrce end -------------------------------------------

If I add -g "!*.pod" after --smart-case above, i.e.

-------------------------------- _vimrc begin ------------------------------------
" Get text in files with Rg
command! -bang -nargs=* Rg
  \ call fzf#vim#grep(
  \   'rg --column --line-number --no-heading --color=always --smart-case '.shellescape(<q-args>), 1,
  \   fzf#vim#with_preview(), <bang>0)
-------------------------------- _vimrc end -------------------------------------------

The search result can filter out .pod file as I want. But seems like --glob=!*.{1,md,pob,o,obj,exe,a,a2l,lst,map} can't work. --glob=!LICENSE and --glob=!AUTHORS can work. Which means the file .ripgreprc has been sucessfuly link to ripgrep but only some of --glob can't work.  Even I try to use --glob=!*/*.pod or --glob=!**/*.pod still can't exclude .pod via .ripgreprc. But in command line --glob=!*.pod can work.

I have read out the link of https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file 
, but cann't find the answer.  How to exclude .pod files via .ripgreprc?


---

_Comment by @okdana on 2020-09-01 03:47_

I've only just skimmed this (and your formatting is broken so it's hard to follow), but the line in the config file you posted says `pob` instead of `pod`. Is that the problem? Or did you actually try `--glob=!*.pod` (exactly) in the config file?

---

_Comment by @jiangjianshan on 2020-09-01 05:06_

@okdana ,  finally I have find the reason. I should use --glob=!.{1,md,pod,o,obj,exe,a,a2l,lst,map} but not --glob=!.{1,md,pob,o,obj,exe,a,a2l,lst,map}, that will work. I have made one character misstaken which should be '*.pod' but I write it as '*.pob' in .ripgreprc. Now it work. Sorry for my mistaken.

---

_Closed by @jiangjianshan on 2020-09-01 05:06_

---

_Label `invalid` added by @BurntSushi on 2020-09-01 10:32_

---
