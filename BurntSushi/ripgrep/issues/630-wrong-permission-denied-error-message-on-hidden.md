```yaml
number: 630
title: "Wrong \"Permission denied\" error message on hidden files"
type: issue
state: closed
author: fcantournet
labels: []
assignees: []
created_at: 2017-10-09T15:27:55Z
updated_at: 2017-11-30T15:24:30Z
url: https://github.com/BurntSushi/ripgrep/issues/630
synced_at: 2026-01-12T16:13:22Z
```

# Wrong "Permission denied" error message on hidden files

---

_@fcantournet_

`rg` refuses to search hidden files, reporting a permission denied error.

```
 fcantournet ~ $ ls -lah .bashrc 
-rw-r--r-- 1 fcantournet fcantournet 4,0K oct.   3 14:09 .bashrc
 fcantournet ~ $ rg -i complete .bashrc
.bashrc: Permission denied (os error 13)
```

This is only true directly from my home directory.  if I move the file to a directory inside my home then it works.
```
 fcantournet  ~  $  ls -lah testrg/
total 12K
drwxrwxr-x  2 fcantournet fcantournet 4,0K oct.   9 17:28 .
drwxr-xr-x 39 fcantournet fcantournet 4,0K oct.   9 17:28 ..
-rw-r--r--  1 fcantournet fcantournet 4,0K oct.   9 17:28 .bashrc
 fcantournet  ~  $  rg --debug bash /home/fcantournet/testrg/.bashrc 
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'b',
        'a',
        's',
        'h'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(bash)], limit_size: 250, limit_class: 10 }
DEBUG:rg::args: will try to use memory maps
1:# ~/.bashrc: executed by bash(1) for non-login shells.
2:# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
12:# See bash(1) for more options
18:# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
111:# ~/.bash_aliases, instead of adding them here directly.
112:# See /usr/share/doc/bash-doc/examples in the bash-doc package.
114:if [ -f ~/.bash_aliases ]; then
115:    . ~/.bash_aliases
119:# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
120:# sources /etc/bash.bashrc).
122:  if [ -f /usr/share/bash-completion/bash_completion ]; then
123:    . /usr/share/bash-completion/bash_completion
124:  elif [ -f /etc/bash_completion ]; then
125:    . /etc/bash_completion
129:[ -f ~/.fzf.bash ] && source ~/.fzf.bash
```


Version is 0.5.2. I'll try with 0.6.0 asap.
 
debug output : 
```
rg --debug bash /home/fcantournet/.bashrc 
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'b',
        'a',
        's',
        'h'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(bash)], limit_size: 250, limit_class: 10 }
DEBUG:rg::args: will try to use memory maps
/home/fcantournet/.bashrc: Permission denied (os error 13)
```





---

_Comment by @BurntSushi on 2017-10-09 15:30_

What version of ripgrep? What is the output with the --debug flag?

On Oct 9, 2017 11:27 AM, "Félix Cantournet" <notifications@github.com>
wrote:

> rg refuses to search hidden files, reporting a permission denied error.
>
>  fcantournet ~ $ ls -lah .bashrc
> -rw-r--r-- 1 fcantournet fcantournet 4,0K oct.   3 14:09 .bashrc
>  fcantournet ~ $ rg -i complete .bashrc
> .bashrc: Permission denied (os error 13)
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/630>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34i83fPs0KNWxuQBPXcge-Q19RnAQks5sqjt9gaJpZM4PyqfK>
> .
>


---

_Comment by @fcantournet on 2017-10-09 15:37_

@BurntSushi It appears to be fixed in 0.6.0. :ok_hand: 

---

_Closed by @BurntSushi on 2017-10-09 16:41_

---

_Comment by @fcantournet on 2017-11-30 15:24_

For what it's worth : the behavior I encountered has nothing to do with ripgrep and everything to do with running inside snap default confinement.

If you end up here someday : `snap install rg --classic` will solve your problems.
The default `strict` confinement has some weird interactions with hidden files..

---
