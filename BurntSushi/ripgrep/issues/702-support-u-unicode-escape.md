```yaml
number: 702
title: Support \u Unicode escape
type: issue
state: closed
author: roblourens
labels:
  - enhancement
assignees: []
created_at: 2017-12-02T17:52:22Z
updated_at: 2018-03-14T02:55:40Z
url: https://github.com/BurntSushi/ripgrep/issues/702
synced_at: 2026-01-12T16:13:22Z
```

# Support \u Unicode escape

---

_@roblourens_

Ripgrep supports `\x{1234}`, but not `\u1234`, while many people expect the latter. Have you considered also supporting `\u`?

If not, we'll rewrite one to the other for compatibility with the JS regex engine. 

Ref: https://github.com/Microsoft/vscode/issues/39404

---

_Comment by @BurntSushi on 2017-12-02 18:24_

This is an interesting ticket. It probably belongs as an issue on the [regex engine](https://github.com/rust-lang/regex) itself. Historically, I chose not to support the `\u` syntax because Rust itself has support for such syntax in its string literals. But obviously that isn't available when you aren't writing Rust code. That seems like a good enough argument to support the syntax to me.

Note that technically, [according to UAX#18](http://www.unicode.org/reports/tr18/#Hex_notation), the `\x` syntax satisfies the requirement, but they also list a specific grammar for the `\u` (and `\u{...}`) style of syntax.

I [filed a ticket with the regex crate](https://github.com/rust-lang/regex/issues/424), but I'll leave this open so it's easy to track with respect to ripgrep.

(I don't have a specific timeline on this. I should be able to get it done as part of the `regex-syntax` rewrite that I'm working on, but I don't have an ETA. It could be a few weeks (ideal) if things go well, or it could be months.)

---

_Label `enhancement` added by @BurntSushi on 2017-12-02 18:25_

---

_Assigned to @BurntSushi by @BurntSushi on 2017-12-02 18:25_

---

_Closed by @BurntSushi on 2018-03-14 02:55_

---
