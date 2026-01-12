```yaml
number: 1062
title: Better globs for ocaml files
type: issue
state: closed
author: Wilfred
labels: []
assignees: []
created_at: 2018-09-24T14:55:15Z
updated_at: 2018-09-24T21:09:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1062
synced_at: 2026-01-12T16:13:22Z
```

# Better globs for ocaml files

---

_@Wilfred_

https://github.com/BurntSushi/ripgrep/blob/master/ignore/src/types.rs#L212

```rust
("ml", &["*.ml"]),
```

It'd be really useful to add `*.mli` files, which are header files for ocaml code. I think it might also make sense to rename the language from "ml" to "ocaml".

---

_Comment by @BurntSushi on 2018-09-24 15:08_

There is already an Ocaml type: https://github.com/BurntSushi/ripgrep/blob/f72c2dfd90f98cfbd678d3d957e3d104237ab15b/ignore/src/types.rs#L220

The ml type is for Standard ML.

---

_Closed by @BurntSushi on 2018-09-24 15:08_

---

_Comment by @Wilfred on 2018-09-24 21:09_

Argh, I totally missed that. Thanks.

I'm currently just using a dumb 'first match' heuristic in my Emacs integration of ripgrep. I think I'd be better off using the match with the most extensions, as those are typically more widely used languages.

---
