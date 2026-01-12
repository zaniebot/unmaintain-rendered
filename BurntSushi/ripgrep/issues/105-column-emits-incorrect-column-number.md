```yaml
number: 105
title: "--column emits incorrect column number"
type: issue
state: closed
author: olson-dan
labels:
  - bug
assignees: []
created_at: 2016-09-26T18:09:20Z
updated_at: 2016-09-26T23:10:03Z
url: https://github.com/BurntSushi/ripgrep/issues/105
synced_at: 2026-01-12T18:23:11Z
```

# --column emits incorrect column number

---

_@olson-dan_

Trying to use ripgrep 0.2.0 with vim, I'm using "-n --color=never --column --no-header" as arguments, which lets me use a grepformat string of "%f:%l:%c:%m" to capture output and jump to the matches.

Problem is that the %c match in Vim expects a 0-based column to be output by the grep tool.  The previous grep tool I was using matched this expectation.  Lines are still expected to be 1-based.  Documentation for vim's grepformat strings are here, for reference: http://vimdoc.sourceforge.net/htmldoc/quickfix.html#errorformat

I've not encountered a way to do math on the column number within vim, although it may be possible.   Wondering if you can support a syntax like "--column=0based", "--column=1based" or some other method to support 0-based columns.


---

_Comment by @olson-dan on 2016-09-26 18:12_

Just to add a note, checking with ag 0.18.1 (which I had laying around), it also appears to use 0-based columns.


---

_Comment by @BurntSushi on 2016-09-26 18:13_

Really? @latortuga claimed 1-based columns in #18.

Looking at your vim docs link, I can't see where columns are specified to be 1-based?

When I use vim, I see its line numbers and column numbers reported as both 1-based.


---

_Comment by @BurntSushi on 2016-09-26 18:15_

Ug, it looks like I have a mismatch:

```
[andrew@Serval tmp] echo 'zztest' > foo
[andrew@Serval tmp] rg --column test foo
1:4:zztest
[andrew@Serval tmp] rg --vimgrep test foo
1:3:zztest
```

Which one is correct?


---

_Comment by @BurntSushi on 2016-09-26 18:17_

Well, obviously, `4` is wrong. The answer is either `3` (1-based) or `2` (0-based).

`ag` is 1-based:

```
[andrew@Serval tmp] ag --column test foo
1:3:zztest
[andrew@Serval tmp] ag --vimgrep test foo
foo:1:3:zztest
```


---

_Comment by @olson-dan on 2016-09-26 18:17_

--vimgrep is correct, I didn't see this option earlier.  It solves my problem entirely.


---

_Renamed from "0-based column support" to "--column emits incorrect column number" by @BurntSushi on 2016-09-26 18:17_

---

_Comment by @BurntSushi on 2016-09-26 18:18_

@olson-dan That's good. Meanwhile, I will re-purpose this issue to fix the off-by-1 error with --column. (How embarrassing.) Thanks!


---

_Label `bug` added by @BurntSushi on 2016-09-26 20:58_

---

_Closed by @BurntSushi on 2016-09-26 23:10_

---
