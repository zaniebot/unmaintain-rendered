---
number: 491
title: Redirection of STDOUT fails
type: issue
state: closed
author: workanator
labels: []
assignees: []
created_at: 2016-04-28T18:48:47Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/491
synced_at: 2026-01-10T01:26:30Z
---

# Redirection of STDOUT fails

---

_Issue opened by @workanator on 2016-04-28 18:48_

Hello,

I'm encountering an issue with redirection of STDOUT into a file. For example, I have a shell command

``` shell
download sharefile --path="/new" --list --format=json > "/var/folders/jc/6gbmydn96vb97vbj1wm8q77w0000gn/T/KmiUAWdmuE"
```

and I get this error message

``` text
error: Found argument '> "/var/folders/jc/6gbmydn96vb97vbj1wm8q77w0000gn/T/KmiUAWdmuE"' which wasn't expected, or isn't valid in this context
```

Am I doing something wrong or do I miss some important section in the documentation? Thanks.


---

_Comment by @workanator on 2016-04-28 18:52_

Sorry for the spam. The issue was in Perl `system` arguments.


---

_Closed by @workanator on 2016-04-28 18:52_

---

_Comment by @kbknapp on 2016-04-28 23:35_

No problem :smile:


---
