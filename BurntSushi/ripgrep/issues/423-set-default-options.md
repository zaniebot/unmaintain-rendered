```yaml
number: 423
title: Set default options
type: issue
state: closed
author: sgraham
labels:
  - duplicate
assignees: []
created_at: 2017-03-28T19:13:10Z
updated_at: 2017-03-28T19:52:21Z
url: https://github.com/BurntSushi/ripgrep/issues/423
synced_at: 2026-01-12T16:13:22Z
```

# Set default options

---

_@sgraham_

I prefer --no-heading as the default for copy-paste-ability.

On Mac/Linux I can just add a wrapper script that adds --no-heading, but on Windows a wrapper .bat to set default options causes the terrifically annoying "Terminate batch file (Y/N)?" prompt when hitting Ctrl-C so it's not optimal.

Is there any way to set some default options (environment variable, rc file, etc.)?

---

_Comment by @BurntSushi on 2017-03-28 19:52_

> Is there any way to set some default options (environment variable, rc file, etc.)?

No. That's the subject of #196, so I'm going to close this as a duplicate.

> but on Windows a wrapper .bat to set default options causes the terrifically annoying "Terminate batch file (Y/N)?" prompt when hitting Ctrl-C so it's not optimal.

This is a critical piece of motivation for supporting config files. Could you share your experience in #196? In particular, are `bat` files the right way to write wrapper scripts in Windows? Is there a way to create aliases? (Please respond in #196, not here. :-))

---

_Closed by @BurntSushi on 2017-03-28 19:52_

---

_Label `duplicate` added by @BurntSushi on 2017-03-28 19:52_

---
