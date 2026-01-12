```yaml
number: 1778
title: ignore rules relativized to command line path
type: issue
state: closed
author: mobileink
labels:
  - duplicate
assignees: []
created_at: 2021-01-07T14:09:33Z
updated_at: 2021-01-07T14:25:39Z
url: https://github.com/BurntSushi/ripgrep/issues/1778
synced_at: 2026-01-12T16:13:24Z
```

# ignore rules relativized to command line path

---

_@mobileink_

#### What version of ripgrep are you using?

ripgrep 12.1.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

I don't remember

#### What operating system are you using ripgrep on?

MacOS 10.15.7

#### Describe your bug.

I need to ignore `src/lib/snarky/website`.  If I add this to  `.ignore` and run `rg foo src/` it has no effect. However, if I add `lib/snarky/website` to `src/.ignore` then it is ignored.  

Furthermore `rg foo src/lib` fails (does not ignore) with both `src/.ignore`and `.ignore` but succeeds with `src/lib/.ignore`

Whether or not this is a bug is debatable, but it is surprising and cumbersome, since it requires that I match my .ignore files to my search command. Sometimes I want to search `src/` and sometimes `src/lib`, so I have to sprinkle `.ignore` files throughout my source tree.




---

_Comment by @BurntSushi on 2021-01-07 14:25_

Dupe of #829.

---

_Label `duplicate` added by @BurntSushi on 2021-01-07 14:25_

---

_Closed by @BurntSushi on 2021-01-07 14:25_

---
