```yaml
number: 3093
title: "Using the `ignore` create together with the `vfs` create, is it possible?"
type: issue
state: closed
author: TheKnarf
labels:
  - wontfix
assignees: []
created_at: 2025-07-06T12:48:31Z
updated_at: 2025-07-06T13:54:44Z
url: https://github.com/BurntSushi/ripgrep/issues/3093
synced_at: 2026-01-12T16:13:25Z
```

# Using the `ignore` create together with the `vfs` create, is it possible?

---

_@TheKnarf_

#### Describe your feature request

Is it possible to use the `ignore` create together with the `vfs` create for [virtual file system](https://crates.io/crates/vfs). I use the `fvs` create so that its easy to write unit tests that use a mock file system in my project.

The best way I've found currently to use the `ignore` create and `vfs` together is to use the `GitignoreBuilder` manually and reimplement file traversal myself (I can't even add a file with `GitignoreBuilder` since that takes a path and not file content, so I'll have to [add_line](https://docs.rs/ignore/0.4.23/ignore/gitignore/struct.GitignoreBuilder.html#method.add_line) manually for each line). In practice that means that I have to re-implement a lot of stuff myself when I'd prefer to just use the `WalkBuilder` directly.

Is there an more ergonomic way this could be done?

---

_Comment by @BurntSushi on 2025-07-06 13:36_

No. `ignore` is tightly coupled with file system APIs and I don't want to change that. Sorry.

---

_Closed by @BurntSushi on 2025-07-06 13:36_

---

_Label `wontfix` added by @BurntSushi on 2025-07-06 13:36_

---

_Comment by @TheKnarf on 2025-07-06 13:45_

Wouldn't making a bit less tightly coupled with file system API make it easier to write unit test for the ripgrep project? And potentially open up more re-use across the ecosystem, ex if someone want to create a tool that reuses `ignore` / `ripgrep` logic for other things than a "real filesystem" like zip archives, AWS S3 filestorage, a filestructure loaded from a database or an api call, etc.

---

_Comment by @BurntSushi on 2025-07-06 13:53_

I have no problem writing tests using the real file system.

I understand the advantages of abstraction, but they don't really confer many advantages for me personally. I just don't need that sort of abstraction to write tests for `ignore`, and I'm not interested in working on making gitignore filtering work in contexts other than standard file systems. In contrast, you neglect to discuss the disadvantages, which involve a massive amount of work and API design across at least `ignore` and `walkdir`.

If this sort of abstraction is a requirement for you, then you'll need to build your own crate to do it.

---
