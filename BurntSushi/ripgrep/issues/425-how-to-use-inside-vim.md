```yaml
number: 425
title: How to use inside vim?
type: issue
state: closed
author: Neats29
labels: []
assignees: []
created_at: 2017-03-30T09:29:26Z
updated_at: 2024-07-06T23:04:37Z
url: https://github.com/BurntSushi/ripgrep/issues/425
synced_at: 2026-01-12T16:13:22Z
```

# How to use inside vim?

---

_@Neats29_

Thanks for building this, it's very cool. Quick question, I was wondering if you could please let me know or add to the readme how to integrate this with vim. At the very least I'm interested in 'search and replace' globally within a repository.

Thank you.

 

---

_Comment by @FSMaxB on 2017-03-30 09:36_

You could ask [mileszs/ack.vim](https://github.com/mileszs/ack.vim) for integration of rg.

---

_Comment by @Neats29 on 2017-03-30 09:38_

@FSMaxB but from reading the other issues, it seems like some people are using it inside vim already, so I thought there might be a way...

---

_Comment by @Neats29 on 2017-03-30 09:53_

My bad for not searching the internet first. There is this plugin: [vim-ripgrep](https://github.com/jremmen/vim-ripgrep) and [this article](https://medium.com/@crashybang/supercharge-vim-with-fzf-and-ripgrep-d4661fc853d2) for using with [fzf](https://github.com/junegunn/fzf) :)

---

_Closed by @BurntSushi on 2017-03-31 11:52_

---

_Comment by @BurntSushi on 2017-03-31 11:53_

Sounds like you got your answer.

I wouldn't be opposed to putting links to editor integrations in the README. But since I don't use any of them myself, I probably won't do it.

---

_Comment by @ericandrewlewis on 2017-12-07 02:01_

> I wouldn't be opposed to putting links to editor integrations in the README. But since I don't use any of them myself, I probably won't do it.

It took me a little to figure out how to do this. Can I open a PR with some suggested context setting for folks interested in this?

---

_Comment by @BurntSushi on 2017-12-07 11:50_

@ericandrewlewis Yeah sure! Just put it toward the bottom of the README with the other questions. (At some point, I will split it out into a proper FAQ.)

---

_Comment by @pirj on 2018-04-15 23:18_

For the future researchers:
```
set grepprg=rg\ --vimgrep\ --no-heading\ --smart-case
```
works great with no other configuration required.

I use this mapping to open the list of the files in location list:
```
nnoremap <Leader>g :silent lgrep<Space>
```
and further navigate it:
```
nnoremap <silent> [f :lprevious<CR>
nnoremap <silent> ]f :lnext<CR>
```

---

_Comment by @dyng on 2019-07-21 05:00_

If you don't mind using a plugin, It's my pleasure to recommend my plugin [ctrlsf.vim](https://github.com/dyng/ctrlsf.vim) which integrates `rg` out of the box, and supports **asynchronous searching**.

![](https://raw.githubusercontent.com/dyng/i/master/ctrlsf.vim/async-demo.gif)

---

_Comment by @jpesce on 2020-10-01 16:15_

> For the future researchers:
> 
> ```
> set grepprg=rg\ --vimgrep\ --no-heading\ --smart-case
> ```
> 
> works great with no other configuration required.
> 
> I use this mapping to open the list of the files in location list:
> 
> ```
> nnoremap <Leader>g :silent lgrep<Space>
> ```
> 
> and further navigate it:
> 
> ```
> nnoremap <silent> [f :lprevious<CR>
> nnoremap <silent> ]f :lnext<CR>
> ```

As an addendum, since this issue comes up when searching for this in search engines.
Also set the format like so: `set grepformat+=%f:%l:%c:%m`
This will make the location list understand the output format better

---
11/01/2022: Updated `grepformat` — Thanks @craigmac!

---

_Comment by @nyngwang on 2022-01-29 07:48_

> For the future researchers:
> 
> ```
> set grepprg=rg\ --vimgrep\ --no-heading\ --smart-case
> ```
> 
> works great with no other configuration required.

@pirj A noob question here: If this works, why do we need a plugin like vim-ripgrep?

---

_Comment by @pirj on 2022-01-29 08:07_

> why do we need a plugin like vim-ripgrep?

At a glance, it offers match highlighting, and configuring quickfix window location.

---

_Comment by @craigmac on 2022-11-01 19:21_

> Also set the format like so: set grepformat=%f:%l:%c:%m,%f:%l:%m
This will make the location list understand the output format better

The second value is not needed, it's there in the default 'grepformat' string, and it's better to only append to that value instead of overwriting it, in case you don't have rg installed, you'll still have a usable grepformat value, so something like:

```vim
if executable('rg') | set grepformat+=%f:%l:%c:%m grepprg=rg <options you prefer> | endif
```

---

_Comment by @dehidehidehi on 2024-07-06 23:04_

Builing on the shoulders of what's been posted here.
This will use the `grep` command to search for a pattern, and then will prompt you for a replacement; it prefills most of the boiler plate to issue this command; I feel it's easier to search and replace everywhere if you have the visual confirmation (using the quickfix window) before proceeding to type the replacement.

```vim
" Handling search and replace boilerplate using ripgrep
if executable('rg') | set grepformat+=%f:%l:%c:%m grepprg=rg\ --vimgrep\ --no-heading\ --smart-case | endif
" We use feedkeys because we don't want to press enter at the end of this command.
command! -nargs=1 FindReplaceAll :silent grep <args> | copen | call feedkeys(":cdo %s/" . <q-args> . "/") | redraw!
" We use feedkeys because otherwise the screen isn't redrawn
nnoremap <silent> <C-r> :call feedkeys(':FindReplaceAll ')<CR>
```

---
