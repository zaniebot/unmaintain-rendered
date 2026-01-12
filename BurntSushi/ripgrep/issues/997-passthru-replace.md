```yaml
number: 997
title: passthru replace
type: issue
state: closed
author: LippyBoy
labels:
  - enhancement
  - libripgrep
assignees: []
created_at: 2018-07-29T11:22:00Z
updated_at: 2018-08-20T11:10:21Z
url: https://github.com/BurntSushi/ripgrep/issues/997
synced_at: 2026-01-12T16:13:22Z
```

# passthru replace

---

_@LippyBoy_

#### What version of ripgrep are you using?

0.81

#### How did you install ripgrep?

releases deb

#### What operating system are you using ripgrep on?

Ubuntu 16.04

#### Describe your question, feature request, or bug.

It would be really great if the replace flag worked with the passthru flag - that is, it will replace everything that matches the regex, but will still output non-matching lines.

I know that you can use sed for this, but ripgrep is much more convenient and user friendly, and also highlighting the replacements is great.

Use cases:
1. Multiple replaces - when you want to replace multiple things, you can't chain ripgreps since strings that aren't matched by the first replace won't be inputted into the second, so it won't replace them
2. Replacing paths - sometimes I read other people logs, and for the sake of convenience I want to replace their file paths with mine - no problem, just replace the prefix, but of course I can't do it currently in ripgrep since all other logs will be omitted.
3. This stack overflow question I stumbled upon could have bin solved with this feature: https://stackoverflow.com/questions/51504318/how-to-replace-a-with-o-with-0-in-a-word-list-and-remove-the-unchanged-wor

---

_Comment by @BurntSushi on 2018-07-29 11:28_

The replace flag should already be working with passthru. If it isn't, that sounds like a bug. Could you please provide a small reproducible example, asking with actual and expected output?

---

_Comment by @LippyBoy on 2018-07-29 11:33_

```
~/rust> rg --version
ripgrep 0.8.1
-SIMD -AVX
~/rust> 
~/rust> rg --passthru --replace b a
error: The argument '--replace <REPLACEMENT_TEXT>' cannot be used with '--passthru'

USAGE:
    
    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f FILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list

For more information try --help
~/rust> 
```

---

_Comment by @BurntSushi on 2018-07-29 12:19_

@LippyBoy Hah, nice. I suspect I added that because the implementation simply doesn't support it.

I'll look into adding this as part of libripgrep by not implementing `--passthru` as a hack.

---

_Label `enhancement` added by @BurntSushi on 2018-07-29 12:19_

---

_Label `libripgrep` added by @BurntSushi on 2018-07-29 12:19_

---

_Added to milestone `libripgrep` by @BurntSushi on 2018-07-29 12:19_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
