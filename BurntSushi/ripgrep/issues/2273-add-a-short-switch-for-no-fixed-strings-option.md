```yaml
number: 2273
title: "Add a short switch for \"--no-fixed-strings\" option"
type: issue
state: closed
author: steliyan
labels:
  - wontfix
assignees: []
created_at: 2022-08-03T14:19:42Z
updated_at: 2022-08-03T14:42:48Z
url: https://github.com/BurntSushi/ripgrep/issues/2273
synced_at: 2026-01-12T16:13:24Z
```

# Add a short switch for "--no-fixed-strings" option

---

_@steliyan_

I am using `ripgrep` through `neovim` and `telescope`, both for finding files and for searching in files.

When I search in files I want a non-regex match, hence I use the `--fixed-strings` flag. Sometimes, I want to perform a regex search and it would be useful to have a shorthand name for the `--no-fixed-strings` flag. I thinking `-NF` as in "No -F"?

I think this would be a small win from a UX perspective. Also, a release hasn't been done in a long time, so you can include it in the next one. :)



---

_Comment by @BurntSushi on 2022-08-03 14:42_

There are very few short flag names left and this one doesn't cut it IMO. The intended flow is to have fixed strings disabled and enable it when you need it.

Also, your suggestion isn't a short flag. It's two short flags combined that already mean something today.

As for the release schedule, please see: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#release :)

---

_Closed by @BurntSushi on 2022-08-03 14:42_

---

_Label `wontfix` added by @BurntSushi on 2022-08-03 14:42_

---
