```yaml
number: 520
title: Add -x/--line-regexp
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: feature/line-regexp
created_at: 2017-06-17T05:28:20Z
updated_at: 2017-08-09T10:53:36Z
url: https://github.com/BurntSushi/ripgrep/pull/520
synced_at: 2026-01-12T18:23:13Z
```

# Add -x/--line-regexp

---

_@okdana_

Let's try this!

This PR adds `-x`/`--line-regexp`, which essentially wraps the pattern in `^`...`$` in order to make it match whole lines only. This is an option defined in the [POSIX spec](http://pubs.opengroup.org/onlinepubs/009695399/utilities/grep.html) for `grep`, and one that i use a lot. I found it rather frustrating that `ag` didn't have it.

I will add additional notes as comments on the diff.

---

_@okdana reviewed on 2017-06-17 05:29_

---

_Review comment by @okdana on `doc/convert-to-man`:5 on 2017-06-17 05:29_

I found that this script didn't run on macOS with just `sed -i`, as the BSD version of `sed` requires the back-up suffix. So i added that in a way that should be compatible with both GNU and BSD `sed`. I also added the `&&` to make the script bail if any part of the process fails.

---

_@okdana reviewed on 2017-06-17 05:31_

---

_Review comment by @okdana on `src/app.rs`:112 on 2017-06-17 05:31_

It should actually be fine to apply both the `word-regexp` and `line-regexp` pattern transformations at once, since `\b` also matches the beginning/end of line. But i figured it'd be cleaner and more future-proof to make the one override the other.

---

_@okdana reviewed on 2017-06-17 05:32_

---

_Review comment by @okdana on `src/args.rs`:486 on 2017-06-17 05:32_

This line exceeds 80 cols, and it's becoming quite unwieldy anyway, so i think maybe i should change it. I wasn't sure what would be the best way to do that though.

ETA: Maybe, given that the two options are exclusive, we should combine their respective functions into a single `word_or_line_pattern()`?

---

_@okdana reviewed on 2017-06-17 05:33_

---

_Review comment by @okdana on `src/args.rs`:521 on 2017-06-17 05:33_

Added non-capturing group to avoid the same problem described in #506.

---

_Comment by @okdana on 2017-08-08 23:53_

btw @BurntSushi how do you feel about this? Does it fit with your vision for the tool's feature set? If so, is there anything you'd need me to do (besides rebase now) to get it in?

If not, that's cool too obv; i guess i'm just wondering if i should keep it tracking new changes or simply close it

---

_Review comment by @BurntSushi on `doc/convert-to-man`:5 on 2017-08-08 23:58_

> I also added the && to make the script bail if any part of the process fails.

I usually put `set -e` at the top of scripts for that. Could we do that instead? It feels a little nicer than trailing every command with `&&`. :-)

(I'm not up on my shell compatibility, but if this means switching `#!/bin/sh` to `#!/bin/bash`, then I'm fine with that.)

---

_Review comment by @BurntSushi on `src/args.rs`:486 on 2017-08-09 00:01_

Yeah I'd definitely like to stick to a 79 column limit (inclusive). I'd probably just write it like this:

```rust
let litpat = self.literal_pattern(pat.to_string());
let s = self.line_pattern(self.word_pattern(litpat));
```

But if you want to make a new method, that's cool too!

---

_@BurntSushi requested changes on 2017-08-09 00:02_

@okdana Arg, sorry, I let this one slip by me. Thanks so much for the ping. This was a pretty easy review. :-)

Basically, this all looks pretty good. I left a couple minor comments to iron out before merging! Thanks so much for doing this!

---

_Comment by @okdana on 2017-08-09 00:44_

Thanks! Updated:

* Rebased
* Accounted for new zsh completion function
* Added `-e` to `convert-to-man` (it does make more sense here, and it works everywhere)
* Changed long line in `args.rs` as suggested

---

_@BurntSushi approved on 2017-08-09 00:49_

---

_Merged by @BurntSushi on 2017-08-09 10:53_

---

_Closed by @BurntSushi on 2017-08-09 10:53_

---
