```yaml
number: 130
title: "don't output line numbers when the input is from `stdin`"
type: issue
state: closed
author: jzinn
labels:
  - question
assignees: []
created_at: 2016-09-28T22:08:34Z
updated_at: 2024-07-11T12:09:24Z
url: https://github.com/BurntSushi/ripgrep/issues/130
synced_at: 2026-01-12T16:13:21Z
```

# don't output line numbers when the input is from `stdin`

---

_@jzinn_

When reading input from `stdin`, the output includes line numbers by default:

```
$ echo hi | rg i
1:hi
```

It would be nice for `rg` not to print line numbers in this case.

Although the `-N` flag can be used to turn off line numbers, when using `rg` in a pipeline, it would be more convenient not to have to.

Also `ag` does not print line numbers:

```
$ echo hi | ag i
hi
```


---

_Comment by @kaushalmodi on 2016-09-28 23:00_

On the other hand, I have aliased `ag` to **always** show the line numbers, using its `--line-numbers` option :) So looks like there is no one _right_ way to do this; it's purely personal choice based.


---

_Comment by @BurntSushi on 2016-09-29 00:03_

I can go both ways here, but I should clarify one thing that is kind of important:

> when using rg in a pipeline

If `rg` is used in a pipeline, then it should do the right thing. Note the difference between these commands:

```
$ rg 'Sherlock Holmes' short.txt 
5:Chapter 1. Mr. Sherlock Holmes
7:Mr. Sherlock Holmes, who was usually very late in the mornings, save

$ cat short.txt | rg 'Sherlock Holmes'
5:Chapter 1. Mr. Sherlock Holmes
7:Mr. Sherlock Holmes, who was usually very late in the mornings, save

$ cat short.txt | rg 'Sherlock Holmes' | cat
Chapter 1. Mr. Sherlock Holmes
Mr. Sherlock Holmes, who was usually very late in the mornings, save
```

The first two show line numbers, but only because `rg` knows it's writing to a tty. In the last one, `rg` is in a pipeline, so it defaults to standard `grep` output.

It does kind of feel like the first two should be consistent with each other. One might argue that this is a little too smart, though.


---

_Label `question` added by @BurntSushi on 2016-09-29 00:03_

---

_Comment by @jzinn on 2016-09-29 00:23_

Oh, I see.  I was building up a pipeline and was looking at the output of `rg` before piping it to the next stage.  I didn't realize that the numbers would go away when the next stage was connected.

The current behavior is good, but somewhat surprising.


---

_Closed by @jzinn on 2016-09-29 00:23_

---

_Comment by @jzinn on 2017-03-16 23:13_

#380

---

_Comment by @BurntSushi on 2017-03-16 23:21_

@jzinn Oh interesting, completely forgot about this ticket! Thanks. :-)

---

_Comment by @jzinn on 2017-03-17 03:28_

@BurntSushi, thanks for this amazing utility.  A core part of my workflow is ripgrep and
```
rg foo -l | xargs perl -i -ple 's/foo/bar/'
```
I type it all day long.


---

_Comment by @dtadrk on 2023-04-28 10:38_

By now you can just use the "-N" flag to suppress line numbers.
Just adding this here because this was still the first hit from Google.

---

_Comment by @mg on 2024-07-11 12:02_

Hi. Is there a way to KEEP the line numbers when redirecting? I am redirecting stdout to a file/pbcopy/vim buffer and I want the line numbers to be in the final output, along with all the new line characters

---

_Comment by @BurntSushi on 2024-07-11 12:09_

@mg Please ask questions in new Discussion posts instead of bumping old issues.

But yes, the `-n/--line-number` flag will do what you want... It's documented in `rg -h` (short) and `rg --help` (long) and `man rg` (if available on your system).

---

_Locked by @ghost on 2024-07-11 12:09_

---
