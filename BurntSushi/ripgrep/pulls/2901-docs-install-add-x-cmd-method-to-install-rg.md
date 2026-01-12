```yaml
number: 2901
title: "docs(install): add x-cmd method to install rg"
type: pull_request
state: closed
author: lunrenyi
labels: []
assignees: []
base: master
head: patch-1
created_at: 2024-09-25T09:33:33Z
updated_at: 2024-09-25T13:52:28Z
url: https://github.com/BurntSushi/ripgrep/pull/2901
synced_at: 2026-01-12T18:23:14Z
```

# docs(install): add x-cmd method to install rg

---

_@lunrenyi_

- Hi, [x-cmd](https://www.x-cmd.com/) is a **toolbox for Posix Shell**, offering a lightweight package manager built using shell and awk. It helps you download rg release packages from the internet and extract them into a unified directory for management, without requiring root permissions.

- **I mean, can the installation method provided by x-cmd be added to the rg installation.md?**[The installation method for the x command](https://www.x-cmd.com/start/).
  ```sh
  x env use rg
  # or
  x rg
  ```

- We wrote a [rg introduction article](https://www.x-cmd.com/pkg/rg) and a [demo](https://www.x-cmd.com/1min/rg)


---

_Comment by @BurntSushi on 2024-09-25 12:45_

When adding new installation instructions, I either need to be familiar with the tool or it needs to be clearly trustworthy. The latter can be met by clear widespread adoption.

I looked into x-cmd and how it works is not clear to me. There are at least some yellow flags I see:

* There are no commit messages whatsoever.
* PRs from outside contributors seem to be rejected.
* I couldn't find the mechanism, either in the code or in the documentation, where by the tool downloads packages to install. Is it downloading from github? Building from source? 
* Reading through the docs, most prose is non-specific and somewhat vague. This doesn't inspire confidence.
* After searching, I could not find anyone else using it.

Because of those yellow flags, I'm not comfortable adding it ripgrep's installation instructions at this time.

---

_Closed by @BurntSushi on 2024-09-25 12:45_

---

_Comment by @BurntSushi on 2024-09-25 12:49_

I do see that `x-cmd` was added as installation instructions to eza: https://github.com/eza-community/eza/pull/1153

But it's not clear if the eza folks did due diligence. cc @cafkafk 

---

_Comment by @edwinjhlee on 2024-09-25 13:52_

Hi ~ I am the author of x-cmd.

Thanks for your attention. And I admire your due diligence.
I will add more documentations in the future.

---

We repackage the rg binary build, and the x-cmd pkg download the rg bianry from here:
https://github.com/x-cmd-build/rg/releases

We have a metadata file for sha512sum checking about the integrity of package.

We also have the integreity check of x-cmd source code:
https://www.x-cmd.com/start/integrity-guarantee

In China, we use npm to deliver the package because github is blocked.

---

X-CMD is not conmunity-driven. I have established a small full-time team to exclusively focus on its development.
However, all of the source code are open for maximum transparency. 

---

We not only provide rg package, we also provided an **experimental** shell wrapper for rg -- rg module.

This is the demo of x-cmd rg module: 
https://www.x-cmd.com/mod/rg

This is the code of x-cmd rg module:
https://github.com/x-cmd/x-cmd/blob/main/mod/rg/lib/main

---

In the future, I plan to add more rg shell wrappers in rg module. It is so much fun, like we do it on jq: https://www.x-cmd.com/mod/jq

---

X-CMD is not well known to the public. But now we are progressing in China, mostly because Chinese is our first language, and we are not good at English communication.

We will fire an issue or pr later once we achieve clear widespread adoption.

---

Thank you for creating such a great tool.














---
