```yaml
number: 326
title: "--files flag tries to interpret some arguments as regexes"
type: issue
state: closed
author: n00bmind
labels:
  - question
assignees: []
created_at: 2017-01-16T09:30:59Z
updated_at: 2017-02-18T20:35:57Z
url: https://github.com/BurntSushi/ripgrep/issues/326
synced_at: 2026-01-12T16:13:21Z
```

# --files flag tries to interpret some arguments as regexes

---

_@n00bmind_

Hi.
When trying to integrate your wonderful tool inside vim using 'let g:ctrlp_user_command = ...' I always got an error back because it seems that CtrlP always sends the path in windows format, which rg doesn't like, because apparently it tries to interpret the path as a regex (I checked this on the command line).

Strangely enough, even when setting 'shellslash' to on, the behaviour doesn't change (I'm gonna open an issue with CtrlP about this), so I had to resort to a custom batch script that substitutes the backslashes with forward slashes. Hacky to say the least.. :P

I think backslash support in Windows should be improved. I'm surprised nobody had problems with this before.. I don't know what the best approach would be, though. Maybe a specific switch or marker char that would mean "use this verbatim" or something?

---

_Comment by @BurntSushi on 2017-01-16 10:31_

Thanks for reporting this!

I'll be honest, I have no idea what you're talking about or how to reproduce the problem. ripgrep should have no problems with Windows paths on Windows.

Could you please try to reproduce the issue outside of ctrlp/vim? Also, could you please include other basic information like your ripgrep version and environment? E.g., cygwin or native Windows?

---

_Comment by @n00bmind on 2017-01-16 18:26_

Of course I tried in the command line. In fact that's how I found out / worked around the problem.
If I enter this in my terminal:
`>rg --files .\repo\newrepo`
all I get is "Literal '\n' not allowed".
If I do forward slashes instead I get the expected result.
I'm using ripgrep 0.3.2.

---

_Comment by @BurntSushi on 2017-01-17 15:46_

It sounds like you're trying to type a file path for your regex?

Could you please provide more details about your environment? Are you in cygwin? A native console?

I've certainly used paths like `foo\bar\baz` on Windows before with ripgrep and it works just fine. (I don't have a Windows machine currently available to test. It will have to wait until I get home.)

---

_Comment by @n00bmind on 2017-01-17 16:47_

This is a normal windows console, yes.
I'm merely providing a path as specified in this example from the 'usage' message:
`rg [OPTIONS] --files [<path> ...]`


---

_Comment by @BurntSushi on 2017-01-18 00:42_

I can't reproduce this problem on ripgrep 0.4.0. I can run `rg --files .\some\path` and get the expected output. Note that `rg .\repo\newrepo` shows the error you've reported which is **correct**.

Can you please provide additional information?

---

_Label `question` added by @BurntSushi on 2017-01-18 00:42_

---

_Comment by @ErrEoE on 2017-01-18 07:20_

This is my test result,
![test](https://cloud.githubusercontent.com/assets/14369333/22054636/886d94fa-dd91-11e6-96e3-20fa66c05275.png)


---

_Comment by @n00bmind on 2017-01-18 09:03_

Please note that I also used the '--files' flag, so ripgrep should interpret the rest as a path, isn't that right?
What do yo mean the error is "correct"?


---

_Comment by @BurntSushi on 2017-01-18 11:58_

@chopsueysensei I'm trying to figure out the cause of the bug you're reporting. Sometimes the cause of bugs is that the report is either flawed in some way or I've misunderstood it (i.e., "user error" bugs). I'm trying to rule out that possibility.

I will be very clear. This is expected and correct output, because `.\repo\newrepo` is interpreted as a regex and not a file path:

```
$ rg '.\repo\newrepo'
Literal '\n' not allowed.
```

But this is unexpected and *incorrect* output because `.\repo\newrepo` is seemingly interpreted as a regex when it should be interpreted as a file path:

```
$ rg --files '.\repo\newrepo'
Literal '\n' not allowed.
```

However, I *cannot* reproduce this problem. Given the relative simplicity of the invocation, this led me to try to rule out user error.

I don't have any idea as to what's actually wrong yet. This bug doesn't make any sense to me.

---

_Comment by @BurntSushi on 2017-01-18 12:08_

Interesting. While I couldn't reproduce this on Windows, I can seem to reproduce it on Linux:

```
$ rg --files '.\repo\newrepo'
Literal '\n' not allowed.
```

Weird. I should be able to track down the cause of the problem now.

---

_Comment by @n00bmind on 2017-01-18 13:00_

I understand your motivations, of course. But, as I noted, I was using the '--files' switch so indeed it seems as if it still wanted to interpret what should be a path as a regex instead.
I'm glad you have something to work on, now. Please let me know if there's anything I can try to help you track this down..

---

_Comment by @speedyleion on 2017-01-26 15:07_

A slight workaround if you do want it to work in ctrl-p
```
    let g:ctrlp_user_command = 'rg --files --color never "" %s'
```
This will have a stray `: The system cannot find the path specified. (os error 3)` for the extra quotes but it will still build the list with the correct path.


---

_Comment by @dfbrown on 2017-02-03 21:33_

Another workaround is to add the -F flag so it treats patterns as literal strings instead of as regex. So for ctrl-p on windows I use

    let g:ctrlp_user_command ='rg -F --files %s'


---

_Renamed from "RG inside Ctrlp/vim fails due to backslashes in Windows" to "--files flag tries to interpret some arguments as regexes" by @BurntSushi on 2017-02-18 17:55_

---

_Closed by @BurntSushi on 2017-02-18 20:35_

---

_Comment by @BurntSushi on 2017-02-18 20:35_

@dfbrown Clever hack! This bug should be fixed in the next release. :-)

---
