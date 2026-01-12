```yaml
number: 1391
title: "ignore/types: Add slim, slime, and skim template filetypes"
type: pull_request
state: merged
author: paradox460
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2019-09-26T17:52:56Z
updated_at: 2020-02-15T14:17:46Z
url: https://github.com/BurntSushi/ripgrep/pull/1391
synced_at: 2026-01-12T18:23:13Z
```

# ignore/types: Add slim, slime, and skim template filetypes

---

_@paradox460_

[Slim](http://slim-lang.com/) is a popular templating language for ruby, and [Slime](https://github.com/slime-lang/slime) is its elixir counterpart. [Skim](https://github.com/appjudo/skim) is "rails javascript" variant of it.

---

_Comment by @BurntSushi on 2019-09-26 17:55_

Is it appropriate to group all of them under one file type? Do they have enough differences where it would make sense to give them each their own type?

---

_Comment by @paradox460 on 2019-09-26 18:57_

Slime attempts to be identical to Slim, with a few minor elixir-specific things that wouldn't make sense in ruby.

Skim is designed to be pretty minimal, but still has identical syntax to main slim, just with features removed

---

_Merged by @BurntSushi on 2020-02-15 14:17_

---

_Closed by @BurntSushi on 2020-02-15 14:17_

---
