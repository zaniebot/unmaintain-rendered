```yaml
number: 1962
title: "Documentation for '--quiet' is misleading"
type: issue
state: closed
author: zachriggle
labels:
  - help wanted
  - doc
assignees: []
created_at: 2021-08-03T18:08:59Z
updated_at: 2023-03-28T11:23:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1962
synced_at: 2026-01-12T16:13:24Z
```

# Documentation for '--quiet' is misleading

---

_@zachriggle_

The documentation for the `--quiet` flag is misleading with the `--files` flag.

The current documentation states:

```
       -q, --quiet
           Do not print anything to stdout. If a match is found in a file,
           then ripgrep will stop searching. This is useful when ripgrep is
           used only for its exit code (which will be an error if no matches
           are found).

           When --files is used, then ripgrep will stop finding files after
           finding the first file that matches all ignore rules.
```

The very last part, "the first file that matches all ignore rules" seems like it's worded backwards.  I get what it means, it's just oddly worded.  It might be better to state instead "the first file that does not match any ignore rules".

Just an opinion, feel free to close if you disagree.

#### What version of ripgrep are you using?

```
rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

---

_Label `doc` added by @BurntSushi on 2021-08-03 18:16_

---

_Comment by @BurntSushi on 2021-08-03 18:16_

Feel free to file a PR! (For very small wording tweaks like this, I'd prefer just filing a PR.)

---

_Comment by @zachriggle on 2021-08-03 18:19_

I would absolutely love to, especially since it's small and easy, but my employer has VERY strict rules against FOSS contributions (even for permissive, MIT-licensed code, even for non-"code" changes).

---

_Label `help wanted` added by @BurntSushi on 2021-08-03 18:25_

---

_Comment by @ryanwhitehouse on 2023-03-17 21:58_

PR to fix this here https://github.com/BurntSushi/ripgrep/pull/2462 :) 

---

_Closed by @BurntSushi on 2023-03-28 11:23_

---
