```yaml
number: 81
title: Usage with vim/neovim and fzf.vim
type: issue
state: closed
author: chopfitzroy
labels: []
assignees: []
created_at: 2016-09-25T09:37:36Z
updated_at: 2016-09-26T06:58:51Z
url: https://github.com/BurntSushi/ripgrep/issues/81
synced_at: 2026-01-12T18:23:11Z
```

# Usage with vim/neovim and fzf.vim

---

_@chopfitzroy_

I am trying to set this up with [fzf.vim](https://github.com/junegunn/fzf.vim) and as per this [comment](https://github.com/junegunn/fzf.vim/pull/36#issuecomment-249406310) `rg` does not seem to currently work.

I was hoping you would have some insight into this?

Cheers.


---

_Renamed from "Usage with vim/neovim" to "Usage with vim/neovim and fzf.vim" by @chopfitzroy on 2016-09-25 09:38_

---

_Comment by @junegunn on 2016-09-25 10:39_

Another problem I noticed is this:

<img width="341" alt="rg-column" src="https://cloud.githubusercontent.com/assets/700826/18814749/b520b9e4-8357-11e6-8d25-6c9a8da610ed.png">

I don't think it's intentional?


---

_Comment by @mengelbrecht on 2016-09-25 11:13_

You mean the missing line numbers? If the output is not to a tty the parameter for line numbers -n has to be explicitly specified.


---

_Comment by @BurntSushi on 2016-09-25 12:49_

Yes, it is intentional. When searching at a tty, line numbers, headings and colors are enabled by default. Otherwise, the default format is the same as `git grep`.

I otherwise don't know what the problem is here. Could you please describe it in more detail?


---

_Comment by @junegunn on 2016-09-25 13:02_

I see, so then it's not a problem. The issue we have is that the output of rg (0.1.17) is not captured by `system()` function of vim unlike grep or ag.

``` vim
:echo system('ag foo')
:echo system('grep foo *')

:echo system('rg foo')
```

EDIT:

``` sh
ag foo < /dev/null
grep foo * < /dev/null
rg foo < /dev/null
```


---

_Comment by @BurntSushi on 2016-09-25 14:21_

I think `rg` and `grep` actually have the same behavior here. If you pass a file path argument, then `stdin` is ignored. The issue here is what happens when no path is given. Here's an illustration:

```
$ echo PM_RESUME | ag PM_RESUME | wc -l
1
$ ag PM_RESUME < /dev/null | wc -l
16
$ echo PM_RESUME | rg PM_RESUME | wc -l
1
$ rg PM_RESUME < /dev/null | wc -l
0
```

I bet this is the same bug that's being seen in #35.


---

_Comment by @BurntSushi on 2016-09-25 15:13_

Hooray, this is indeed a dupe of #35! Fixing this fixes that too.


---

_Comment by @BurntSushi on 2016-09-25 15:15_

Weird. Github didn't close this. Fixed in ab0d1c1c797c0464ad4fcdfc641bcf88f9ade304.


---

_Closed by @BurntSushi on 2016-09-25 15:15_

---

_Comment by @chopfitzroy on 2016-09-25 20:26_

So just to clarify does `rg` need to be invoked with `--column` or `--no-heading` for this to now work?

Going of #35..?

Cheers.


---

_Comment by @BurntSushi on 2016-09-25 20:30_

@CrashyBang I don't think the problem has actually been explained to me, but I imagine you'd want `rg --no-heading --vimgrep`.


---

_Comment by @chopfitzroy on 2016-09-25 20:49_

Hey @BurntSushi okay sweet will go over as soon as I am home from work will let you know how it goes hoping to do a write up on medium `rg` has already massively supercharged my workflow.


---

_Comment by @BurntSushi on 2016-09-25 20:56_

@CrashyBang glad to hear it! Please send a link over if you do that write up. :-) (I'd also be happy to read a draft.)


---

_Comment by @chopfitzroy on 2016-09-26 06:17_

Hey @BurntSushi write up here: https://medium.com/@crashybang/supercharge-vim-with-fzf-and-ripgrep-d4661fc853d2 sorry wanted to get it up but still making tweaks if needed.


---

_Comment by @junegunn on 2016-09-26 06:46_

@CrashyBang I would advise against suggesting `-i` and `-e` as the default. fzf by default performs smart-case matching which is really handy with CamelCase files. `-i` disables that and makes the search always case-insensitive. You might also want to experiment without `-e` especially because of [the new scoring mechanism](https://github.com/junegunn/fzf/issues/638) introduced in 0.15.0.


---

_Comment by @chopfitzroy on 2016-09-26 06:57_

Hey @junegunn noted and removed :).

Otherwise it is okay?


---
