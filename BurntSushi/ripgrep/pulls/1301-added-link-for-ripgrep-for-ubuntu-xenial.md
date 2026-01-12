```yaml
number: 1301
title: " added link for ripgrep for Ubuntu Xenial"
type: pull_request
state: closed
author: rajpratik71
labels: []
assignees: []
base: master
head: master
created_at: 2019-06-16T17:51:11Z
updated_at: 2019-06-18T11:13:08Z
url: https://github.com/BurntSushi/ripgrep/pull/1301
synced_at: 2026-01-12T18:23:13Z
```

#  added link for ripgrep for Ubuntu Xenial

---

_@rajpratik71_

_No description provided._

---

_Review comment by @BurntSushi on `.whitesource`:8 on 2019-06-16 22:08_

Why is this here?

---

_Review comment by @BurntSushi on `README.md`:316 on 2019-06-16 22:09_

Please fix the formatting here. You have whitespace around punctuation and lines exceeding 79 columns.

---

_Review comment by @BurntSushi on `README.md`:310 on 2019-06-16 22:09_

This doesn't read right to me? "... and a Debian package is available in a PPA."

---

_Review comment by @BurntSushi on `README.md`:311 on 2019-06-16 22:09_

Please include *all* commands necessary.

---

_@BurntSushi requested changes on 2019-06-16 22:10_

Thanks, but I have some concerns before merging this.

---

_@rajpratik71 reviewed on 2019-06-18 09:57_

---

_Review comment by @rajpratik71 on `.whitesource`:8 on 2019-06-18 09:57_

it is a configuration file for Whitesource. , "whitesource" is tool which check for vulnerability in  repositories

---

_Review comment by @BurntSushi on `.whitesource`:8 on 2019-06-18 10:42_

Why did you mark this as resolved? I didn't sign off on this. Please remove this from this PR.

I went to their [web site](https://www.whitesourcesoftware.com), and I got immediately spammed with blinking messages. I found no obvious high level description of how it works. I'm immediately skeptical of this tool.

It is poor decorum on your part to try sneaking stuff like this into unrelated PRs. Please don't do it again on any of my repositories.

---

_Review comment by @BurntSushi on `README.md`:310 on 2019-06-18 10:45_

You still have spaces around punctuation, please remove them.

You still have lines exceeding 79 columns. Please fix that.

Please change `Deb` back to `Debian`.

---

_@BurntSushi requested changes on 2019-06-18 10:47_

You've marked several of my comments as resolved, but they aren't actually resolved. Please don't do that. You also haven't addressed all of my feedback. And finally, please remove the "whitesource" crap from this PR. Do not try to sneak things like that into an unrelated PR again. If you seriously want ripgrep to consider it, then open a new issue and make a convincing argument for its utility.

Finally, now that I actually [look at the PPA](https://launchpad.net/~jerem-ferry/+archive/ubuntu/rust), it looks like it is not being updated. It is still offering ripgrep 0.10.0. Therefore, I'm not sure I want to advertise it in the README at all.

---

_Comment by @BurntSushi on 2019-06-18 11:13_

You deleted the whitesource config file... and then added it right back.

There are still other lingering issues, but I'm not interested in doing this back-and-forth with you again. Additionally, since the PPA isn't updated, I don't think it's a good idea to add it to the README.

---

_Closed by @BurntSushi on 2019-06-18 11:13_

---
