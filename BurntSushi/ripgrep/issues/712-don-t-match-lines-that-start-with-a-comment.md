```yaml
number: 712
title: "Don't match lines that start with a comment"
type: issue
state: closed
author: flip111
labels:
  - duplicate
  - question
assignees: []
created_at: 2017-12-11T18:49:17Z
updated_at: 2017-12-11T20:50:00Z
url: https://github.com/BurntSushi/ripgrep/issues/712
synced_at: 2026-01-12T16:13:22Z
```

# Don't match lines that start with a comment

---

_@flip111_

How do i match a word `foo` that doesn't start with a comment `^\s*--`?

So this should match:
```
barfoo -- qux
    foo
foo bar
```

This should not match:
```
-- foo
     --
     -- foo
-- bar foo
```

---

_Comment by @okdana on 2017-12-11 20:38_

There's not really a great way to do it.\* You have to either build up a pattern with alternations enumerating the possible non-comment matches or pipe one `rg` into another.

If multiple `-e` patterns could be `AND`ed together rather than `OR`ed that would be a solution for you. Unfortunately, `rg` doesn't have that feature (yet) — see #473.

\* With `rg` specifically, i mean. In tools like `ag` you can accomplish it with look-around, but `rg`'s regex engine doesn't support that.

---

_Comment by @BurntSushi on 2017-12-11 20:49_

Yeah this is a duplicate of #473. Otherwise, `rg -v '^\s*--' | rg foo` works for now at the cost of losing the normal "pretty" format emitted by ripgrep.

---

_Closed by @BurntSushi on 2017-12-11 20:49_

---

_Label `duplicate` added by @BurntSushi on 2017-12-11 20:50_

---

_Label `question` added by @BurntSushi on 2017-12-11 20:50_

---
