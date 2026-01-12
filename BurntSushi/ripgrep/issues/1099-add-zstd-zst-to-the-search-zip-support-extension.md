```yaml
number: 1099
title: "Add zstd (.zst) to the --search-zip support (extension of #539)"
type: issue
state: closed
author: cemeyer
labels: []
assignees: []
created_at: 2018-11-03T17:53:42Z
updated_at: 2019-01-23T02:05:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1099
synced_at: 2026-01-12T16:13:22Z
```

# Add zstd (.zst) to the --search-zip support (extension of #539)

---

_@cemeyer_

#### What version of ripgrep are you using?

git master

#### Describe your question, feature request, or bug.

Just need an additional list entry in https://github.com/BurntSushi/ripgrep/blob/master/grep-cli/src/decompress.rs#L349 and a small update to the manual page.  Kudos to @balajisivaraman for already having done the work to easily add more decompressors in a general fashion.

Something like:

```
+ const ARGS_ZSTD: &[&str] = &["zstd", "-d", "-c"];
...
+     cmd("*.zst", ARGS_ZSTD),
```

I can't find where the OPTIONS (`-z, --search-zip`) portion of the manual page is stored in the directory hierarchy, but that should be updated as well.

Should be pretty trivial!  Thanks for considering it.

---

_Closed by @BurntSushi on 2019-01-23 01:56_

---

_Comment by @cemeyer on 2019-01-23 02:05_

Awesome, thanks!

---
