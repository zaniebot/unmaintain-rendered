```yaml
number: 1710
title: Add a type for minified files
type: issue
state: closed
author: duongdominhchau
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2020-10-19T12:22:37Z
updated_at: 2020-10-19T13:10:56Z
url: https://github.com/BurntSushi/ripgrep/issues/1710
synced_at: 2026-01-12T16:13:24Z
```

# Add a type for minified files

---

_@duongdominhchau_

When searching, usually the matches from minified files are unwanted because each match is extremely long and the match can be found in original file as well. Because matches from minified files are rarely desired, and currently `rg --type-list` has no type for minified files, I suggest adding a `minified` type so people can ignore minified files while searching by using `--type-not minified`. This new type should be printed as `minified: *.min.js, *.min.css, *.min.html` when we run `rg --type-list`.

---

_Comment by @BurntSushi on 2020-10-19 12:33_

I think this seems reasonable to me. I don't usually track open issues for file types. Just submitting a PR is fine.

Also, you might be interested in the `-M/--max-columns` flag. It is particularly helpful with things like minified files since it will suppress extremely long lines.

---

_Label `enhancement` added by @BurntSushi on 2020-10-19 12:33_

---

_Label `help wanted` added by @BurntSushi on 2020-10-19 12:33_

---

_Closed by @BurntSushi on 2020-10-19 13:10_

---
