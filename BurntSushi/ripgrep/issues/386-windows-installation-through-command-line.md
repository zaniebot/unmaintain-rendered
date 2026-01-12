```yaml
number: 386
title: Windows installation through command-line
type: issue
state: closed
author: Fireforge
labels: []
assignees: []
created_at: 2017-02-28T18:49:30Z
updated_at: 2017-02-28T19:10:06Z
url: https://github.com/BurntSushi/ripgrep/issues/386
synced_at: 2026-01-12T16:13:21Z
```

# Windows installation through command-line

---

_@Fireforge_

You seem to have command line installation supported for every platform except Windows. Command-line installation on Windows is not yet common place, but it is out there:

- Scoop.sh: https://github.com/lukesampson/scoop
- Chocolatey: https://chocolatey.org/

Would you consider supporting one or both of these installation methods? The community (such as myself) could try to add them, but if you already officially support all the other platforms then hopefully it's not to hard to include these as well.



---

_Comment by @BurntSushi on 2017-02-28 18:54_

I don't personally support anything. (Well, I do have a brew tap associated with this repo, but that's temporary and for folks that want the SIMD accelerated version of ripgrep on Mac.)

I don't personally have any intention of fiddling with Windows package repositories. I just don't have the time to maintain that. It does however seem like ripgrep is already in Chocolatey according to #342.

Otherwise, I think this is a dupe of #10 and #342.

---

_Closed by @BurntSushi on 2017-02-28 18:54_

---

_Comment by @Fireforge on 2017-02-28 19:06_

Okay, I understand. It's all community support then I guess.

Thanks for the Chocolatey repo link (https://chocolatey.org/packages/ripgrep), it's up-to-date. I thought I had searched for ripgrep there, I don't know why that didn't show up. I could add that link to your Readme along with the others in a PR if you like.


---

_Comment by @BurntSushi on 2017-02-28 19:10_

> I could add that link to your Readme along with the others in a PR if you like.

That would be fantastic, thank you. :-) If it could include the basic install instructions like the others that'd be great too!

---
