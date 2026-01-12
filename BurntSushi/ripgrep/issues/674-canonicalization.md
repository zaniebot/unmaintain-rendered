```yaml
number: 674
title: Canonicalization
type: issue
state: closed
author: horse-latitudes
labels:
  - question
assignees: []
created_at: 2017-11-11T15:44:08Z
updated_at: 2017-11-11T16:28:59Z
url: https://github.com/BurntSushi/ripgrep/issues/674
synced_at: 2026-01-12T16:13:22Z
```

# Canonicalization

---

_@horse-latitudes_

can ripgrep be used with canonize? I'm trying to find the best way of running a long list of search-and-replaces on a large number of files.

Any help appreciated.

Cheers Phil.

---

_Label `question` added by @BurntSushi on 2017-11-11 15:46_

---

_Comment by @BurntSushi on 2017-11-11 15:46_

I don't understand the question you're asking. I don't know what "canonize" is. Please provide more details. Please include examples.

---

_Comment by @BurntSushi on 2017-11-11 15:46_

> I'm trying to find the best way of running a long list of search-and-replaces on a large number of files.

`find ... | sed -i ...`

---

_Comment by @horse-latitudes on 2017-11-11 15:51_

it exploits the .. path specifier to traverse back up the directory hierarchy in an attempt to search a file

although I don't know why I didn't consider sed. Just want to use ripgrep I suppose 

---

_Comment by @horse-latitudes on 2017-11-11 15:53_

Wikipedia puts it better than me:

[https://en.wikipedia.org/wiki/Canonicalization](url)

---

_Comment by @BurntSushi on 2017-11-11 16:10_

I still don't understand. **Please provide an example and explicitly describe the problem you are trying to solve.** I have no earthly idea what canonicalization as a general concept has to do with search and replace.

---

_Comment by @horse-latitudes on 2017-11-11 16:28_

OK thanks to your suggestion I've achieved what I set out to do using awk.

Sorry for the noise.

---

_Closed by @horse-latitudes on 2017-11-11 16:28_

---
