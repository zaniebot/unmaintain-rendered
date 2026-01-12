```yaml
number: 1422
title: "Ripgrep should not interet {} in .gitignore"
type: issue
state: closed
author: hwalinga
labels:
  - duplicate
assignees: []
created_at: 2019-11-07T16:49:20Z
updated_at: 2019-11-07T16:52:56Z
url: https://github.com/BurntSushi/ripgrep/issues/1422
synced_at: 2026-01-12T16:13:23Z
```

# Ripgrep should not interet {} in .gitignore

---

_@hwalinga_

#### What version of ripgrep are you using?

```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

#### How did you install ripgrep?

Installed with `cargo`.

#### What operating system are you using ripgrep on?

    uname -a

    Linux LMDE 4.19.0-0.bpo.6-amd64 #1 SMP Debian 4.19.67-2+deb10u1~bpo9+1 (2019-09-30) x86_64 GNU/Linux

#### Describe your question, feature request, or bug.

`rg` gitignore has some strange interpretation on some strange pattern. The pattern is `*.py{}`, which will ignore all `*.py` files. As seen in the wild here https://github.com/biopython/biopython/blob/master/.gitignore

#### If this is a bug, what are the steps to reproduce the behavior?

```bash
git clone https://github.com/biopython/biopython/
cd biopython
rg -l .  # this will list no .py files.
```

#### If this is a bug, what is the actual behavior?

If there is a `*.py{}` in the `.gitignore`, all `*.py` will be ignored by `rg`. It seems to interpret {} as an empty string.

#### If this is a bug, what is the expected behavior?

`rg` should interpret a pattern like `*.py{}` literally. So, do not interpret {} as anything.

#### About brace extensions

I know there exist something as bash brace extensions, but this is not allowed in `.gitignore`. Also a correct brace extensions should have a `,` or `..`, otherwise it is taken literally.


---

_Label `duplicate` added by @BurntSushi on 2019-11-07 16:52_

---

_Comment by @BurntSushi on 2019-11-07 16:52_

Dupe of #1221 

---

_Closed by @BurntSushi on 2019-11-07 16:52_

---
