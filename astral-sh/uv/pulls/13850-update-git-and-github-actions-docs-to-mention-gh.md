```yaml
number: 13850
title: "update Git and GitHub Actions docs to mention `gh auth login`"
type: pull_request
state: merged
author: oconnor663
labels: []
assignees: []
merged: true
base: main
head: jack/github_auth_docs
created_at: 2025-06-04T21:56:23Z
updated_at: 2025-06-06T17:54:39Z
url: https://github.com/astral-sh/uv/pull/13850
synced_at: 2026-01-12T16:10:53Z
```

# update Git and GitHub Actions docs to mention `gh auth login`

---

_@oconnor663_

This came up in https://github.com/astral-sh/uv/issues/11342#issuecomment-2938352842 and https://github.com/astral-sh/uv/issues/12368#issuecomment-2747900465. I'm leaving out the maximally complicated `GIT_CONFIG_COUNT`-based approach that I mentioned in https://github.com/astral-sh/uv/issues/11342#issuecomment-2938352842, but let me know if anyone things it would be more helpful than confusing to mention it. (In any case, all of this might change in the future if we add more support for Git credentials?)

---

_Review comment by @Gankra on `docs/configuration/authentication.md`:34 on 2025-06-04 22:35_

...It seems weird to have a sidebar that then says "as follows"

---

_Review comment by @Gankra on `docs/configuration/authentication.md`:34 on 2025-06-04 22:41_

...should we be recommending these formats above if they're kinda broken like this?

---

_@Gankra approved on 2025-06-04 22:42_

Can't speak to the precise wording but this is a big improvement, great job figuring this wizardry out!

---

_Assigned to @oconnor663 by @oconnor663 on 2025-06-05 00:13_

---

_Unassigned @oconnor663 by @oconnor663 on 2025-06-05 00:13_

---

_Assigned to @zanieb by @oconnor663 on 2025-06-05 00:13_

---

_@zanieb reviewed on 2025-06-05 16:22_

---

_Review comment by @zanieb on `docs/configuration/authentication.md`:34 on 2025-06-05 16:22_

> ...should we be recommending these formats above if they're kinda broken like this?

We talked about this in Discord, those formats are totally valid for `uv pip install` and other local usage.

---

_@zanieb reviewed on 2025-06-05 16:27_

---

_Review comment by @zanieb on `docs/configuration/authentication.md`:25 on 2025-06-05 16:27_

Aside

> If a username is provided without credentials, you will be prompted to enter them.

This isn't even true anymore? Should we remove that in a separate pull request?


---

_@oconnor663 reviewed on 2025-06-05 16:52_

---

_Review comment by @oconnor663 on `docs/configuration/authentication.md`:25 on 2025-06-05 16:52_

I went ahead and added it here: cad2d1a05

---

_Review comment by @oconnor663 on `docs/configuration/authentication.md`:36 on 2025-06-05 17:47_

@zanieb this doesn't seem to happen when I try it with a test user on my machine (this is a demo PAT that I don't care about leaking):

```
$ gh auth setup-git --force --hostname github.com
$ cat ~/.gitconfig 
[credential "https://github.com"]
        helper = 
        helper = !/usr/bin/gh auth git-credential
[credential "https://gist.github.com"]
        helper = 
        helper = !/usr/bin/gh auth git-credential
$ ls ~/.config/gh
ls: cannot access '/home/test/.config/gh': No such file or directory
$ uv add 'git+https://git:[REDACTED]@github.com/oconnor663/private_a'
...
Installed 1 package in 1ms
 + foo==0.1.0 (from git+https://github.com/oconnor663/private_a@16a893d62cbf0d58546b3440cc8a78f1e49de01f)
$ rm uv.lock
$ uv sync
   Updating https://github.com/oconnor663/private_a (HEAD)                                                                                         
  × Failed to download and build `foo @ git+https://github.com/oconnor663/private_a`
  ├─▶ Git operation failed
  ├─▶ failed to fetch into: /home/test/.cache/uv/git-v0/db/8d5a0ce3cccf1bae
  ╰─▶ process didn't exit successfully: `/usr/bin/git fetch --force --update-head-ok 'https://github.com/oconnor663/private_a'
      '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
      --- stderr
      fatal: could not read Username for 'https://github.com': terminal prompts disabled
$ ls ~/.config/gh
ls: cannot access '/home/test/.config/gh': No such file or directory
```

Is that expected?

---

_@oconnor663 reviewed on 2025-06-05 17:47_

---

_@zanieb reviewed on 2025-06-05 20:28_

---

_Review comment by @zanieb on `docs/configuration/authentication.md`:36 on 2025-06-05 20:28_

That's the `gh` credential helper, something like `git-credential-osxkeychain` will automatically persist used credentials into the keychain.

---

_Merged by @oconnor663 on 2025-06-05 20:45_

---

_Closed by @oconnor663 on 2025-06-05 20:45_

---

_Branch deleted on 2025-06-05 20:45_

---

_Review comment by @oconnor663 on `docs/configuration/authentication.md`:36 on 2025-06-06 17:54_

> (this is a demo PAT that I don't care about leaking)

ARG

![image](https://github.com/user-attachments/assets/9169eab3-583d-4916-a40c-e3547e48cc78)

I get it, but ARG.

---

_@oconnor663 reviewed on 2025-06-06 17:54_

---
