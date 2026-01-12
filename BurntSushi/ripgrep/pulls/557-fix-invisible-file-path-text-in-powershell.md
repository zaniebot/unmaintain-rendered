```yaml
number: 557
title: Fix invisible file path text in PowerShell
type: pull_request
state: merged
author: latkin
labels: []
assignees: []
merged: true
base: master
head: latkin-powershell-color-fix
created_at: 2017-07-16T15:54:00Z
updated_at: 2017-07-17T22:01:21Z
url: https://github.com/BurntSushi/ripgrep/pull/557
synced_at: 2026-01-12T18:23:13Z
```

# Fix invisible file path text in PowerShell

---

_@latkin_

Fix for https://github.com/BurntSushi/ripgrep/issues/342

Root cause is that the standard Windows PowerShell launch shortcut sets the console background color to "dark magenta", then re-defines "dark magenta" as RGB(1, 36, 86) aka "powershell noble blue." Thus if a console app writes output with "dark magenta" foreground color, it will be invisible in a default PowerShell console.

Fix is to do a simple, Windows-only check for a "dark magenta" background at startup, and in this case change path output style to "intense magenta," which will render fine.

Result:

![image](https://user-images.githubusercontent.com/5943573/28249015-2bcd8166-6a03-11e7-9990-c825daa12a0d.png)

Potential concerns:
  - Arg parsing now has to know about Windows console. That's not great but it was simplest path forward for me as a rust n00b.
  - This will add "intense" styling to path output even when user has manually overridden the color themselves. Additional `path:style:nointense` will be needed to counteract this change.
    - Seems not terribly burdensome. By definition this will only affect users who are already comfortable overriding color settings.

---

_Review comment by @BurntSushi on `wincolor/src/win.rs`:100 on 2017-07-17 12:58_

I'd rather not rename these methods, since that would be a breaking change.

---

_@BurntSushi reviewed on 2017-07-17 13:01_

@latkin Thanks for doing this!

I'm not sure if this is the path I necessarily want to go down. In particular, what do you think about just changing the default colors on Windows and removing the conditional check altogether? That way, we don't interfere with any specific user settings but we avoid the "text is invisible" problem by default.

Tangentially, I did a lot of work to avoid making `wincolor` a direct dependency on ripgrep in an effort to hide that layer of abstraction. I'll bow to the alter of pragmatism if necessary, but I think I'd rather see if we can get away with just changing the defaults for now.

---

_Comment by @latkin on 2017-07-17 20:22_

@BurntSushi No problem, I understand your concerns. If you are ok with simply changing the Windows defaults, then that's even easier :-) I went with Cyan for now.

PR udpated. New behavior:

![image](https://user-images.githubusercontent.com/5943573/28288509-41f4a8b0-6af4-11e7-9639-2bd553247b8e.png)




---

_Merged by @BurntSushi on 2017-07-17 22:01_

---

_Closed by @BurntSushi on 2017-07-17 22:01_

---

_Comment by @BurntSushi on 2017-07-17 22:01_

Fantastic, thank you!


---
