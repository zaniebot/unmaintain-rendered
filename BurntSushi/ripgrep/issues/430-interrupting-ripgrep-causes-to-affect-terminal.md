```yaml
number: 430
title: Interrupting ripgrep causes to affect terminal colors. Windows
type: issue
state: closed
author: DoumanAsh
labels: []
assignees: []
created_at: 2017-03-31T05:27:22Z
updated_at: 2017-03-31T10:48:22Z
url: https://github.com/BurntSushi/ripgrep/issues/430
synced_at: 2026-01-12T16:13:22Z
```

# Interrupting ripgrep causes to affect terminal colors. Windows

---

_@DoumanAsh_

I'm not sure how exactly coloring is made on Windows but you can `Crtl-C` ripgrep and it gets your console like that:

http://i.imgur.com/PR0wrCe.png

All non-colored fonts became red for example.
It can happen with any color that ripgrep is using just interrupt it in the moment when coloring starts...

P.s. workaround for powershell:
```
[Console]::ResetColor()
```

---

_Comment by @DoumanAsh on 2017-03-31 05:36_

Ok, sorry i haven't looked properly and found https://github.com/BurntSushi/ripgrep/commit/de5cb7d22e00c18dfce082a68b0cf45924b56e7f

But no, it is bad. Really bad for user.
You cannot expect user to do clean-up after software.

---

_Comment by @BurntSushi on 2017-03-31 09:17_

I reopened #281. Please use that and suggest a path forward. I don't need to be told that it is not desirable. I need help to figure out how to fix it.

---

_Closed by @BurntSushi on 2017-03-31 09:17_

---

_Comment by @DoumanAsh on 2017-03-31 10:48_

Ok, i'm sorry i didn't mean to be rude. But i was surprised to see that issue is closed while bad fix has been reverted.
I'm planning to look into possible ways to deal with it.

---
