```yaml
number: 34
title: Add an option similar to -o, --only-matching
type: issue
state: closed
author: dzamlo
labels:
  - enhancement
  - libripgrep
assignees: []
created_at: 2016-09-23T20:38:39Z
updated_at: 2017-07-13T00:16:37Z
url: https://github.com/BurntSushi/ripgrep/issues/34
synced_at: 2026-01-12T16:13:21Z
```

# Add an option similar to -o, --only-matching

---

_@dzamlo_

With grep you can print only the matched parts of the files. The option is described like this in the grep manpage:

```
-o, --only-matching
       Print only the matched (non-empty) parts of a matching line, with each such part on a separate output line.
```

A more powerful option, would something similar to `--replace`, but that doesn't print the non matched part of the text. 


---

_Comment by @cwillu on 2016-09-24 00:57_

Note that surrounding the search regex with ._(  )._ and then using --replace $1 will do this; pretty verbose though, especially when using it in a pipeline.


---

_Comment by @BurntSushi on 2016-09-24 02:06_

`--replace` already exists (and I agree it's kind of verbose for this case).

`-o`/`--only-matching` is an option on `grep`, so I'm OK adding this.


---

_Label `enhancement` added by @BurntSushi on 2016-09-24 02:07_

---

_Comment by @sergeevabc on 2016-12-15 04:49_

Input
```
host -t A wikipedia.org | openssl dgst -sha256 | rg --no-line-number [A-Fa-f0-9]{64}
```
Output
```
(stdin)= 0d00409623da8ae039de83200af7f6fa9211bb13e1b3e10ea45142cd08b3e50b
```
Hash is coloured red, but I need hash (that red part) only.
Would the considered option ```--only-matching``` be of help?

---

_Comment by @BurntSushi on 2016-12-15 11:48_

@sergeevabc Yes, it would solve your problem. But there is a work-around for now:

```
$ host -t A wikipedia.org | openssl dgst -sha256 | rg -N '^.*([A-Fa-f0-9]{64}).*$' --replace '$1'
```

---

_Comment by @sergeevabc on 2016-12-15 20:04_

@BurntSushi, thanks, but when ```--only-matching``` will be implemented?

---

_Comment by @BurntSushi on 2016-12-15 20:05_

@sergeevabc I don't know. It happens when it happens. I do this in my free time and I don't typically schedule my free time in advance. Sorry.

---

_Comment by @gavinfoley on 2016-12-15 20:29_

To counterbalance the above comment, I wanted to say that I use and enjoy this excellent tool you've created every day, and I appreciate whatever improvements are made whenever they are made. Thanks for using some of your limited free time to make my life and others' easier. 

---

_Comment by @BurntSushi on 2016-12-15 21:00_

@sergeevabc This issue tracker isn't place to give me advice on how to schedule my free time or to shame anyone into doing work on their free time. Future comments in that vain won't be tolerated.

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Label `libripgrep` added by @BurntSushi on 2017-01-11 03:04_

---

_Closed by @BurntSushi on 2017-04-09 12:48_

---

_Comment by @22a on 2017-04-09 12:52_

Wonderful, thank you for the great work @kpp + @BurntSushi 🎉 

---
