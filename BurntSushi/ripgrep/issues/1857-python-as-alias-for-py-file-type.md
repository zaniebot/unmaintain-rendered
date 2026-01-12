```yaml
number: 1857
title: python as alias for py file type
type: issue
state: closed
author: bluss
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2021-04-23T08:40:08Z
updated_at: 2023-07-08T22:53:08Z
url: https://github.com/BurntSushi/ripgrep/issues/1857
synced_at: 2026-01-12T16:13:24Z
```

# python as alias for py file type

---

_@bluss_

While I know type aliases would maybe best be avoided, maybe they can smooth out the user interface.

Currently there is a bit of inconsistency:

- `-t rust` for *.rs files
- `-t py` for *.py files

Naturally I'd reach for `-t python` if I didn't double check the type list - allowing both python and py to mean the same file type would smooth this out. I'm not sure how to avoid an explosion of aliases, but allowing the "full name" like python seems like a good idea on its own.

---

_Comment by @BurntSushi on 2021-04-24 13:27_

Yeah, I'm a little torn on this. I previously didn't want to do this because I didn't want to create a precedent of aliases. But maybe adding aliases isn't such a big deal.

FWIW, the reason why we have `rust` and `py` is purely because of length.

---

_Label `enhancement` added by @BurntSushi on 2021-04-24 13:27_

---

_Comment by @BurntSushi on 2021-04-24 13:29_

I think in order for this to happen, I would want a "proper" way to define aliases without listing the glob rules for each alias. If someone wants to work on this, that would be lovely. A good start might be to propose a straw man implementation in this issue before submitting a PR. (But if you want to jump straight to a PR, that's okay too.)

---

_Label `help wanted` added by @BurntSushi on 2021-04-24 13:29_

---

_Comment by @EduardoLaranjo on 2021-06-01 13:28_

hi, I would be interested in working on this, although I'm new to the project

---

_Comment by @BurntSushi on 2021-06-01 13:33_

@EduardoLaranjo Hi! Unfortunately I don't quite have the bandwidth to mentor, even though I would generally love to do that.

I think the big idea here is that we want to be able to say that "`py` is an alias of `python`", but _without_ having to specify the globs twice.

I haven't looked at the ignore/file-type code in quite a while, so I don't have an implementation path in mind off the top of my head unfortunately.

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
