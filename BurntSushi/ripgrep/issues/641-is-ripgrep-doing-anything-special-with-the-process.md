```yaml
number: 641
title: Is ripgrep doing anything special with the process?
type: issue
state: closed
author: borekb
labels:
  - duplicate
assignees: []
created_at: 2017-10-15T15:22:15Z
updated_at: 2017-10-15T15:42:01Z
url: https://github.com/BurntSushi/ripgrep/issues/641
synced_at: 2026-01-12T16:13:22Z
```

# Is ripgrep doing anything special with the process?

---

_@borekb_

Sorry for a generic question but I'm seeing a strange behavior when trying to invoke `rg` from my Node.js app: child_process.spawn doesn't emit any events https://github.com/nodejs/help/issues/888

What's strange is that the command I'm trying to spawn works fine in the console (both `cmd.exe` and Git Bash) and also spawning something like `rg -h` works fine; only performing real searches hangs the whole app. It might be a Node.js issue but I also wanted to ask here if `rg` doesn't do anything special when providing output and exiting itself?

Thanks and BTW, the speed I'm seeing is just incredible, ripgrep rocks!

---

_Comment by @BurntSushi on 2017-10-15 15:29_

This is probably a duplicate of: #410. TL;DR: Your process is waiting to read something on stdin. To force the issue:

> For the best results, I think you probably want to set your CWD to `<path to search>` and then use `./` as the path. (This will let you work around #278, which is probably unlikely to come up anyway.)

---

_Label `duplicate` added by @BurntSushi on 2017-10-15 15:29_

---

_Comment by @borekb on 2017-10-15 15:41_

Works like a charm, thanks!

---

_Closed by @borekb on 2017-10-15 15:42_

---
