```yaml
number: 361
title: Short form alternative to the --files-without-match option
type: issue
state: closed
author: agostonbarna
labels:
  - question
assignees: []
created_at: 2017-02-13T00:32:23Z
updated_at: 2017-05-08T22:35:18Z
url: https://github.com/BurntSushi/ripgrep/issues/361
synced_at: 2026-01-12T16:13:21Z
```

# Short form alternative to the --files-without-match option

---

_@agostonbarna_

Other tools like grep, ack, ag, sift use `-L` as a short form alternative to the `--files-without-match` option.
Unfortunately `-L` is currently defined as an alternative to `--follow` but I think changing it would make ripgrep more consistent with other tools. So I suggest `-L` as a short form of `--files-without-match`, and `-R` as a short form of `--follow` because grep uses this short option for following symbolic links. 

---

_Comment by @BurntSushi on 2017-02-13 16:39_

But `-L` is also consistent with other tools like `find`. Personally, I am very uncomfortable removing flags that are likely widely used at this point, and even more uncomfortable redefining their meaning. I suggest we stop that line of thinking.

I am in principle OK with creating a short option for `--files-without-match`, but it has never seemed deserving of one IMO.

---

_Label `question` added by @BurntSushi on 2017-02-18 17:36_

---

_Comment by @BurntSushi on 2017-03-12 23:54_

Can someone propose a short option for this?

Is this flag really used so frequently that it is deserving of a short option?

---

_Comment by @BurntSushi on 2017-05-08 22:35_

My inclination is to leave this as is, without a short option. If I hadn't already used `-L` (from `find`) to mean "search symlinks," then I'd be happy to add `-L` as a short option for `--files-without-match`. However, `-L` is definitely staying as-is, so we'd have to pick something different for `--files-without-match` and I'm not sure it's worth it to add a non-standard short option for this flag.

---

_Closed by @BurntSushi on 2017-05-08 22:35_

---
