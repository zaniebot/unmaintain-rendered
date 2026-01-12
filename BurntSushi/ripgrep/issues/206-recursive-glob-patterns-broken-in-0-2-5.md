```yaml
number: 206
title: Recursive glob patterns broken in 0.2.5
type: issue
state: closed
author: alejandro5042
labels:
  - bug
assignees: []
created_at: 2016-10-31T20:57:10Z
updated_at: 2016-11-01T04:04:37Z
url: https://github.com/BurntSushi/ripgrep/issues/206
synced_at: 2026-01-12T18:23:11Z
```

# Recursive glob patterns broken in 0.2.5

---

_@alejandro5042_

In an empty directory, create a file `test\file.txt` with contents `hi`. For simplicity of testing, make copies of `rg.exe` versions 0.2.3 and 0.2.5 and rename them according to their version.

```
0.08 - C:\Dev\Temp\rg-bug-subdirs> rg-v0.2.3 -g *.txt hi
test\file.txt
1:hi

0.03 - C:\Dev\Temp\rg-bug-subdirs> rg-v0.2.5 -g *.txt hi
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

Running with --debug:

```
0.04 - C:\Dev\Temp\rg-bug-subdirs> rg-v0.2.5 --debug -g *.txt hi
DEBUG:rg::args: will try to use memory maps
DEBUG:globset: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'h',
        'i'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(hi)], limit_size: 250, limit_class: 10 }
DEBUG:ignore::walk: ignoring ./test: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

However, move that file outside of the test directory to the current/root directory and it works.

---

_Label `bug` added by @BurntSushi on 2016-10-31 23:42_

---

_Closed by @BurntSushi on 2016-10-31 23:55_

---

_Comment by @BurntSushi on 2016-10-31 23:57_

This was a nice find, thank you! It slipped in with my latest changes to the ignore matcher code, but now there's a regression test for it. :-)


---

_Comment by @BurntSushi on 2016-11-01 00:03_

I think this is a bad bug, so I've kicked off a `0.2.6` release.


---

_Comment by @alejandro5042 on 2016-11-01 04:04_

I can confirm the latest `0.2.6` fixes this issue and passes my tests. Great turn around - thanks! 👍 


---
