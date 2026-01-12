```yaml
number: 201
title: (recommended?) emacs mode
type: issue
state: closed
author: stapelberg
labels:
  - question
assignees: []
created_at: 2016-10-29T16:13:36Z
updated_at: 2019-05-22T18:13:33Z
url: https://github.com/BurntSushi/ripgrep/issues/201
synced_at: 2026-01-12T16:13:21Z
```

# (recommended?) emacs mode

---

_@stapelberg_

I know of the following ripgrep modes for emacs:
- [rg-el](https://github.com/xuchunyang/rg-el)
- [ripgrep.el](https://github.com/nlamirault/ripgrep.el)

AFAICT, neither of them allows navigating through the search results using the [compilation mode](https://www.gnu.org/software/emacs/manual/html_node/emacs/Compilation-Mode.html) `M-g n` and `M-g p` key bindings.

[ag.el](https://github.com/Wilfred/ag.el) and the now-abandoned [ack-and-a-half](https://github.com/bbatsov/projectile/issues/584) both supports this feature, so I’m used to the convenience from other grep modes :).

Is there a more fully featured emacs mode for ripgrep, which supports compilation mode key bindings?


---

_Comment by @BurntSushi on 2016-10-29 16:14_

I'm not an emacs user, so I can't answer. cc @nikomatsakis I think you have something like this right?


---

_Label `question` added by @BurntSushi on 2016-10-30 01:06_

---

_Comment by @avdv on 2016-11-02 10:25_

`helm` has added support for ripgrep lately. You might want to give it a spin.

If you're used to `ag.el`, [here](https://gist.github.com/avdv/2ee9a382cee297f2a3d7081e7a818ea2) is a small wrapper script for `rg` which makes it "behave" like `ag`. I'm using that currently with [`swiper`](https://github.com/abo-abo/swiper) which lacks support for ripgrep currently.


---

_Comment by @BurntSushi on 2016-11-19 14:33_

I don't know if this question has actually been answered, but I don't think I can answer it.

I would be happy to start accumulating a list of editor plugins in the README.


---

_Closed by @BurntSushi on 2016-11-19 14:33_

---

_Comment by @manuel-uberti on 2016-11-19 14:35_

I recentely added support for `rg` in Ivy/Counsel: `counsel-rg`. Just wait for this PR to be merged to have a silly bug fixed: https://github.com/abo-abo/swiper/pull/785


---

_Comment by @nikomatsakis on 2016-11-19 17:42_

I do have a mode myself, but I've not cleaned it up and posted it publicly. I based it on `ack-and-a-half.el` -- basically did some search-and-replace :)


---

_Comment by @dajva on 2016-12-20 19:25_

I recently made [dajva/rg.el](https://github.com/dajva/rg.el) for my personal usage. It's essentially a port of emacs rgrep for ripgrep and works very similar. It's even possible to use wgrep in the result buffer.

---

_Comment by @Wilfred on 2019-05-22 18:13_

Apologies for posting on this old thread, but there's also https://github.com/Wilfred/deadgrep if you want a custom ripgrep frontend for Emacs.

---
