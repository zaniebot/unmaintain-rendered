```yaml
number: 1791
title: Support apply smart case to each individual search pattern separately
type: issue
state: open
author: xaljer
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2021-01-31T03:09:55Z
updated_at: 2021-07-10T15:12:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1791
synced_at: 2026-01-12T16:13:24Z
```

# Support apply smart case to each individual search pattern separately

---

_@xaljer_

#### Describe your feature request

ripgrep support to search several patterns together by `-e` or listing patterns in a file and using `-f` to specify the file. 
Meanwhile, `--smart-case` option can judge search pattern if it needs a case sensitive searching. But, for now, all patterns are treated together to match the smart case rule. i.e. if I use `rg -S -e Abc -e bcd`, search pattern `bcd` also be treated as case sensitive. 
So, I think it's better to support apply smart case rule to every search pattern separately, especially in the `-f` usage. But I am not sure if patterns in a single regex should support this.

I think ripgrep's man page for this change would be like:
```
 -S, --smart-case
            Searches case insensitively if the pattern is all lowercase. Search case
            sensitively otherwise. Every individual search pattern follow this rule 
            by itself, including PATTERN specified by -e and patterns in PATTERNFILE
            specified by -f.
```

---

_Label `enhancement` added by @BurntSushi on 2021-06-01 00:14_

---

_Label `help wanted` added by @BurntSushi on 2021-06-01 00:14_

---

_Comment by @FinnRG on 2021-07-10 15:08_

The case-insensitive flag also behaves like that. Should this be changed for every flag?

---

_Comment by @BurntSushi on 2021-07-10 15:12_

The `-i` flag does not behave like this. The key here is that `-S` has to actually look at the pattern to determine whether to enable the mode or not. `-i` does not, as it is unconditional.

---
