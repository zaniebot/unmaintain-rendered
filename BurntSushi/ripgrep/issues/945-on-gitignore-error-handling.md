```yaml
number: 945
title: On .gitignore error handling
type: issue
state: closed
author: gcp
labels:
  - duplicate
assignees: []
created_at: 2018-06-12T23:19:51Z
updated_at: 2025-04-27T08:48:51Z
url: https://github.com/BurntSushi/ripgrep/issues/945
synced_at: 2026-01-12T16:13:22Z
```

# On .gitignore error handling

---

_@gcp_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### How did you install ripgrep?

cargo

#### What operating system are you using ripgrep on?

Linux (Ubuntu 16.04)

#### Describe your question, feature request, or bug.

Using ripgrep over the entire Android AOSP tree produces quite a few errors parsing .gitignore files. There's 2 concerns with this: 1) when using ripgrep, I don't really care about its opinion on the .gitignore files of another project 2) I'm assuming this means there's a behavior difference between real git and ripgrep's gitignore parsing.

Examples:

```
./external/glide/.gitignore: line 24: error parsing glob '**local.properties': invalid use of **; must be one path component

./external/chromium-trace/catapult/telemetry/bin/.gitignore: line 2: error parsing glob '!**.sha1': invalid use of **; must be one path component
```

I'm not sure what the correct solution here should be, but these error messages are not useful and obscure the real output.

---

_Comment by @okdana on 2018-06-12 23:33_

You can silence the error messages with the `--no-messages` flag. To make the effect permanent you can add the option either to a shell alias or to your [config file](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file).

As far as behaviour differences, it does appear that git (2.17.1) accepts both of those patterns (confirmed via `git status` and `git check-ignore`), even though [the documentation](https://git-scm.com/docs/gitignore) explicitly states that `**` is invalid unless preceded and/or followed by a slash. 🤷‍♀️

---

_Comment by @BurntSushi on 2018-06-12 23:40_

This same issue has been filed several times:  #373, #646, #507, #859. The first one in particular contains the most discussion.

Current master has a [`--no-ignore-messages`](https://github.com/BurntSushi/ripgrep/commit/0ee0b160b583715b2c4629978695f3567cb6fc09) flag, which suppresses exactly these error messages.

---

_Closed by @BurntSushi on 2018-06-12 23:40_

---

_Label `duplicate` added by @BurntSushi on 2018-06-12 23:40_

---

_Comment by @Pateo-zhengfang on 2025-04-27 08:48_

Did you successfully port ripgrep to AOSP?

---
