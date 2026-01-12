```yaml
number: 471
title: Encoding issue when piping rg output
type: issue
state: closed
author: elirnm
labels: []
assignees: []
created_at: 2017-05-02T08:50:40Z
updated_at: 2017-05-02T10:48:22Z
url: https://github.com/BurntSushi/ripgrep/issues/471
synced_at: 2026-01-12T16:13:22Z
```

# Encoding issue when piping rg output

---

_@elirnm_

There seems to be an encoding issue when piping the output of a ripgrep search. The characters `ø æ å ä` (and probably others, those are just the ones I've tested with) are converted to `??` when being piped to anything.

It seems to only be with piping; passing stdout to a file with `>` works fine (at least in cmd.exe where files are created with utf8).

![capture](https://cloud.githubusercontent.com/assets/19817004/25610476/32325c72-2ed8-11e7-8c12-443fb138567f.PNG)

This is what I used to test this.
```
mkdir rgtest
cd rgtest
echo "testøæåä" > strtest.txt
mkdir testø
echo "blahblah" > testø/testå.txt
```

Then search for various things as seen in the picture. (The `--colors` option is unrelated; that's just so you can see the filenames in Powershell).

This is on Windows 7 with ripgrep 0.5.1. I've recreated this in both powershell and cmd.exe, but I don't have a Linux install to test it there.

---

_Comment by @BurntSushi on 2017-05-02 09:44_

Can you provide the raw bytes of strtest.txt?

---

_Comment by @elirnm on 2017-05-02 09:58_

```
116
101
115
116
195
184
195
166
195
165
195
164
13
10
```

If that's not what you wanted you'll have to give me more specific instructions.

The file is currently UTF-8 encoded but this issue occurs with other file encodings as well (and when not reading from a file, as seen in the `--files` examples.

---

_Comment by @elirnm on 2017-05-02 10:28_

Hmm, this actually doesn't look like ripgrep. Piping these characters is succeeding with every native Powershell command, but failing with every call to an executable or any non-built-in program.

![cap](https://cloud.githubusercontent.com/assets/19817004/25614555/4b3a87bc-2ee7-11e7-882e-965428a56dec.PNG)

And while I seem to remember it failing in cmd.exe before, it actually seems to work fine there.

Sorry not looking into this more before reporting.

---

_Closed by @elirnm on 2017-05-02 10:48_

---
