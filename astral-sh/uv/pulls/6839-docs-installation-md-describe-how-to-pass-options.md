```yaml
number: 6839
title: "docs `installation.md`: describe how to pass options to the installer on Linux"
type: pull_request
state: merged
author: ilyagr
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-08-29T22:47:33Z
updated_at: 2024-09-17T16:09:30Z
url: https://github.com/astral-sh/uv/pull/6839
synced_at: 2026-01-10T12:53:35Z
```

# docs `installation.md`: describe how to pass options to the installer on Linux

---

_Pull request opened by @ilyagr on 2024-08-29 22:47_

(This is a suggestion that was easy for me to make a PR for; if other approaches are considered better, feel free to consider this as a FR for those instead)

I'd feel more comfortable using the installer with the instructions in this commit, since I'm uncomfortable with random scripts trying to modify my system config (PATH in this case).

Currently, the installer seems to be the best way to install `uv` that allows updating it on a system without Homebrew or `pipx`. I hope somebody will provide similar instructions for Windows.

I considered recommending saving the script to a file and then running that, but I think it's better to have fewer options in the instructions. Most people who'd want to save the file would figure it out.

As an aside, I would personally appreciate if `uv` could be installed easily with `cargo install` or `cargo binstall`, but a friendly script that acts predictably is probably more useful for more people.


## Test Plan

I tested the command on my machine, but I did not test compiling the docs (yet). If the CI does not compile the docs, I could test this a bit later, or perhaps this would be easier for somebody who already has a dev environment set up.


---

_Assigned to @zanieb by @zanieb on 2024-08-29 23:03_

---

_Comment by @ilyagr on 2024-08-30 19:22_

Thinking some more about my motivations: if `uv self update` worked for downloads from https://github.com/astral-sh/uv/releases, there would be less need for using the installation script at all (though this PR might be useful even in that case).

One more thing I tested is that if I run

     curl -LsSf https://astral.sh/uv/0.3.0/install.sh | CARGO_HOME=~/.local sh -s -- --no-modify-path -v

and then run `uv self update`, it correctly updates the `uv` binary in ~/.local/bin, even though CARGO_HOME is not set during the self-update. This is great!

Given that, getting the regular `uv` download to self-update seems like it should be quite possible (though I might of course be missing something).

---

_Comment by @zanieb on 2024-09-17 16:09_

Thanks!

---

_Label `documentation` added by @zanieb on 2024-09-17 16:09_

---

_Merged by @zanieb on 2024-09-17 16:09_

---

_Closed by @zanieb on 2024-09-17 16:09_

---
