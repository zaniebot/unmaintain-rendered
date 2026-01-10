```yaml
number: 1240
title: "test: update snapshot"
type: pull_request
state: closed
author: BurntSushi
labels: []
assignees: []
base: main
head: ag/fix-removal-stats
created_at: 2024-02-03T02:12:10Z
updated_at: 2024-02-03T02:47:55Z
url: https://github.com/astral-sh/uv/pull/1240
synced_at: 2026-01-10T15:33:24Z
```

# test: update snapshot

---

_Pull request opened by @BurntSushi on 2024-02-03 02:12_

I'm seeing this test failure on main on both my Linux and macOS machines.

I can't quite explain it though, because the [last release of `future`
was Jan 2023](https://pypi.org/project/future/#history).


---

_Comment by @BurntSushi on 2024-02-03 02:42_

This ended up being a problem on my end because I was setting [PYTHONDONTWRITEBYTECODE=1](https://github.com/BurntSushi/dotfiles/blob/eb14900839a210f31b826f0ab070506528ea6a99/.envrc#L20). This in turn meant that my setup wasn't generating a couple `.pyc` files that were being generated in CI (and probably most other environments).

I have no idea why I set that env, so I've unset it.

---

_Closed by @BurntSushi on 2024-02-03 02:42_

---

_Branch deleted on 2024-02-03 02:47_

---
