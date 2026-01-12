```yaml
number: 3260
title: "`ignore` crate: support for tool-specific ignore files"
type: issue
state: closed
author: olson-dan
labels:
  - duplicate
assignees: []
created_at: 2026-01-07T19:54:24Z
updated_at: 2026-01-07T21:26:51Z
url: https://github.com/BurntSushi/ripgrep/issues/3260
synced_at: 2026-01-12T16:13:25Z
```

# `ignore` crate: support for tool-specific ignore files

---

_@olson-dan_

I apologize if this has been litigated already but I couldn't find anything in search.

The ignore crate is used by a number of non-ripgrep tools. I have found that I run into situations where I want to ignore things for one tool but not another.  The feature request then is to be able to add additional ignored files to the ignore crate on an individual tool basis.  For instance, a hypothetical tool `foo` could add `.fooignore` as a specific ignore file only honored by that tool. I imagine this could be configured on the `WalkBuilder` but I didn't see anything there presently.

I do not currently maintain a tool that uses or would use the `ignore` crate in this way but if it were added I would open feature requests or PRs for some tools currently using `ignore`.

---

_Comment by @BurntSushi on 2026-01-07 20:01_

This was added long ago: https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#method.add_custom_ignore_filename

`ignore` doesn't encode any ripgrep specific logic.

It's how `fd` supports `.fdignore`:

https://github.com/sharkdp/fd/blob/5f95a781212e3efbc6d91ae50e0ca0ce0693da50/src/walk.rs#L367-L369

It's how ripgrep support `.rgignore`:

https://github.com/BurntSushi/ripgrep/blob/0a88cccd5188074de96f54a4b6b44a63971ac157/crates/core/flags/hiargs.rs#L904-L906

---

_Closed by @BurntSushi on 2026-01-07 20:01_

---

_Label `duplicate` added by @BurntSushi on 2026-01-07 20:01_

---

_Comment by @olson-dan on 2026-01-07 21:26_

Thanks and apologies for the spam, I misinterpreted this part of the docs and didn't scroll down far enough to see the function in question:

> Ignore files currently only come from git ignore files (.gitignore, .git/info/exclude and the configured global gitignore file), plain .ignore files, which have the same format as gitignore files, or explicitly added ignore files.

---
