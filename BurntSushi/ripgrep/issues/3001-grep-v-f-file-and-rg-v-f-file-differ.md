```yaml
number: 3001
title: "`grep -v -f file` and `rg -v -f file` differ"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
  - rollup
assignees: []
created_at: 2025-02-26T16:10:00Z
updated_at: 2025-09-20T01:08:35Z
url: https://github.com/BurntSushi/ripgrep/issues/3001
synced_at: 2026-01-12T16:13:25Z
```

# `grep -v -f file` and `rg -v -f file` differ

---

_@BurntSushi_

While I don't maintain any guarantees with grep specifically, it was my intent to match its behavior here. Moreover, ripgrep's behavior is internally inconsistent:

```
$ echo wat | rg -f /dev/null
$ echo wat | rg -v -f /dev/null
$ echo wat | grep -f /dev/null
$ echo wat | grep -v -f /dev/null
wat
$ echo wat | rg -f <(echo)
wat
$ echo wat | rg -v -f <(echo)
$ echo wat | grep -f <(echo)
wat
$ echo wat | grep -v -f <(echo)
$
```

Specifically, the fact that the first two commands above _both_ report no results is wrong.

### Discussed in https://github.com/BurntSushi/ripgrep/discussions/3000

<div type='discussions-op-text'>

<sup>Originally posted by **datatraveller1** February 26, 2025</sup>
I want to list all found values from a search and all not found values from a search.
The following is a simplified example. The values searched for are listed in the file search_for.txt.

**search_for.txt:**
```
1
2
3
```
The file in which these values are searched is named searched_in.txt.

**searched_in.txt:**
```
item 1
item 2
```
I use the following commands (note that I'm on MS Windows):

**find entries of file search_for.txt:**
```
rg --file=search_for.txt --only-matching searched_in.txt > found.txt
```
**not found entries of file search_for.txt:**
```
rg --invert-match --file=found.txt search_for.txt > not_found.txt
```
The result is correct:

**found.txt:**
```
1
2
```
**not_found.txt:**
```
3
```

Great so far.
However, if I change the content of file search_for.txt so nothing is found...
**search_for.txt:**
```
4
5
6
```
... with the two above commands, the file **found.txt** gets created as a **0 bytes file** (which is correct, because nothing is found) but the file **not_found.txt** gets also created as a **0 bytes file** which doesn't match my expectations.
I have wished to see all values of file search_for.txt as result for the command
 _rg --invert-match --file=found.txt search_for.txt > not_found.txt_
**not_found.txt, wished result:**
```
4
5
6
```
Isn't the inversion of nothing everything?
If this is the expected behaviour, is there a special flag to achieve what I want for --file and 0 bytes content of this file?</div>

---

_Label `bug` added by @BurntSushi on 2025-02-26 16:10_

---

_Label `rollup` added by @BurntSushi on 2025-07-27 19:13_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
