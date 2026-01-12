```yaml
number: 616
title: CLI gives supoptimal suggestion for --files-without-matches
type: issue
state: closed
author: chris-morgan
labels:
  - bug
assignees: []
created_at: 2017-09-30T05:12:20Z
updated_at: 2017-10-21T22:43:12Z
url: https://github.com/BurntSushi/ripgrep/issues/616
synced_at: 2026-01-12T16:13:22Z
```

# CLI gives supoptimal suggestion for --files-without-matches

---

_@chris-morgan_

There are two related options:
- `--files-with-matches`
- `--files-without-match`

When I run the incorrect `rg --files-without-matches`, I get this response:

```
error: Found argument '--files-without-matches' which wasn't expected, or isn't valid in this context
        Did you mean --files-with-matches?

USAGE: […]
```

I don’t know the technique being used to make the suggestion, but `--files-without-match` is closer than `--files-with-matches` (Levenshtein distance of two instead of three), and the latter certainly wasn’t the recommendation that I expected.

---

_Comment by @okdana on 2017-09-30 06:02_

Those suggestions are provided by [clap](https://github.com/kbknapp/clap-rs/blob/master/src/suggestions.rs), which apparently uses [strsim](https://github.com/dguo/strsim-rs)'s Jaro–Winkler implementation.

I don't know much about this stuff at all, but i compiled a little command-line tool to test the function and it gives me 100% confidence when comparing `--files-without-matches` to either possibility... which doesn't seem right? The other implementations i tested give 95% for `--files-with-matches` and 98% for `--files-without-match`. I'm thinking it probably has to do with this note in the `README`:

>this implementation of Jaro-Winkler does not limit the common prefix length

Seems like most implementations limit the common prefix to 4 characters. So i guess if you have a long-ish common prefix the confidence can actually exceed 100% (which strsim then caps downward)?

idk, maybe ask the `clap` fellow whether a different algorithm might be appropriate? 🤷‍♀️

---

_Comment by @BurntSushi on 2017-09-30 12:25_

cc @kbknapp 

---

_Label `bug` added by @BurntSushi on 2017-09-30 12:25_

---

_Comment by @BurntSushi on 2017-10-21 22:43_

I filed a bug with clap: https://github.com/kbknapp/clap-rs/issues/1073

---

_Closed by @BurntSushi on 2017-10-21 22:43_

---
