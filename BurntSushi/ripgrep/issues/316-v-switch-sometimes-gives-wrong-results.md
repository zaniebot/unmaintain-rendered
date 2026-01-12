```yaml
number: 316
title: "-v switch sometimes gives wrong results"
type: issue
state: closed
author: stej
labels:
  - question
assignees: []
created_at: 2017-01-12T15:15:43Z
updated_at: 2017-01-13T11:02:59Z
url: https://github.com/BurntSushi/ripgrep/issues/316
synced_at: 2026-01-12T16:13:21Z
```

# -v switch sometimes gives wrong results

---

_@stej_

I still don't have some reliable repro, but I'll try at least..

I have this search: 
`rg 30193293 -g 2017-01-09* | rg -v "Logging inputStream from mail service|notifications received"`
When I run it only on several (about 10) files, it returns correct results. But when I run it on a lot of files (log files for one month), then it gives weird results.  `rg 30193293 -g 2017-01-09* -c` returns results from 6 files, but when I add the pipe and `-v`, it returns results from files where string "30193293" is not contained. Any idea, how to make a reliable repro, if this is a bug?

---

_Comment by @stej on 2017-01-12 15:15_

Btw. I'm on windows.

---

_Comment by @BurntSushi on 2017-01-12 15:35_

From your description, it sounds like you're not saying that `-v` is wrong, but that the initial search is wrong. Namely, if you're seeing results without the string `30193293`, then doesn't that mean the *first* `rg` command reported a line as a match that wasn't a match? If so, that's where I'd start with a repro.

For example, compare the output of your command:

```
$ rg 30193293 -g 2017-01-09* | rg -v "Logging inputStream from mail service|notifications received"
```

with this

```
$ rg 30193293 -g 2017-01-09* | grep -v "Logging inputStream from mail service|notifications received"
```

and this

```
$ grep 30193293 --include 2017-01-09* | grep -v "Logging inputStream from mail service|notifications received"
```

---

_Label `question` added by @BurntSushi on 2017-01-13 01:43_

---

_Comment by @stej on 2017-01-13 08:17_

I didn't write it clearly. I think there is no problem with the first run of `rg 30193293 ` that returns correct results. It seems that combination of pipe and `-v` option doesn't work in some cases. 

I tried 

```
grep.exe 30193293 2017-01-09* | grep.exe -v "Logging inputStream from mail service|notifications received" > %~dp0grep-grep.out.txt

rg 30193293 -g 2017-01-09* | grep.exe -v "Logging inputStream from mail service|notifications received" > %~dp0rg-grep.out.txt

rg 30193293 -g 2017-01-09* | rg -v "Logging inputStream from mail service|notifications received" > %~dp0rg-rg.out.txt
```

and the first 2 commands give me the same results (after some sorting).

I killed the the last command when rg-rg.out.txt had 10GB size. And indeed I inspected the output file rg-rg.out.txt and it looks like the `-v` ignores the input from pipe and runs the search in the current directory for all the files. There was no line that would match pattern `"Logging inputStream from mail service|notifications received"`.

---

_Comment by @garoto on 2017-01-13 10:00_

What version are you running? Not so long ago, piping any output to `rg` was broken and it would behave exactly like that.

---

_Comment by @stej on 2017-01-13 11:02_

Yes, you are right. I had 2 versions on my machine. One on the path, the other one diretly in the directory hidden behind a lot of files..
Versions 0.3.2 vs. 0.2.6.

Problem solved. Thank you for your time.

---

_Closed by @stej on 2017-01-13 11:02_

---
