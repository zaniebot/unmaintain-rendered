```yaml
number: 237
title: Add an option to open matching files in $EDITOR
type: issue
state: closed
author: jkramer
labels:
  - question
assignees: []
created_at: 2016-11-18T14:03:38Z
updated_at: 2022-03-15T12:25:27Z
url: https://github.com/BurntSushi/ripgrep/issues/237
synced_at: 2026-01-12T16:13:21Z
```

# Add an option to open matching files in $EDITOR

---

_@jkramer_

Add an option (maybe -E) that, when set, makes rg go silent (no console output) and instead after finishing the search run `$EDITOR <file1> [file2] ...` with the matching files, as a convenient shortcut to `vim $(rg ...)`.

---

_Comment by @BurntSushi on 2016-11-18 14:10_

Have you tried `vim $(rg -l foo)`? If not, does that work for you? If you have tried it, why doesn't it work for you?


---

_Label `question` added by @BurntSushi on 2016-11-18 14:10_

---

_Comment by @jkramer on 2016-11-18 14:32_

That's what I'm currently doing. As I said in the original comment, it's about having a convenient shortcut to doing exactly that. I (and I guess many others) often search something, look at the result and then decide that I have to edit the file(s). Just going one up in the shell history and adding -E is really convenient and saves a lot of time. I've added this in [rst](https://github.com/jkramer/rst/blob/master/rst) (slimmer faster version of ack I made long ago) and don't wanna miss it. :)


---

_Comment by @BurntSushi on 2016-11-18 14:52_

I still don't quite buy it. Put this in `~/bin/rge`:

```
#!/bin/sh
exec vim $(rg -l "$@")
```

Now you can do:

```
$ rg foo
# i want to see in vim
$ rge foo
```

... which seems about roughly equivalent to adding `rg -E`.

Note that I've resisted a similar feature that causes the results of ripgrep to open in a pager by suggesting this alternative in `~/bin`:

```
#!/bin/sh
exec rg -p "$@" | less -RFX
```


---

_Comment by @BurntSushi on 2016-11-20 20:33_

OK, I'm going to close this because I think it's relatively easy to make this work in a low effort way without adding support to ripgrep proper.


---

_Closed by @BurntSushi on 2016-11-20 20:33_

---

_Comment by @mlippert on 2018-04-23 21:39_

I agree no need to add support in ripgrep proper, but your comment above, pretty much verbatim would make a good FAQ entry.

---

_Comment by @bradleybossard on 2019-09-11 21:53_

FWIW, I'm kind of a fan of this tool

https://github.com/aykamko/tag

which augments the `ripgrep` output with a numbering system, then allows you to type `e<number>` to jump straight to that result in VIM.

---

_Comment by @SMUsamaShah on 2021-10-25 16:38_

@BurntSushi 
What about cases when we have multiple results and want to open one of those in editor, would you recommend using https://github.com/aykamko/tag or would be a better idea to have this as a ripgrep feature? I think it will be nicer to have this built in instead of using another tool. 

My use case is searching in thousands of code files with results are most often deep directories spread around all over the place. I usually use mouse to copy path and open in editor (which isn't full path by default). Or copy and open it in nano/vim.

It would be equally great even if you could just recommend ripgrep flags to do this in a saner way, which is opening any one of the results in an editor quickly.

**EDIT: tag does not work on windows**

---

_Comment by @BurntSushi on 2021-10-25 16:53_

@SMUsamaShah I don't know what you would consider to be a "saner" way that ripgrep could offer you. In some terminals, you can click on file paths and have them open in your text editor. You might also configure your text editor to do the searching instead of doing it in a terminal. Or just be okay with typing or copying the path. That's what I do.

---

_Comment by @SMUsamaShah on 2021-10-25 19:19_

Sorry if the use of 'saner' felt offensive. I use `ripgrep` with same love as I use `Voidtools Everything`. 

> In some terminals, you can click on file paths and have them open in your text editor.  

Thanks for telling this. On windows `cmder` can do that by pressing `left ctrl` to highlight the path.

---

_Comment by @BurntSushi on 2021-10-25 19:45_

No "sane" is fine. I just wanted to know what sort of thing you wanted and what you meant by it. "Sane" is just ambiguous, so it is difficult to interpret what you mean when you say that. It's fine in some contexts where you don't need or want to be specific, but specificity is important here. :)

---

_Comment by @SMUsamaShah on 2021-10-26 22:57_

By sane I meant an easier alternative to copy pasting or writing the path to open one of the results. From your response I learned about this feature in cmder which just works for me. Thanks. :) 

---

_Comment by @matkoniecz on 2022-02-24 11:23_

<s>It seems that following works well when you use `.bashrc` file:

```
rgopen() {
    codium $(rg -l "$@")
}
```
https://github.com/matkoniecz/dotfiles_public/commit/1a4313efac57d373136394dd2a960ea0c1180f11</s>

this solution is broken: migrated in https://github.com/matkoniecz/dotfiles_public/commit/154b1573e680b171c5cfd6f80061d431733b5e34 from bash to python

```
import sys
import subprocess
import os

searched = sys.argv[1]
returned = subprocess.check_output(["rg", "-l", searched]).decode('utf-8').strip().split("\n")
for file in returned:
    os.system('codium "' + file + '"') 
```

---

_Comment by @matkoniecz on 2022-03-15 12:24_

@BurntSushi 

> Have you tried vim $(rg -l foo)? If not, does that work for you? If you have tried it, why doesn't it work for you?

is seems that it will fail for files with spaces in them

---
