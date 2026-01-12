```yaml
number: 259
title: Ignore tags file
type: issue
state: closed
author: bluexnine
labels:
  - question
assignees: []
created_at: 2016-12-01T07:22:03Z
updated_at: 2016-12-01T15:11:33Z
url: https://github.com/BurntSushi/ripgrep/issues/259
synced_at: 2026-01-12T16:13:21Z
```

# Ignore tags file

---

_@bluexnine_

I can't seem to find an elegant way to ignore 'tags' or 'TAGS' files. I have tried adding tags, *tags*, *.tags, tags* to .ignore file, add -g '!tags' (with various wildcard regex around tags), and I even tried making a filetype and ignoring it by using "rg <pattern> --type-add 'tag: *{tags} *" (with various wildcard regex around tags). My tags file is fairly large and causes useless a large amount of search hits. I was using Ag and was looking for an alternative that would resolve this problem. Apparently ack has the same issue. Please let me know if you know of a way to resolve this or add this feature in future release.

For now I am resolving the issue by generating my tags file as .tags.

---

_Comment by @BurntSushi on 2016-12-01 12:07_

What version of ripgrep are you using? Could you please provide a minimally reproducible test case that demonstrates your problem? Can you please include the output of running `--debug` with `rg`?

For example, I cannot reproduce your problem from your description:

```
$ echo foo > tags
$ echo foo > test
$ mkdir bar
$ echo foo > bar/tags
$ rg foo
test
1:foo

tags
1:foo

bar/tags
1:foo
$ echo tags > .ignore
$ rg foo
test
1:foo
```

The `-g` flag works:

```
$ rm .ignore
$ rg foo
test
1:foo

tags
1:foo

bar/tags
1:foo
$ rg -g '!tags' foo
test
1:foo
```

So does the `--type-add` flag (note that your definition is *really* weird, here's something simpler):

```
$ rg --type-add 'tag:tags' -Ttag foo
test
1:foo
```

---

_Label `question` added by @BurntSushi on 2016-12-01 12:08_

---

_Comment by @bluexnine on 2016-12-01 15:09_

I realize what i was doing wrong. I used an astrix at the end which cause rg to search all files. Thanks for your help.



---

_Closed by @BurntSushi on 2016-12-01 15:11_

---

_Comment by @BurntSushi on 2016-12-01 15:11_

Thanks! I like bugs that fix themselves. :-)

---
