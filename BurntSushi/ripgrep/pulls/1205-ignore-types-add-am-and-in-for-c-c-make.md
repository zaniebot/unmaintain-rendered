```yaml
number: 1205
title: "ignore/types: add *.am and *.in for C/C++/make"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/c-in-types
created_at: 2019-02-28T01:09:45Z
updated_at: 2019-04-06T12:02:05Z
url: https://github.com/BurntSushi/ripgrep/pull/1205
synced_at: 2026-01-12T18:23:13Z
```

# ignore/types: add *.am and *.in for C/C++/make

---

_@okdana_

I keep running into this issue where i want to use `-tc` to search a repository, and i don't get everything i expect because some of the files are those `foo.h.in` ones. I know it's not feasible to support *every* possible `*.in` extension, but these are really common in C and C++ projects — for example, zsh and PCRE have a lot of them — so hopefully that justifies it...?

In doing this, i combined a bunch of patterns with `[...]` because it seemed like it'd be easier to maintain the `*.in` variants that way. Let me know if it makes it more confusing though

---

_@BurntSushi approved on 2019-04-06 12:01_

Aye, I think I buy this! I've definitely seen a lot of `*.in` files too in my day, but I don't tend to work in repos with them a lot. But I think this makes sense.

---

_Merged by @BurntSushi on 2019-04-06 12:02_

---

_Closed by @BurntSushi on 2019-04-06 12:02_

---
