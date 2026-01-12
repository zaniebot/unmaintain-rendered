```yaml
number: 1665
title: "[FR] Add option to include the filename in search"
type: issue
state: closed
author: NightMachinery
labels:
  - duplicate
assignees: []
created_at: 2020-08-26T15:03:09Z
updated_at: 2020-08-26T15:31:09Z
url: https://github.com/BurntSushi/ripgrep/issues/1665
synced_at: 2026-01-12T16:13:24Z
```

# [FR] Add option to include the filename in search

---

_@NightMachinery_

#### Describe your feature request

I use ripgrep to search my notes. I also use tags in my  notes to make searching for stuff easy. Sometimes I include the tag in the filenames. So I want to be able to include the filename in the search.

As an example, I can have these files:

```
FILE1: notes/plants/hi.md

- the jungle
```

```
FILE2: notes/todo/@perf.md

- A Lion
- A basket
```

```
FILE3: notes/sth.md

- A man
- @perf A tiger
```

Then I want to do a search on `@perf` that would return:

```
notes/todo/@perf.md:- A lion
notes/todo/@perf.md:- A Basket
notes/sth.md:- @perf A tiger
```

I know I can already simulate this by first transforming all the lines of these files to `FILENAME: LINE_CONTENT` manually, and then using ripgrep on them, but that would be be much slower than ripgrep supporting this natively, no? And speed is of paramount importance in searching.


---

_Comment by @BurntSushi on 2020-08-26 15:31_

Duplicate of #1034.

Probably the best way to solve your problem is to run two searches. One for searching the contents of files and one that just searches file names:

```
$ rg @perf
$ rg '.' -g '*@perf*'
```

Unless you have a truly massive directory tree, this shouldn't be too slow. The latter command, for example, will only search a subset of files matching `@perf`.

---

_Closed by @BurntSushi on 2020-08-26 15:31_

---

_Label `duplicate` added by @BurntSushi on 2020-08-26 15:31_

---
