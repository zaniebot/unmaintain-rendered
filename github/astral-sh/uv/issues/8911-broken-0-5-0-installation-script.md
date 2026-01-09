---
number: 8911
title: Broken 0.5.0 installation script
type: issue
state: closed
author: apellex
labels: []
assignees: []
created_at: 2024-11-08T00:49:00Z
updated_at: 2024-11-08T01:09:03Z
url: https://github.com/astral-sh/uv/issues/8911
synced_at: 2026-01-07T13:12:18-06:00
---

# Broken 0.5.0 installation script

---

_Issue opened by @apellex on 2024-11-08 00:49_

This is a step from BentoML base image. Prior to 0.5.0 release (which was about 1.5h ago), it completes with no issue with uv version 0.4.30. Was there any change in the install script that might have cause it to fail here? Curious if anyone else encounter this in their pipeline

```
#11 [base-container  4/26] RUN curl -LO https://astral.sh/uv/install.sh &&     sh install.sh && rm install.sh && mv $HOME/.cargo/bin/uv /usr/local/bin/
#11 0.165   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
#11 0.165                                  Dload  Upload   Total   Spent    Left  Speed
#11 0.165 
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100   167  100   167    0     0   1018      0 --:--:-- --:--:-- --:--:--  1018
#11 0.356 
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
#11 0.363 
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
#11 0.433 
100 53510  100 53510    0     0   194k      0 --:--:-- --:--:-- --:--:--  194k
#11 0.459 downloading uv 0.5.0 x86_64-unknown-linux-gnu
#11 0.855 installing to /root/.local/bin
#11 0.861   uv
#11 0.864   uvx
#11 0.864 everything's installed!
#11 0.888 
#11 0.888 To add $HOME/.local/bin to your PATH, either restart your shell or run:
#11 0.888 
#11 0.888     source $HOME/.local/bin/env (sh, bash, zsh)
#11 0.888     source $HOME/.local/bin/env.fish (fish)
#11 0.896 mv: cannot stat '/root/.cargo/bin/uv': No such file or directory
```


---

_Comment by @charliermarsh on 2024-11-08 00:53_

Take a look at the release notes: https://github.com/astral-sh/uv/releases/tag/0.5.0. We now install to `~/.local/bin` by default instead of `~/.cargo/bin`. So you'd need to change your `mv $HOME/.cargo/bin/uv /usr/local/bin/` step.

---

_Comment by @apellex on 2024-11-08 01:05_

Thanks @charliermarsh. This is a breaking change, may I ask why only a minor version bump?

---

_Comment by @charliermarsh on 2024-11-08 01:06_

Pre-1.0, we use minor version bumps for breaking changes. You can check out our versioning policy here: https://docs.astral.sh/uv/reference/versioning/

---

_Comment by @apellex on 2024-11-08 01:09_

Alrighty then, thank you for responding quickly.

---

_Closed by @apellex on 2024-11-08 01:09_

---

_Referenced in [geopandas/pyogrio#496](../../geopandas/pyogrio/pulls/496.md) on 2024-11-14 13:28_

---
