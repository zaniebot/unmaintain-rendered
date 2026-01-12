```yaml
number: 693
title: "Don't display context separators when only listing files"
type: issue
state: closed
author: tupton
labels: []
assignees: []
created_at: 2017-11-27T18:08:32Z
updated_at: 2017-11-29T17:55:43Z
url: https://github.com/BurntSushi/ripgrep/issues/693
synced_at: 2026-01-12T16:13:22Z
```

# Don't display context separators when only listing files

---

_@tupton_

```
❯ command rg --context=3 -l "context-separator"
complete/_rg
--
doc/rg.1.md
--
src/app.rs
--
src/args.rs
```

When listing files, even if `--context` is an option, I don't think we need the context separator. I understand that those options don't make much sense in this contrived example. However, I have an alias set up for `rg` that adds a bunch of options that I want as defaults, including `--context`. I (sometimes) use `-l` to list out files that I can pass to `vim`, e.g. `vim $(rg "context-separator" -l)`.

I tried to set `--context=0` when using `-l` – which would be an ok tradeoff for me – but it looks like multiple `--context` flags are not allowed.

`rg … | sed '/--/d'` is another potential solution, but it would break if `--context-separator` specified a different separator.

---

_Comment by @okdana on 2017-11-27 18:38_

Yeah, also affects `--count` and `--files-without-match`.

---

_Closed by @BurntSushi on 2017-11-29 17:55_

---
