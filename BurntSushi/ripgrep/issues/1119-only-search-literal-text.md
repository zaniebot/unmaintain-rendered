```yaml
number: 1119
title: Only search literal text 
type: issue
state: closed
author: Xatenev
labels: []
assignees: []
created_at: 2018-11-23T12:17:49Z
updated_at: 2025-07-10T13:18:23Z
url: https://github.com/BurntSushi/ripgrep/issues/1119
synced_at: 2026-01-12T16:13:23Z
```

# Only search literal text 

---

_@Xatenev_

#### What version of ripgrep are you using?

ripgrep 0.6.0 
-AVX -SIMD

#### How did you install ripgrep?

cargo install

#### What operating system are you using ripgrep on?

Ubuntu 16.04

#### Describe your question, feature request, or bug.

Hi,

I am using ripgrep via fzf.vim inside vim. I'd like to only find exact matches of my text without using the regex engine but I haven't found how to do that in the docs.

Example:

Search term: `exists: function`

I only want to find text that is literally `exists: function`.

What arguments do I have to pass to `rg` that it only finds literal text?

I have tried using     -F, --fixed-strings        Treat the pattern as a literal string instead of
                               a regular expression. 
but I get the results seen below

 https://gyazo.com/03a5c370b24b635e04ba01b384ae2c48

I am not 100% sure if this is a ripgrep question or a FZF question, if it is related to FZF please close this and I will move over.

Greetings
Xatenev

EDIT: My current full `rg` call is `rg --column --line-number --no-heading --fixed-strings --ignore-case --no-ignore --hidden -g "*.js" -g "*.java" --follow --glob "!build/*" --glob "!.git/*" --color "always"`

EDIT 2: Fixed it, it was a FZF configuration

---

_Closed by @Xatenev on 2018-11-23 12:36_

---

_Comment by @BurntSushi on 2018-11-23 12:41_

I'm glad you fixed it. Could you please share your fix so that others can benefit from it?

Also, in the future, when filling bugs against ripgrep, please include a reproduction that uses ripgrep directly and not through some other tool.

---

_Comment by @Xatenev on 2018-11-26 18:43_

The FZF Configuration that solved the problem was 

`{ 'options': '-e' }`

E.g. 
`command! -bang -nargs=* Find call fzf#vim#grep('rg --column --line-number --no-heading --fixed-strings --smart-case --no-ignore --hidden -g "*.js" -g "*.java" --follow --glob "!build/*" --glob "!.git/*" --color "always" '.shellescape(<q-args>).'| tr -d "\017"', 1, { 'options': '-e' },<bang>0)`

Nothing related to `ripgrep`, nor a bug in `fzf` - just gotta read the docs :).

---

_Comment by @philgyford on 2019-06-07 10:45_

Thanks for posting this @Xatenev ... I'm having trouble working out where to add `{'options': '-e'}` in my configuration though, as I have the extra `with_preview` section, and everywhere I've tried adding it causes errors:

```vim
command! -bang -nargs=* Find
  \ call fzf#vim#grep(
  \   'rg --column --line-number --no-heading --color=always --fixed-strings --follow --smart-case --glob "!{.git/*,node_modules/*}" '.shellescape(<q-args>), 1,
  \   <bang>0 ? fzf#vim#with_preview('up:40%')
  \           : fzf#vim#with_preview('right:50%:hidden', '?'),
  \   <bang>0
  \ )
```
Any ideas?

---

_Comment by @danamkaplan on 2019-07-29 18:48_

> Thanks for posting this @Xatenev ... I'm having trouble working out where to add `{'options': '-e'}` in my configuration though, as I have the extra `with_preview` section, and everywhere I've tried adding it causes errors:
> 
> ```viml
> command! -bang -nargs=* Find
>   \ call fzf#vim#grep(
>   \   'rg --column --line-number --no-heading --color=always --fixed-strings --follow --smart-case --glob "!{.git/*,node_modules/*}" '.shellescape(<q-args>), 1,
>   \   <bang>0 ? fzf#vim#with_preview('up:40%')
>   \           : fzf#vim#with_preview('right:50%:hidden', '?'),
>   \   <bang>0
>   \ )
> ```
> 
> Any ideas?


I just had this issue. Try putting in after the second call to fzf like I did for your snippet below.
```viml
command! -bang -nargs=* Find
  \ call fzf#vim#grep(
  \   'rg --column --line-number --no-heading --color=always --fixed-strings --follow --smart-case --glob "!{.git/*,node_modules/*}" '.shellescape(<q-args>), 1,
  \   <bang>0 ? fzf#vim#with_preview('up:40%')
  \           : fzf#vim#with_preview({ 'options': '-e' }, 'right:50%:hidden', '?'),
  \   <bang>0
  \ )
```

I can however guarantee that my following call makes Rg NOT search filenames and also makes the FZF interface EXACT and NOT fuzzy, only when content grepping. 

```viml
" Make Ripgrep ONLY search file contents and not filenames
command! -bang -nargs=* Rg
  \ call fzf#vim#grep(
  \   'rg --column --line-number --hidden --ignore-case --no-heading --color=always '.shellescape(<q-args>), 1,
  \   <bang>0 ? fzf#vim#with_preview({'options': '--delimiter : --nth 4..'}, 'up:60%')
  \           : fzf#vim#with_preview({'options': '--delimiter : --nth 4.. -e'}, 'right:50%:hidden', '?'),
  \   <bang>0)
```


---

_Comment by @philgyford on 2019-09-04 17:12_

Thanks for this @danamkaplan ! I've spent some time puzzling over this and finally have it all working, with pretty much your final example shown.

Now, if I search for `dogs cats` then I get lines containing "dogs" and/or "cats". (Previously I also got lines containing any of those letters.)

If I search for `dogs\ cats` then I only get lines containing the phrase "dogs cats".

Whatever it is that stops fzf searching in filenames is also a real help - thanks again!

---

_Comment by @bnoordsij on 2025-07-10 05:51_

a bit slow, but I use an alias for that
```
rg_raw() {
   regex=$*
   rg "$regex" | sed -E "s~.*$regex.*~\1~"
}
```
you can use it like this: to for example find all quoted text `rg_raw (".*")`

---

_Comment by @BurntSushi on 2025-07-10 13:18_

@bnoordsij I think this is a different issue. This issue is something about fzf, not ripgrep. In any case, it seems like you could just do `rg '"([^"]+)"' -or '$1'`.

---
