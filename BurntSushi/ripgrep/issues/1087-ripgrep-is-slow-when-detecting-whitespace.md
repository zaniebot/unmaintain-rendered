```yaml
number: 1087
title: "ripgrep is slow when detecting whitespace literals, e.g., `'[ \\t]$'`"
type: issue
state: closed
author: shlomif
labels:
  - enhancement
  - rollup
assignees: []
created_at: 2018-10-19T09:06:54Z
updated_at: 2020-03-15T17:31:19Z
url: https://github.com/BurntSushi/ripgrep/issues/1087
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep is slow when detecting whitespace literals, e.g., `'[ \t]$'`

---

_@shlomif_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

My system is Mageia Linux v7 x86-64 on a Core i3 Sandy Bridge machine.

rg is much slower than ag when searching for the `'[ \t]$'` regex on a
sample repo. With this benchmark case:


```bash
#! /bin/bash
#
# ag-rg-bench.bash
# Copyright (C) 2018 Shlomi Fish <shlomif@cpan.org>
#
# Distributed under terms of the MIT license.
#

set -e -x
d=freecell-pro-0fc-deals

if ! test -d "$d"
then
    git clone https://github.com/shlomif/freecell-pro-0fc-deals
fi

rg --version
ag --version
locale
(
    cd "$d"
    set +e
    time ag '[ \t]$'
    time rg '[ \t]$'
    time ag '[ \t]$'
    time rg '[ \t]$'
    time rg '[ \t]$'
)
```

I am getting the following:

```
+ d=freecell-pro-0fc-deals
+ test -d freecell-pro-0fc-deals
+ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
+ ag --version
ag version 2.1.0

Features:
  +jit +lzma +zlib
+ locale
LANG=en_GB.UTF-8
LC_CTYPE=en_US.UTF-8
LC_NUMERIC=en_GB.UTF-8
LC_TIME=en_GB.UTF-8
LC_COLLATE=en_US.UTF-8
LC_MONETARY=en_US.UTF-8
LC_MESSAGES=en_US.UTF-8
LC_PAPER=en_US.UTF-8
LC_NAME=en_GB.UTF-8
LC_ADDRESS=en_US.UTF-8
LC_TELEPHONE=en_US.UTF-8
LC_MEASUREMENT=en_GB.UTF-8
LC_IDENTIFICATION=en_GB.UTF-8
LC_ALL=
+ cd freecell-pro-0fc-deals
+ set +e
+ ag '[ \t]$'

real	0m0.101s
user	0m0.303s
sys	0m0.025s
+ rg '[ \t]$'

real	0m1.249s
user	0m4.799s
sys	0m0.086s
+ ag '[ \t]$'

real	0m0.094s
user	0m0.305s
sys	0m0.016s
+ rg '[ \t]$'

real	0m1.272s
user	0m4.910s
sys	0m0.078s
+ rg '[ \t]$'

real	0m1.246s
user	0m4.828s
sys	0m0.074s
```

My system is Mageia Linux v7 x86-64 on a Core i3 Sandy Bridge machine.


---

_Comment by @BurntSushi on 2018-10-19 10:58_

Thanks for the easy reproduction! Much appreciated.

This is unfortunately a case where ripgrep's literal optimizations end up making it slower rather than faster. The presence of literal optimizations virtually guarantees that cases like this exist, although we might consider some common sense heuristics to improve it. e.g., If all detected literals are just whitespace, then it's probably better not to use them. That would be a fairly easy change in `grep-regex/src/literal.rs`.

You can test this by trying ripgrep with PCRE2. For example,  `rg '[ \t]$' -P -U --no-pcre2-unicode --mmap` runs faster on my machine than `ag '[ \t]$'`. In particular, the flags given to ripgrep here enable the same performance profile as ag, albeit with PCRE2 instead of PCRE1. Similarly, `rg '\s$'` runs quite a bit faster than `ag '\s$'` (although, `rg -U '\s$'` is the more accurate comparison, but still runs quite a bit faster) precisely because `\s` prevents literal detection from happening since there are too many of them.

---

_Renamed from "rg is much slower than ag when searching for the `'[ \t]$'` regex." to "ripgrep is slow when detecting whitespace literals, e.g., `'[ \t]$'`" by @BurntSushi on 2018-10-19 10:58_

---

_Label `enhancement` added by @BurntSushi on 2018-10-19 11:00_

---

_Comment by @shlomif on 2018-10-19 21:45_

On Fri, 19 Oct 2018 03:58:27 -0700
Andrew Gallant <notifications@github.com> wrote:

> This is unfortunately a case where ripgrep's literal optimizations end up
> making it slower rather than faster. The presence of literal optimizations
> virtually guarantees that cases like this exist, although we might consider
> some common sense heuristics to improve it. e.g., If all detected literals
> are just whitespace, then it's probably better not to use them. That would be
> a fairly easy change in `grep-regex/src/literal.rs`.
> 
> You can test this by trying ripgrep with PCRE2. For example,  `rg '[ \t]$' -P
> -U --no-pcre2-unicode --mmap` runs faster on my machine than `ag '[ \t]$'`.
> In particular, the flags given to ripgrep here enable the same performance
> profile as ag, albeit with PCRE2 instead of PCRE1. Similarly, `rg '\s$'` runs
> quite a bit faster than `ag '\s$'` (although, `rg -U '\s$'` is the more
> accurate comparison, but still runs quite a bit faster).
> 

thanks for the reply.

-- 
-----------------------------------------------------------------
Shlomi Fish       http://www.shlomifish.org/
List of Networking Clients - http://shlom.in/net-clients

I achieved my fast times by multitudes of 1% reductions.
— Bill Raymond in
https://groups.yahoo.com/neo/groups/fc-solve-discuss/conversations/messages/222

Please reply to list if it's a mailing list post - http://shlom.in/reply .


---

_Label `rollup` added by @BurntSushi on 2020-03-15 16:02_

---

_Closed by @BurntSushi on 2020-03-15 17:19_

---

_Comment by @shlomif on 2020-03-15 17:31_

Thanks for fixing it, @BurntSushi ! :+1: 

---
