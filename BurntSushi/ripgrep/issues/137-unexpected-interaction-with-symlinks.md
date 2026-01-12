```yaml
number: 137
title: Unexpected interaction with symlinks
type: issue
state: closed
author: boblehest
labels:
  - bug
assignees: []
created_at: 2016-09-29T23:09:13Z
updated_at: 2016-10-10T23:11:10Z
url: https://github.com/BurntSushi/ripgrep/issues/137
synced_at: 2026-01-12T18:23:11Z
```

# Unexpected interaction with symlinks

---

_@boblehest_

I noticed that `rg` ignores symlinks by default, even when I invoke it solely to search a single symlinked file. For me, that's very surprising behavior. The `-debug` flag also said nothing about why it was skipping my file.

So I'm suggestion to either change that behavior (unless maybe it's made this way for a good reason?), or inform the user that his symlinks are being ignored.


---

_Comment by @BurntSushi on 2016-09-29 23:15_

Agreed, `rg` should search a symlinked file if it's explicitly specified. This is good enough reason for me:

```
[andrew@Cheetah tmp] echo test > foo
[andrew@Cheetah tmp] rg test foo
1:test
[andrew@Cheetah tmp] ln -s foo bar
[andrew@Cheetah tmp] rg test bar
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
[andrew@Cheetah tmp] grep test bar
test
```

Otherwise, you can use the `-L` flag with `rg` to follow symlinks.


---

_Label `bug` added by @BurntSushi on 2016-10-05 23:28_

---

_Closed by @BurntSushi on 2016-10-10 23:11_

---
