```yaml
number: 740
title: "Add support for `--passthr{u,ough}` flag"
type: issue
state: closed
author: zx8
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2018-01-09T09:40:06Z
updated_at: 2018-06-05T00:58:59Z
url: https://github.com/BurntSushi/ripgrep/issues/740
synced_at: 2026-01-12T16:13:22Z
```

# Add support for `--passthr{u,ough}` flag

---

_@zx8_

After reading [this comment](https://news.ycombinator.com/item?id=16097948) on HN, I thought it'd be neat to add support for the `--passthrough` / `--passthru` flag in ripgrep as well.

Thoughts?
  

---

_Comment by @okdana on 2018-01-09 09:45_

I was thinking about that myself. I personally think it's easier to do it the way i mentioned in that thread, but it seems like some people are quite passionate about it, and it would be very trivial to add a flag that wraps the pattern in `^|(?:...)`, the way we do with `-w` and `-x`. (At least, that's the method that first comes to mind, because it's easy and i have experience with it.)

---

_Comment by @BurntSushi on 2018-01-09 12:08_

I'm kind of meh on it, but I don't feel strongly enough to reject it outright.

`ag` spells it `--passthrough` while `ack` spells it `--passthru`. How should ripgrep spell it? o_0 I suppose we could just accept both.

I like @okdana's suggested implementation.
  

---

_Label `enhancement` added by @BurntSushi on 2018-01-09 12:08_

---

_Label `help wanted` added by @BurntSushi on 2018-01-09 12:08_

---

_Comment by @okdana on 2018-01-09 15:53_

If it were me i think i'd make it `--passthru`, with `--passthrough` as like a hidden alias. Can't recall if Clap supports that tho

---

_Comment by @kbknapp on 2018-01-09 18:15_

> Can't recall if Clap supports that tho

[It does](https://docs.rs/clap/2.29.0/clap/struct.Arg.html#method.alias) :wink: (You can also [make it visible](https://docs.rs/clap/2.29.0/clap/struct.Arg.html#method.visible_alias) so it's findable in the help output)
  

---

_Closed by @BurntSushi on 2018-01-11 23:45_

---

_Comment by @BurntSushi on 2018-06-05 00:51_

@jzinn both are supported. Ripgrep has no specific goal to be "formal."

Please drop this. It's a waste of time.

---
