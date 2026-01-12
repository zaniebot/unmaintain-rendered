```yaml
number: 2490
title: Fix code block label in docs
type: pull_request
state: closed
author: BD103
labels: []
assignees: []
base: master
head: patch-1
created_at: 2023-04-13T17:02:14Z
updated_at: 2023-07-03T03:26:27Z
url: https://github.com/BurntSushi/ripgrep/pull/2490
synced_at: 2026-01-12T18:23:14Z
```

# Fix code block label in docs

---

_@BD103_

I changed the attribute (info string) `ignore` to `text`. `ignore` is used for Rust code that should not be run and also receives a label in the generated documentation, which is incorrect for the modified block. Changing it to `text` removes syntax highlighting (though the previous highlighting was incorrect).

Additional links:
- [Rustdoc code attributes](https://doc.rust-lang.org/rustdoc/write-documentation/documentation-tests.html#attributes)
- [Commonmark spec on info string](https://spec.commonmark.org/0.30/#info-string)

---

_Comment by @BD103 on 2023-07-03 03:26_

Closing due to inactivity. Should this ever gain attention, I can create another PR.

---

_Closed by @BD103 on 2023-07-03 03:26_

---
