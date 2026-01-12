```yaml
number: 338
title: "Error parsing regex near 'E:\\Relea' at character offset 3: Unrecognized escape sequence: '\\R'."
type: issue
state: closed
author: jiangjianshan
labels:
  - duplicate
assignees: []
created_at: 2017-01-22T04:26:08Z
updated_at: 2017-02-18T17:53:15Z
url: https://github.com/BurntSushi/ripgrep/issues/338
synced_at: 2026-01-12T16:13:21Z
```

# Error parsing regex near 'E:\Relea' at character offset 3: Unrecognized escape sequence: '\R'.

---

_@jiangjianshan_

Hello,

    I want to combine to use ripgrep and CtrlP together, but meet the following issue as the picure showing:
![image](https://cloud.githubusercontent.com/assets/20179047/22179973/7cdcb74c-e09d-11e6-993a-4ee130481917.png)

    As you see my setting for the plugin CtrlP. I think the issue is came from the below line
let g:ctrlp_user_command = 'rg %s --files --color=never --glob ""'
    The version of ripgrep I'm using is 0.4.0, which is installed from commad 'cargo install ripgrep'. 
    Is it something wrong for my setting for the rg command or any other reasong to cause this issue?

---

_Comment by @BurntSushi on 2017-01-22 05:24_

I think this is a dupe of #326.

---

_Comment by @jiangjianshan on 2017-01-22 06:30_

@BurntSushi, I have look through the #326 issue, it seems like it is still open.  Below is what I'm try on my Windows 7 SP1 64bit system.
![image](https://cloud.githubusercontent.com/assets/20179047/22180601/419c07ac-e0af-11e6-8965-1be19034d3d0.png)

    I have already following the instruction on your github website to build ripgrep.
![image](https://cloud.githubusercontent.com/assets/20179047/22180597/344633d4-e0af-11e6-99c3-1677968beccf.png)


---

_Comment by @BurntSushi on 2017-01-22 14:16_

It is indeed still open. But it still looks like a duplicate to me. :-)

---

_Comment by @jiangjianshan on 2017-01-23 14:49_

@BurntSushi , I have found a new strange behaviour for command "rg --files [path]"
![image](https://cloud.githubusercontent.com/assets/20179047/22208362/9256afe4-e1bd-11e6-954e-202bc9c855b1.png)
   As you see above, the command "rg --files .\doc" worked at the folder "E:\Releases\lua-5.3.3", but "rg --files .\gnu_regex" not work at folder "E:\Releases\ctags-5.8".
   The version of Rust I'm using is 1.14.0, which target is x86_64-pc-windows-msvc. 
   When I'm using the command "cargo install ripgrep", I have noticed that the version of regex to compile ripgrep is 0.2.1 but not the 0.2 on this github.
   I'm still finding the solution for the issue I meet.


---

_Comment by @BurntSushi on 2017-01-23 14:52_

The bug is that the file path appears to be interpreted as a regex. If that's true, then your error messages make sense: `\d` is a valid escape sequence while `\g` is not.

> When I'm using the command "cargo install ripgrep", I have noticed that the version of regex to compile ripgrep is 0.2.1 but not the 0.2 on this github.

ripgrep uses [regex 0.2.1](https://github.com/BurntSushi/ripgrep/blob/master/Cargo.toml#L41).

---

_Comment by @jiangjianshan on 2017-01-24 02:14_

@BurntSushi, so what you mean this bug maybe came from regex 0.2.1?

---

_Comment by @BurntSushi on 2017-01-24 02:17_

No. It's clearly a bug in ripgrep proper. For some reason, the file path is
being parsed as a regex, but it shouldn't be. I haven't had a chance to
investigate further.

On Jan 23, 2017 21:14, "jiangjianshan" <notifications@github.com> wrote:

> @BurntSushi <https://github.com/BurntSushi>, so what you mean this bug
> maybe came from regex 0.2.1?
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/338#issuecomment-274681266>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34jBuz1xkzn6EZOU4ObwOd5cA4-cNks5rVV6VgaJpZM4LqRkw>
> .
>


---

_Label `duplicate` added by @BurntSushi on 2017-02-18 17:53_

---

_Closed by @BurntSushi on 2017-02-18 17:53_

---
