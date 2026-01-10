---
number: 9060
title: "`uv add` git dependency using ssh protocol"
type: issue
state: closed
author: theelderbeever
labels:
  - question
assignees: []
created_at: 2024-11-12T15:24:30Z
updated_at: 2024-11-12T18:17:05Z
url: https://github.com/astral-sh/uv/issues/9060
synced_at: 2026-01-10T01:24:36Z
---

# `uv add` git dependency using ssh protocol

---

_Issue opened by @theelderbeever on 2024-11-12 15:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

The docs [here](https://docs.astral.sh/uv/pip/packages/#installing-a-package) suggest, accurately, that git repo dependencies are able to to be added as a project dependency by `uv add git+https://path.to.project/repo.git` which works correctly. What is the proper syntax for adding via ssh? Our private repos don't allow https. Everything I have tried so far. says the file doesn't end in a known extension or that the the path isn't a repo.

TLDR: How do I `uv add` with an ssh git path instead of https?


---

_Comment by @FishAlchemist on 2024-11-12 15:28_

``git+ssh``?
https://docs.astral.sh/uv/configuration/authentication/#git-authentication

---

_Comment by @theelderbeever on 2024-11-12 15:42_

@FishAlchemist Not sure how I missed that. Regardless that just gives a public key rejection. Which I have this repo checked out via ssh so that shouldn't be the case. Any idea what `uv` uses for the public key?

---

_Comment by @FishAlchemist on 2024-11-12 16:06_

@theelderbeever 
I'm unsure about the specific public key used by UV, but it seems that UV simply calls the Git CLI when working with Git repositories. Therefore, is it possible that your SSH authentication has passed, but you're unable to clone the project using Git over SSH?

I recall that uv switched to using the Git CLI for some reason. I don't think uv is switched back to using an embedded one yet.



---

_Comment by @theelderbeever on 2024-11-12 16:12_

So I think that is the strange thing... I _CAN_ clone the project with the default ssh url that github generates ie: `git@github.com:<organization>/<repository>.git`. But if you try and add any form of `ssh://` to the urls in anyway I will get unauthenticated.

---

_Comment by @FishAlchemist on 2024-11-12 16:43_

@theelderbeever It can be used, but the format will be slightly different from git clone.
For Example https://github.com/navdeep-G/setup.py
*  ``uv add git+ssh://git@github.com/navdeep-G/setup.py.git``
*  ``git clone git@github.com:navdeep-G/setup.py.git``

---

_Label `question` added by @charliermarsh on 2024-11-12 17:48_

---

_Comment by @charliermarsh on 2024-11-12 17:49_

Yeah I think you want `uv add git+ssh://git@github.com/...`. We just call through to the Git CLI, so it's possible that there's something wrong with your Git authentication itself?

---

_Closed by @charliermarsh on 2024-11-12 17:49_

---

_Comment by @theelderbeever on 2024-11-12 18:17_

@charliermarsh After some digging I think I figured it out...

`git+ssh://git@github.com/...` - requires READ and WRITE access on the repo being added
`git clone git@github.com/...` - only needs READ access to the repo

From the standpoint of being able to depend on ssh access only repositories where you may want to maintain fine grained access to the repo this seems like a limitation of `uv`. Any ideas here? I wouldn't necessarily say this is closed unless it should be a different issue instead?

---
