```yaml
number: 509
title: Possible flag for keeping header row
type: issue
state: closed
author: michaelbarton
labels:
  - question
assignees: []
created_at: 2017-06-07T18:26:13Z
updated_at: 2023-06-02T12:05:37Z
url: https://github.com/BurntSushi/ripgrep/issues/509
synced_at: 2026-01-12T16:13:22Z
```

# Possible flag for keeping header row

---

_@michaelbarton_

Hey @BurntSushi, thanks for making such a neat tool. I've been finding myself typing `rg` more and more at the command line. I wanted to suggest a possible feature for rg that would make it useful for CSV. I tried looking through the documentation, but didn't find that this already exists. Apologies if it does or this is redundant.

I often use rg with large CSV files to subset and pull out the rows that match a certain field. This is useful for multi gigabyte CSV where loading them into R or python is very time consuming otherwise. Using rg to subset only the rows I'm interested in can help reduce the file size significantly. One thing that would help me would be if there was a flag to keep the first row in a CSV, which usually contains the column names. Maybe something like `--keep-first`? This would mean the first row is always printed.

Either way, thanks again for making such a useful tool.

---

_Comment by @BurntSushi on 2017-06-07 18:30_

Thanks for the kind words. Have you tried https://github.com/BurntSushi/xsv instead?

---

_Comment by @michaelbarton on 2017-06-07 18:46_

No, I hadn't heard of this. I will take a look. Thanks!


---

_Comment by @BurntSushi on 2017-10-22 01:33_

I think I'm going to just say no to this one. ripgrep isn't a tool to parse CSV files, even though it may occasionally be useful to use ripgrep on CSV data. Moreover, keeping the first line of a file doesn't even guarantee that it faithfully keeps the first CSV record (since CSV fields can span multiple lines).

---

_Closed by @BurntSushi on 2017-10-22 01:33_

---

_Label `question` added by @BurntSushi on 2017-10-22 01:33_

---

_Comment by @eadmaster on 2021-10-11 10:52_

still, i think this could be useful for other use-cases as well if implemented like this:

E.g.: `--head=3` = print the first 3 lines of each marching file.



---

_Comment by @codaraxis on 2023-06-02 12:05_

This is addressed in greater detail in https://github.com/BurntSushi/ripgrep/issues/461#issuecomment-297461870

Posting because I came across this when searching for the same issue, and this was included in the search hits.

---
