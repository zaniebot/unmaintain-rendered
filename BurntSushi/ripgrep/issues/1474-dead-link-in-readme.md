```yaml
number: 1474
title: Dead link in Readme
type: issue
state: closed
author: atouchet
labels:
  - bug
assignees: []
created_at: 2020-01-28T01:35:50Z
updated_at: 2020-03-15T17:25:31Z
url: https://github.com/BurntSushi/ripgrep/issues/1474
synced_at: 2026-01-12T16:13:23Z
```

# Dead link in Readme

---

_@atouchet_

#### Describe your question, feature request, or bug.

In the Readme the link to http://opus.lingfil.uu.se/OpenSubtitles2016/mono/OpenSubtitles2016.raw.en.gz no longer works. I am not sure if the file was moved to a new URL or is no longer available.

---

_Comment by @BurntSushi on 2020-01-28 11:30_

Drats, okay. I'll either upload my copy of it somewhere or redo the benchmarks with the 2018 data set.

If you want to test on the 2018 English subtitles, then the link for that is: http://opus.nlpl.eu/download.php?f=OpenSubtitles/v2018/mono/OpenSubtitles.raw.en.gz

---

_Label `bug` added by @BurntSushi on 2020-01-28 11:30_

---

_Comment by @rdp on 2020-02-05 17:49_

I also noticed the link to "ack has a [bug]"  says it has been fixed or some odd, FWIW... :)

---

_Comment by @BurntSushi on 2020-02-05 17:56_

That doesn't seem related to this issue. And the link is correct. The bug is wontfix for ack2. The right thing to do here is to update the benchmarks to use ack3.

---

_Closed by @BurntSushi on 2020-03-15 17:25_

---
