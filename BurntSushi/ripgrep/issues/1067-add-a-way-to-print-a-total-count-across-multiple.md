```yaml
number: 1067
title: Add a way to print a total count across multiple files
type: issue
state: closed
author: sabi0
labels:
  - question
assignees: []
created_at: 2018-09-26T12:26:26Z
updated_at: 2021-06-23T11:43:25Z
url: https://github.com/BurntSushi/ripgrep/issues/1067
synced_at: 2026-01-12T16:13:22Z
```

# Add a way to print a total count across multiple files

---

_@sabi0_

Occasionally I need to count a number of particular lines across multiple (log) files.
Currently `-c` option allows to print the counts per file only. It would be nice if there was a way (`--count-total` ?) to add-up those individual counts and print the grand total.

It would be great if the grand total could also be printed alone, without the files counts.
I.e. `--count --count-total` would print per file counts and the grand total.
`--count-total` alone would only print the grand total (and could be used in scripting).

---

_Comment by @BurntSushi on 2018-09-26 12:29_

If you want the grand total, then I'd just suggest piping the output to `wc -l` or an equivalent program.

There is also the `--stats` flag which you might be interested in. You can even combine it with the `--quiet` flag to only show the stats without matches.

---

_Closed by @BurntSushi on 2018-09-26 12:29_

---

_Label `question` added by @BurntSushi on 2018-09-26 12:29_

---

_Comment by @sabi0 on 2018-09-26 13:41_

So for matches in one file one could use efficient built-in counter. But for matches in multiple files he'd have to pipe millions of matching lines through `wc -l`.
That seems inconsistent, don't you think?

`--stats` is an interesting option, thank you. But it would require some "output parsing" to be able to use the total count in a script.

Adding up the numbers produced by `--count` also requires some (awk) scripting. I.e. way more complicated than a simple `--count-total` option.

---

_Comment by @haasn on 2021-06-23 10:22_

> If you want the grand total, then I'd just suggest piping the output to `wc -l` or an equivalent program.

This, and the other suggestions, have non-negligible performance overhead. On a warm cache:

```
( rg -c '^'; )  10.35s user 17.80s system 850% cpu 3.311 total
( rg '^' | wc -l; )  24.80s user 31.46s system 599% cpu 9.388 total
( cat * | wc -l; )  3.00s user 21.46s system 118% cpu 20.577 total
( rg --stats --quiet '^'; )  55.31s user 17.75s system 841% cpu 8.685 total
( rg -c '^' | awk -F':' '{sum += $2} END {print sum}'; )  10.32s user 17.94s system 848% cpu 3.332 total
```

As you can see, the simple awk example parsing the output of `-c` is by far the fastest variant, even in the ideal case of there being only a single match per line. (If you change the `^` to something more like `.`, then `--stats` becomes unusably slow)

P.s. The reason `rg` vastly outperforms `cat` here is because of many small files. Incidentally, the fastest variant for this specific job was this:

```
( ( for f in *; do; wc -l $f &; done; wait; ) | awk ; )  2.84s user 25.08s system 1136% cpu 2.457 total
```

---

_Comment by @BurntSushi on 2021-06-23 11:39_

@haasn Your variants aren't equivalent. And indeed, using `.` changes things in a pertinent way. The `-c` flag only counts lines. But using the `--stats` flag will make ripgrep count every individual match. So when you search for `.`, you're essentially just using a huge hammer to count the number of UTF-8 encoded codepoints across your files.

---

_Comment by @BurntSushi on 2021-06-23 11:43_

Basically, the bottom line here is that `--stats -q` is going to produce a total count in the fastest possible way that ripgrep is capable of. Expecting it to be the fastest possible thing _for all inputs_ is not reasonable. When it comes to queries like `.`, it's plausible there are some performance bugs causing it to be slower than it has to be, and I welcome fixes there (assuming they don't add much complexity).

---
