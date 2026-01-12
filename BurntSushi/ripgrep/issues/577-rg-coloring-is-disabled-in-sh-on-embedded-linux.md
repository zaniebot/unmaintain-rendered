```yaml
number: 577
title: "rg coloring is disabled in \"sh\" on embedded Linux(armhf)"
type: issue
state: closed
author: lilianmoraru
labels:
  - wontfix
assignees: []
created_at: 2017-08-18T13:30:19Z
updated_at: 2017-10-22T01:02:41Z
url: https://github.com/BurntSushi/ripgrep/issues/577
synced_at: 2026-01-12T16:13:22Z
```

# rg coloring is disabled in "sh" on embedded Linux(armhf)

---

_@lilianmoraru_

I want to note that I tested on another embedded Linux device with "sh"(another system - different projects) and the `rg` coloring **is working**.
For some reason, on this device it doesn't.

Does `rg` use different backends for coloring?
`clap`'s coloring is working correctly, while `rg`'s coloring is disabled.
Same issue is visible with `exa`, on the same Linux system.

![image](https://user-images.githubusercontent.com/621738/29460229-d243767c-842f-11e7-957c-a652baae552d.png)

`ls --color=auto` also correctly uses colors on this device.

I understand that there is not enough info to rely on, so I am willing to run commands and test different things to identify the problem.

Side note(maybe says something): The target that *does not support* coloring has 0 Available space on root `/` where its home is `/root`. `/root` doesn't have any files and also, because of that, you don't have things like reverse history search(`Ctrl + R`).
On the target that *does support* coloring: it has free space on root, has reverse search, ...

---

_Comment by @BurntSushi on 2017-08-18 13:42_

What does `rg --debug` say? What is the output of `echo $TERM`? The color determination rule is pretty simple when printing to a tty: https://github.com/BurntSushi/ripgrep/blob/3d9acdab189006880e9d4ccdbe9ae42b164901ec/termcolor/src/lib.rs#L136-L145

The tty logic is here: https://github.com/BurntSushi/ripgrep/blob/3d9acdab189006880e9d4ccdbe9ae42b164901ec/src/args.rs#L681-L699

Does `grep --color=auto -r test` show colors?

---

_Comment by @lilianmoraru on 2017-08-18 23:04_

I'll try to come with a response next week(I can test this only at work).

---

_Comment by @lilianmoraru on 2017-08-23 15:34_

`rg test --debug`:
```
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        't',
        'e',
        's',
        't'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }
test.txt
1:test
```

`echo $TERM` does not display anything(it is not set)
`test -z "${TERM}" && echo true || echo false`: `true`

`grep --color=auto` does not work because it's from `busybox` and it doesn't support that flag.
Without `--color=auto`, it does not display red, as it usually does(might be that the `busybox` version doesn't do that in general, but I am not sure about that - I would expect the busybox version to not do any coloring).

At the same time, `ls` is also from `busybox` and if I pass `ls --color=auto`, it does display colors.

---

_Comment by @BurntSushi on 2017-08-23 15:38_

> `echo $TERM` does not display anything(it is not set)

That's your problem. You need to have `TERM` set to a value that isn't `dumb`. AFAIK, that's what GNU grep does. I'm not particularly inclined to change it without very strong motivation.

---

_Label `wontfix` added by @BurntSushi on 2017-10-22 01:01_

---

_Comment by @BurntSushi on 2017-10-22 01:02_

Closing this because I think it's acceptable to require `TERM` to be set. More generally, I specifically designed ripgrep's coloring to basically do what GNU grep does, and I think it requires strong motivation to diverge from that.

---

_Closed by @BurntSushi on 2017-10-22 01:02_

---
