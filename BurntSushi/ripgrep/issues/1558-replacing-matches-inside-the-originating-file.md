```yaml
number: 1558
title: Replacing matches inside the originating file
type: issue
state: closed
author: djc
labels:
  - invalid
assignees: []
created_at: 2020-04-21T20:32:28Z
updated_at: 2020-04-21T20:35:26Z
url: https://github.com/BurntSushi/ripgrep/issues/1558
synced_at: 2026-01-12T16:13:23Z
```

# Replacing matches inside the originating file

---

_@djc_

I semi-regularly use command-line tools to do mass substitutions in my code (whether with Rust or with Python). Perhaps I ought to learn to make better use of the IDE here, but even for non-code things this has been a useful feature.

Currently I usually use something like `find . -name "*.rs" | xargs sed -i '' 's!foo!bar!g'`. However, `sed` seems like a pretty old-school tool and I sometimes find that it's hard to make it do modern regex features.

It would be nice if ripgrep could do --replace -i '' (the argument is used to as a suffix for backed up files, but I almost never use it because stuff is usually under version control anyway).

I realize this might be out of scope a bit since it involves actually rewriting the files rather than just searching through them. Wanted to mention this as a desirable feature (to me) anyway.

---

_Comment by @BurntSushi on 2020-04-21 20:35_

https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#search-and-replace

---

_Closed by @BurntSushi on 2020-04-21 20:35_

---

_Label `invalid` added by @BurntSushi on 2020-04-21 20:35_

---
