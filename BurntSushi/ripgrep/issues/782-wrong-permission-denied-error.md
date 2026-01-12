```yaml
number: 782
title: "Wrong \"Permission denied\" error"
type: issue
state: closed
author: richarddwang
labels:
  - question
  - wontfix
assignees: []
created_at: 2018-02-08T08:53:35Z
updated_at: 2018-02-22T21:56:45Z
url: https://github.com/BurntSushi/ripgrep/issues/782
synced_at: 2026-01-12T16:13:22Z
```

# Wrong "Permission denied" error

---

_@richarddwang_

#### What version of ripgrep are you using?

repgrep 0.7.1

#### What operating system are you using ripgrep on?

Linux Mint 18.1 (Ubuntu 16.04)

#### What I did, What I got, What I expected
Briefly, I get permission denied,
then I reinstall ripgrep with `--classic` and the error disappeared.
![image](https://user-images.githubusercontent.com/17963619/35963377-7df0e552-0cef-11e8-829c-5be81724a4b0.png)
I tested rg with my emacs config directory.
https://github.com/frankie8518/My_Emacs_Configuration
I got the same error in the other place, though.


---

_Label `question` added by @BurntSushi on 2018-02-08 12:25_

---

_Label `wontfix` added by @BurntSushi on 2018-02-08 12:25_

---

_Comment by @BurntSushi on 2018-02-08 12:25_

This doesn't seem like a ripgrep problem? Sounds like a `snap` problem. I don't know enough about `snap` to know whether this indicates something wrong in the snap package for ripgrep, or if it's just a limitation of `snap`.

---

_Closed by @BurntSushi on 2018-02-08 12:25_

---

_Comment by @nathan-whitaker on 2018-02-22 19:24_

I ran into the same issue, it's not a ripgrep problem. snap installs things in a sandboxed mode by default, which is causing the errors. 

You might avoid having some issues opened if you updated the readme to suggest passing the '--classic' flag during the install. 


---

_Comment by @BurntSushi on 2018-02-22 21:56_

Weird. I installed the `rg` snap on my Ubuntu 17.10 box and it worked without needing the `--classic` flag. Either way, I added a note about it to the README. Thanks!

---
