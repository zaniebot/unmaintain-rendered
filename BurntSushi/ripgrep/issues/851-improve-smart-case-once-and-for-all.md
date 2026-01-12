```yaml
number: 851
title: improve --smart-case once and for all
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
  - libripgrep
assignees: []
created_at: 2018-03-10T12:33:56Z
updated_at: 2018-03-14T02:55:40Z
url: https://github.com/BurntSushi/ripgrep/issues/851
synced_at: 2026-01-12T16:13:22Z
```

# improve --smart-case once and for all

---

_@BurntSushi_

We should improve the `--smart-case` flag to be perfect or nearly perfect at least. The regex-syntax crate rewrite now has a [more detailed AST](https://docs.rs/regex-syntax/0.5.0/regex_syntax/ast/enum.Ast.html), which permits us to handle cases like `[A-Z]` and `\p{Ll}` differently. In particular, I propose:

When the `--smart-case` flag is given, ripgrep chooses to match case insensitively if and only if there is at least one literal character in the pattern and that all such literals are not considered as uppercase. If the pattern contains no literals or at least one uppercase literal character, then normal case sensitive search is used.

Examples: `abc`, `[a-z]`, `abc\w` and `abc\p{Ll}` are all searched case insensitively while `aBc`, `[A-Z]`, `aBc\w` and `\p{Ll}` are searched case sensitively.

See also: https://github.com/BurntSushi/ripgrep/issues/717#issuecomment-372000616

cc @okdana 

---

_Label `enhancement` added by @BurntSushi on 2018-03-10 12:33_

---

_Label `libripgrep` added by @BurntSushi on 2018-03-10 12:33_

---

_Added to milestone `libripgrep` by @BurntSushi on 2018-03-10 12:33_

---

_Closed by @BurntSushi on 2018-03-14 02:55_

---
