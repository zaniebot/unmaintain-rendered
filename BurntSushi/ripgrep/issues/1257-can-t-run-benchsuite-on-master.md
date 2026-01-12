```yaml
number: 1257
title: "Can't run benchsuite on master"
type: issue
state: closed
author: In-line
labels:
  - bug
assignees: []
created_at: 2019-04-17T20:35:07Z
updated_at: 2020-10-14T21:02:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1257
synced_at: 2026-01-12T16:13:23Z
```

# Can't run benchsuite on master

---

_@In-line_

```
$ /benchsuite --download subtitles-ru    
# gunzip /home/alik/CLionProjects/ripgrep/benchsuite/subtitles/OpenSubtitles2016.raw.ru.gz

gzip: /home/alik/CLionProjects/ripgrep/benchsuite/subtitles/OpenSubtitles2016.raw.ru.gz: not in gzip format

```

---

_Comment by @In-line on 2019-04-17 20:36_

```
$ cat /home/alik/CLionProjects/ripgrep/benchsuite/subtitles/OpenSubtitles2016.raw.ru.gz 
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL /OpenSubtitles2016/mono/OpenSubtitles2016.raw.ru.gz was not found on this server.</p>
<hr>
<address>Apache/2.4.7 (Ubuntu) Server at opus.nlpl.eu Port 80</address>
</body></html>

```

---

_Comment by @BurntSushi on 2019-04-17 20:55_

Looks like that URL is dead. Try `https://object.pouta.csc.fi/OPUS-OpenSubtitles/v2016/mono/ru.txt.gz` instead.

The English subtitle URL is also dead. Try `https://object.pouta.csc.fi/OPUS-OpenSubtitles/v2016/mono/en.txt.gz`.

---

_Label `bug` added by @BurntSushi on 2019-04-17 20:55_

---

_Closed by @BurntSushi on 2020-10-14 21:02_

---
