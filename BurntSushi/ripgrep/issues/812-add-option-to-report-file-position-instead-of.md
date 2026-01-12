```yaml
number: 812
title: Add option to report file position instead of line number
type: issue
state: closed
author: David-Gould
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2018-02-17T06:59:01Z
updated_at: 2018-03-10T15:15:38Z
url: https://github.com/BurntSushi/ripgrep/issues/812
synced_at: 2026-01-12T16:13:22Z
```

# Add option to report file position instead of line number

---

_@David-Gould_

#### What version of ripgrep are you using?

ripgrep 0.8.0 (rev 23d1b91ead)
+SIMD -AVX

#### What operating system are you using ripgrep on?

Ubuntu 16.04

#### Describe your question, feature request, or bug.

Please add a feature to report the file offset of the lines containing matches. GNU grep has this

```
       -b, --byte-offset
              Print the 0-based byte offset within the  input  file  before  each  line  of  output.   If  -o
              (--only-matching) is specified, print the offset of the matching part itself.

```

I have a process that generates terabytes of diagnostic output daily. I desire to scan all that
for certain patterns and then to have a script read forward from the lines the patterns are on.
It would be much faster to fseek() to the offset of each matching line than to read all the data
counting lines. rg would be the perfect tool for generating the list of interesting offsets if it offered
that functionality. Thanks.



---

_Label `enhancement` added by @BurntSushi on 2018-02-17 13:24_

---

_Label `help wanted` added by @BurntSushi on 2018-02-17 13:24_

---

_Comment by @BurntSushi on 2018-02-17 13:25_

Definitely agree with this request.

---

_Comment by @balajisivaraman on 2018-02-18 05:04_

The spec for this looks quite straightforward. Duplicate what GNU Grep does, which is the following just to be clear:

```
$ echo -e "foo foo\nsomething else bar bar\nbaz baz" > grep.txt
$ echo -e "foo foo\nsomething else bar bar\nbaz baz" > grep1.txt
$ grep -b 'bar' grep.txt (No filename for a single file, prints offset of line)
8:something else bar bar
$ grep -bo 'bar' grep.txt (No filename for a single file, prints offset of every occurrence)
23:bar
27:bar
$ grep -b 'bar' *.txt (Prints filename, followed by offset of matching line and then line itself)
grep1.txt:8:something else bar bar
grep.txt:8:something else bar bar
$ grep -bo 'bar' *.txt (Prints filename, followed by offset of every occurrence and then the occurrence itself)
grep1.txt:23:bar
grep1.txt:27:bar
grep.txt:23:bar
grep.txt:27:bar
```

---

_Comment by @balajisivaraman on 2018-02-18 12:22_

Another question, how should this flag interact with `--line-number` and `--column`. Currently, the latter implies the former. If we add `-b` into the mix, then that will be three values printed before the actual match itself.

In my current implementation, which is complete, this is how the output looks like with all 3 flags. The line number is colored in Green by default, and the match is colored in Red. The column number and byte offset are white. Are we good to go with this as the default?

```
$ ./target/release/rg --line-number --column -bo 'bar' grep.txt
2:16:23:bar
2:20:27:bar
```

GNU Grep does not have `--column`, but if I combine `--line-number` and `--byte-offset`, this is what I get.

```
$ grep --line-number -bo 'bar' grep.txt
2:23:bar
2:27:bar
```

---

_Comment by @David-Gould on 2018-02-18 21:21_

I don't see the colors here, but it looks like the order when all three are requested is line:column:offset: Fine with me, I"m most interested in the '-b' case so I don't have a strong preference. But it seems sensible to have line and column output adjacent.

---

_Closed by @BurntSushi on 2018-03-10 15:15_

---
