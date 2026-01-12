```yaml
number: 73
title: Glob does not respect --no-ignore
type: issue
state: closed
author: chopfitzroy
labels: []
assignees: []
created_at: 2016-09-25T03:10:57Z
updated_at: 2016-09-25T18:26:39Z
url: https://github.com/BurntSushi/ripgrep/issues/73
synced_at: 2026-01-12T18:23:11Z
```

# Glob does not respect --no-ignore

---

_@chopfitzroy_

Hey I am trying to set this up with [fzf](https://github.com/junegunn/fzf) and replace [ag](https://github.com/ggreer/the_silver_searcher), this was my original `FZF_DEFAULT_COMMAND`:  

`set -g -x FZF_DEFAULT_COMMAND 'ag --ignore-case --skip-vcs-ignores --depth -1 -g ""'`

And here is my new `rg` one:  

`set -g -x FZF_DEFAULT_COMMAND 'rg --no-ignore --hidden --follow ""'`

Now this almost works but it is not listing things like `.gitignore`.

Any ideas?


---

_Comment by @BurntSushi on 2016-09-25 03:27_

Can you please try to reproduce the problem on the command line?

On Sep 24, 2016 11:10 PM, "Otis Wright" notifications@github.com wrote:

> Hey I am trying to set this up with fzf https://github.com/junegunn/fzf
> and replace ag https://github.com/ggreer/the_silver_searcher, this was
> my original FZF_DEFAULT_COMMAND:
> 
> set -g -x FZF_DEFAULT_COMMAND 'ag --ignore-case --skip-vcs-ignores --depth
> -1 -g ""'
> 
> And here is my new rg one:
> 
> set -g -x FZF_DEFAULT_COMMAND 'rg --no-ignore --hidden --follow ""'
> 
> Now this almost works but it is not listing things like .gitignore.
> 
> Any ideas?
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/73, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34mkm0FRJUoljpyrccCqprrOmjlwtks5qteZCgaJpZM4KFzkI
> .


---

_Comment by @chopfitzroy on 2016-09-25 04:03_

Okay on the command line I have to use this:  

`rg --no-ignore --hidden --files --follow ''`  

This **does** fix the `fzf` issue, however this shows all my dotfiles however I have now realised I would like to hide everything in the `.git/` folder so I tried this:  

`rg --no-ignore --hidden --files --follow '' --glob '!.git/*'`  

However the `--glob` argument seems to be being overridden by `--no-ignore`.

Cheers.


---

_Renamed from "Not showing dotfiles in results" to "Glob does not respect --no-ignore" by @chopfitzroy on 2016-09-25 04:05_

---

_Comment by @BurntSushi on 2016-09-25 18:26_

This is fixed in current master. I'll get a new release out today.


---

_Closed by @BurntSushi on 2016-09-25 18:26_

---
