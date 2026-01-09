---
number: 1598
title: Allow hyphen-less options
type: issue
state: closed
author: clarfonthey
labels:
  - C-enhancement
  - S-waiting-on-decision
assignees: []
created_at: 2019-11-11T02:22:56Z
updated_at: 2020-09-29T08:24:00Z
url: https://github.com/clap-rs/clap/issues/1598
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow hyphen-less options

---

_Issue opened by @clarfonthey on 2019-11-11 02:22_

(This is specifically for clap v2, but I assume that it also applies to v3.)

Essentially, allow positional arguments to be interpreted as options. For my specific use case, I'd like to be able to do something like:

```
cmd clean [unused] [old] [uninstalled]
```

And have it be similar to:

```
cmd clean --unused --old --uninstalled
```

It'd process very similarly to existing long options, just, not have any hyphens. Essentially, it'd be an easy way to distinguish special argument values without having to parse them yourself.

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 11:18_

---

_Label `W: maybe` added by @CreepySkeleton on 2020-02-01 11:18_

---

_Comment by @pksunkara on 2020-03-14 14:08_

I don't think we should be doing anything here. This is a very specific use case that can be done by your CLI using the tools we provide (positional, matches, etc..). I would vote to close this.

---

_Closed by @CreepySkeleton on 2020-03-14 16:03_

---

_Comment by @jabedude on 2020-09-29 02:01_

Is there any chance to reopen this? I'm interested in replicating `dd`'s option syntax (e.g. `dd if=/dev/urandom of=/dev/sda bs=4k`) with clap.

---

_Comment by @pksunkara on 2020-09-29 08:24_

Interest is noted. And I concur that it would be a valid use case. But I would still want to wait for more interest.

---
