```yaml
number: 209
title: Added taskpaper as a file type
type: pull_request
state: merged
author: dueyfinster
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2016-11-01T14:00:15Z
updated_at: 2016-11-01T14:29:52Z
url: https://github.com/BurntSushi/ripgrep/pull/209
synced_at: 2026-01-12T18:23:12Z
```

# Added taskpaper as a file type

---

_@dueyfinster_

_No description provided._

---

_Comment by @BurntSushi on 2016-11-01 14:07_

I've never heard of this format. Is it plain text? Is it widely used?


---

_Comment by @dueyfinster on 2016-11-01 14:11_

Hi, yes it's plain text. Here's what it looks like:

https://github.com/rstacruz/tps_reporter/blob/master/data/sample.taskpaper

Here's the spec:

> TaskPaper’s file format is fairly simple. Here’s how TaskPaper reads a file:
> • Files are expected to use the UTF-8 encoding and use ‘\n’ to separate lines.
> • A task is a line that begins with a hyphen followed by a space ('- ') which can
> optionally be prefixed (i.e indented) with tabs or spaces. A task can have zero or
> more tags anywhere on the line (not just trailing at the end).
> • A project is a line that isn't a task and ends with a colon (':'), or a colon (':\n')
> followed by a newline. Tags can exist after the colon, but if any non-tag text is
> present, then it won’t be recognized as a project.
> • A note is any line that doesn't match the task or project rules.
> • Indentation level (with tabs, not spaces) defines ownership. For instance, if you
> indent one task under another task, then it is considered a subtask. Tasks and
> notes own all objects that are indented underneath them. Empty lines are
> ignored when calculating ownership.
> • A tag has the form "@tag", i.e. it starts with an "at" character ("@"), followed by a
> run of non-whitespace characters. A tag can optionally have a value assigned to
> it. The value syntax immediately follows the tag word (no whitespace between)
> and is enclosed by parentheses: '(' and ')'. The value text inside can have
> whitespace, but no newlines. Here is an example of a tag with a value:
> @tag(tag's value)

Ref: http://imissmymac.com/wp-content/uploads/2013/02/TaskPaper-Users-Guide.pdf

Also a lot of other applications support it:
http://support.hogbaysoftware.com/t/what-other-apps-support-taskpapers-file-format/1114

It comes up a lot for a search on Github (in shell aliases etc), over 133 repos and 21,409 code results:
https://github.com/search?q=taskpaper&type=Code&utf8=%E2%9C%93

I think it's worthy of inclusion, in summary. Cheers!


---

_Merged by @BurntSushi on 2016-11-01 14:29_

---

_Closed by @BurntSushi on 2016-11-01 14:29_

---

_Comment by @BurntSushi on 2016-11-01 14:29_

Good enough for me! Thanks!


---

_Comment by @dueyfinster on 2016-11-01 14:29_

Cheers :+1: 


---

_Branch deleted on 2016-11-01 14:29_

---
