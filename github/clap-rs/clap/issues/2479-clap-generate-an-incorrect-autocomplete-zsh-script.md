---
number: 2479
title: clap_generate an incorrect autocomplete zsh script
type: issue
state: closed
author: mothsART
labels:
  - C-bug
  - A-completion
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-05-13T13:17:07Z
updated_at: 2021-05-21T11:47:53Z
url: https://github.com/clap-rs/clap/issues/2479
synced_at: 2026-01-07T13:12:19-06:00
---

# clap_generate an incorrect autocomplete zsh script

---

_Issue opened by @mothsART on 2021-05-13 13:17_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.53.0-nightly

### Clap Version

master

### Minimal reproducible code

Hi, I tried to generate on my little project https://github.com/mothsART/pouf autocompletion on zsh.

Cargo build produce this file :
https://github.com/mothsART/pouf/blob/8335f28d5ea9c6a3916a3d5098481e3aaab9f45d/_pouf

and I included with :

```zsh
source _pouf
```

This produce an error like :

```zsh
_arguments:comparguments:312: can only be called from completion function
```

If i manualy delete ```_pouf "$@"```, everything seems correct.

Same issue with 2 opposite version of Zsh : 
Zsh 5.1 and 5.8 (the last one).

Finally, the script suggest me

```zsh
compdef _pouf pouf
```

but it doesn't work.
I replace it with :

```zsh
compdef _pouf pouf
```

### Steps to reproduce the bug with the above code

cargo build

### Actual Behaviour

https://github.com/mothsART/pouf/blob/8335f28d5ea9c6a3916a3d5098481e3aaab9f45d/_pouf

### Expected Behaviour

https://github.com/mothsART/pouf/blob/7e464e21f0a46795c83026511980f7c18de27dbb/_pouf

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @mothsART on 2021-05-13 13:17_

---

_Label `C: generator` added by @pksunkara on 2021-05-13 13:20_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-05-13 13:20_

---

_Comment by @ajeetdsouza on 2021-05-20 02:36_

> Finally, the script suggest me
> 
> ```
> compdef _pouf pouf
> ```
>
> but it doesn't work.
> I replace it with :
> 
> ```
> compdef _pouf pouf
> ```

You've mentioned the same thing twice. Also, I'm not sure what is incorrect about the script, it works fine for me.

![image](https://user-images.githubusercontent.com/1777663/118910384-fa670380-b941-11eb-9ee7-b73b5306e463.png)

---

_Comment by @pksunkara on 2021-05-20 09:27_

It might be a zsh version issue.

---

_Comment by @ajeetdsouza on 2021-05-20 10:51_

Perhaps. I'm running `zsh 5.8`, which appears to be the latest release. 

---

_Comment by @mothsART on 2021-05-20 16:55_

1. I'm using **zsh 5.8** too.

2. the script suggest me (sorry for duplicate) :
```zsh
#compdef pouf
```
and not 

```zsh
#compdef _pouf pouf
```

3. When I see your screenshot @ajeetdsouza, i think you've cloning my github project and checkout on "master" branch, right ?
It's not what I report.

I've report a specific commit. (and the solution in the other hand)
Master branch works actually because i have temporaly monkey patch the build process here : https://github.com/mothsART/pouf/blob/master/build.rs#L20


---

_Comment by @ajeetdsouza on 2021-05-20 17:23_

Could you share how you're sourcing the completion script? I'm using clap v3 completions in [zoxide](https://github.com/ajeetdsouza/zoxide), and they're working fine on zsh. See my completions file [here](https://github.com/ajeetdsouza/zoxide/blob/b8bb6f0f85a5c778c3d9ba08b1434024f24df20f/contrib/completions/_zoxide).

AFAIK, a completion script is sourced by adding the containing folder to `$fpath`:

```zsh
fpath=("$HOME/ws/zoxide/contrib/completions" "${fpath[@]}")
```

---

_Comment by @mothsART on 2021-05-21 10:51_

Like I say on first comment, I used :

```zsh
source "$HOME_pouf
```

fpath doesn't work... I guess beacuse error isn't show.

---

_Comment by @ajeetdsouza on 2021-05-21 11:25_

Have you called `compinit` after setting `fpath`?
```zsh
autoload -Uz compinit && compinit
```
I don't think you're supposed to `source` completions.

---

_Comment by @mothsART on 2021-05-21 11:45_

Hum, right : my autoload is called before my fpath.
Works nice but some documentation would be great : zsh configuration seems not intuitive.

---

_Closed by @mothsART on 2021-05-21 11:45_

---

_Comment by @ajeetdsouza on 2021-05-21 11:47_

Agreed, that's why I created https://github.com/clap-rs/clap/issues/2488.

---

_Referenced in [starship/starship#2806](../../starship/starship/issues/2806.md) on 2021-06-20 15:19_

---

_Referenced in [pimalaya/himalaya#202](../../pimalaya/himalaya/issues/202.md) on 2021-09-16 22:10_

---

_Referenced in [prefix-dev/pixi#338](../../prefix-dev/pixi/issues/338.md) on 2023-09-21 09:16_

---
