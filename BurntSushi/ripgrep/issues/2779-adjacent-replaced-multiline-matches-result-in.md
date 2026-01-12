```yaml
number: 2779
title: Adjacent replaced multiline matches result in wrong line numbers
type: issue
state: open
author: meedstrom
labels:
  - bug
assignees: []
created_at: 2024-04-11T10:51:57Z
updated_at: 2024-11-29T09:43:03Z
url: https://github.com/BurntSushi/ripgrep/issues/2779
synced_at: 2026-01-12T16:13:24Z
```

# Adjacent replaced multiline matches result in wrong line numbers

---

_@meedstrom_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


### How did you install ripgrep?

APT

### What operating system are you using ripgrep on?

Kubuntu 23.10

### Describe your bug.

This is similar to #2420, and I understand why that's WONTFIX. This is different though.

Using a multiline regexp, when the regexp matches strings that come immediately one after another, it bungles the line numbers of all of them.  Easier to show you with a reproduction example:

### What are the steps to reproduce the behavior?

Save a file `test.txt` containing:

```
:properties:
:id: fnord
:end:
:properties:
:id: boccob
:end:
:properties:
:id: d321fdddffff
:end:
:properties:
:id: clowns
:end:
```

Then run 

```
rg -nU '^:properties:\n:id: (.*)\n:end:' -r '$1' test.txt
```

### What is the actual behavior?

The result is 

```
1:fnord
2:boccob
3:d321fdddffff
4:clowns
```

Only the first hit is correct.

You can see that the line numbers will be correctly reported if you modify the file to add a newline after each instance of ":end:". 

### What is the expected behavior?

Expected the result:

```
1:fnord
4:boccob
7:d321fdddffff
10:clowns
```

---

_Comment by @BurntSushi on 2024-04-11 11:33_

> Using a multiline regexp, when the regexp matches strings that come immediately one after another, it bungles the line numbers of all of them.

This is an incomplete description of the problem you're reporting. It isn't _just_ when a regex matches strings that are adjacent, it's _also_ required that you use the `-r/--replace` flag to replace matches with something else. This can be easily demonstrated by omitting the `-r/--replace` flag and observing that the line numbers are correct:

```
$ rg -nU '^:properties:\n:id: (.*)\n:end:' test.txt
1::properties:
2::id: fnord
3::end:
4::properties:
5::id: boccob
6::end:
7::properties:
8::id: d321fdddffff
9::end:
10::properties:
11::id: clowns
12::end:
```

Indeed though, the adjacency part of this seems important. If instead I use the following haystack as `test2.txt`:

```
:properties:
:id: fnord
:end:
ZZZ
:properties:
:id: boccob
:end:
ZZZ
:properties:
:id: d321fdddffff
:end:
ZZZ
:properties:
:id: clowns
:end:
```

Then the line numbers change:

```
$ rg -nU '^:properties:\n:id: (.*)\n:end:' -r '$1' test2.txt
1:fnord
5:boccob
9:d321fdddffff
13:clowns
```

---

_Label `bug` added by @BurntSushi on 2024-04-11 11:33_

---

_Comment by @meedstrom on 2024-04-25 06:04_

As you say, I forgot to mertion the `--replace`. Changed the title.

---

_Renamed from "Consecutive multiline matches mis-report line numbers" to "Adjacent replaced multiline matches result in wrong line numbers" by @meedstrom on 2024-04-25 06:06_

---

_Comment by @meedstrom on 2024-04-26 17:07_

I'm not proficient with Rust, but I could try to look for the problem. Any pointers on where to look?

---

_Comment by @BurntSushi on 2024-04-26 17:08_

Likely in `grep-printer`. Look at the "standard" printer.

That's where I would start anyway.

---

_Comment by @WalterScottYoung on 2024-11-29 09:32_

It seems that the problem is we didn't keep track of line number when doing replace using regex package.

Within a call of StandardSink::replace, if there are multiple matches we didn't record the line number corresponding to each matches, instead we output the data after replace to sink, in which the line number was counted by number of '\n' which is wrong because some '\n' was modified when we do StandardSink::replace.

---

_Comment by @WalterScottYoung on 2024-11-29 09:42_

With that said, it minght be hard to get the line number of the replaced result, and it requires some changes in apis to pass  the corresponding line number of each replaced result to sink to be used in printing. I wonder would it be easier to do what the the documentation says (https://github.com/BurntSushi/ripgrep/issues/2852), and don't print line number but print index of matches in multi-line search.

---
