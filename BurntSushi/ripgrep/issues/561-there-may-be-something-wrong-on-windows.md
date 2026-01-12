```yaml
number: 561
title: There may be something wrong on windows.
type: issue
state: closed
author: Yggdroot
labels: []
assignees: []
created_at: 2017-07-20T12:00:05Z
updated_at: 2017-07-20T12:57:53Z
url: https://github.com/BurntSushi/ripgrep/issues/561
synced_at: 2026-01-12T16:13:22Z
```

# There may be something wrong on windows.

---

_@Yggdroot_

`rg --files -g "!.svn"` has output while `rg --files -g '!.svn'` does not have.
The only difference is quote, why? Is it a bug?


---

_Comment by @BurntSushi on 2017-07-20 12:07_

> Is it a bug?

I don't understand Windows quoting rules and your bug report doesn't say which shell you're using, so it's impossible to say.

---

_Comment by @Yggdroot on 2017-07-20 12:18_

I used the cmd.exe of windows.

---

_Comment by @dieggsy on 2017-07-20 12:43_

@Yggdroot https://stackoverflow.com/a/24181667/4992688 tl;dr, cmd.exe doesn't use single quotes. FWIW I believe PowerShell can.

---

_Comment by @Yggdroot on 2017-07-20 12:57_

@therockmandolinist  Got it. Thank you.

---

_Closed by @Yggdroot on 2017-07-20 12:57_

---
