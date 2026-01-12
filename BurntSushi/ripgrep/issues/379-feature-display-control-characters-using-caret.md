```yaml
number: 379
title: "Feature: Display control characters using caret notation"
type: issue
state: closed
author: casey
labels:
  - question
  - icebox
assignees: []
created_at: 2017-02-24T00:26:35Z
updated_at: 2018-01-31T02:55:19Z
url: https://github.com/BurntSushi/ripgrep/issues/379
synced_at: 2026-01-12T16:13:21Z
```

# Feature: Display control characters using caret notation

---

_@casey_

I was using `rg` today and it started printing results from a binary file.

The file in question starts with a bunch of text, but ends with an inline zip file, which I think confused ripgrep's binary file type detection.

The results included a lot of control characters, which hosed my terminal.

Would it be possible to print control characters in the output using caret notation? In the general case, I'm not sure that non-printable control characters are useful in results, so it might be worth it to turn this on by default, but it could also live behind a flag.

---

_Comment by @BurntSushi on 2017-02-24 00:44_

Do other tools do this? e.g., grep? If grep detects your file as binary, then I'd like to learn how ripgrep can make its detection better.

I'm pretty hesitant to go down the path of completely rewriting output results, but I agree that messing up your terminal is not a pleasant experience.

---

_Label `question` added by @BurntSushi on 2017-02-24 00:44_

---

_Comment by @casey on 2017-02-24 01:00_

Grep does indeed detect it as binary.

Using `grep -r pattern .` I get `Binary file ./weird-file matches`

How does ripgrep's binary file type detection currently work?

The issue is that this particular file starts with a bunch of plain test, so any heuristic that uses the beginning of the file will fail here.

---

_Comment by @casey on 2017-02-24 01:02_

The file utility seems to notice that it contains binary:

```
$ file wierd-file
wierd-file: Bourne-Again shell script executable (binary data)
```

---

_Comment by @BurntSushi on 2017-02-24 01:43_

@casey Binary detection is basically "find a `NUL` byte." ripgrep had a bug (that's fixed on master) where it only looked at the first 1KB or so for a `NUL` byte. Unfortunately, that bug is only fixed when buffered reading is used. If ripgrep decides to use memory maps, then I think it only examines the first 10KB.

---

_Comment by @casey on 2017-02-24 05:28_

Hmmm, in that case I think there is always a risk that it will misdetect the type of a file.

Although it sounds weird, I think this situation comes up more often than one might expect. In this case, the file is a self-extracting python binary. The file is a shell script followed by a zip archive, where the shell script extracts the zip archive.

This gets a lot of use at both facebook and google (I'm a current facebook employee and ex google employee), and seems to be used in chromeos: https://chromium.googlesource.com/chromiumos/platform/factory/+/stabilize-4068.0.B/py/tools/make_par.py (not sure for what though)


---

_Label `icebox` added by @BurntSushi on 2017-04-09 13:12_

---

_Comment by @BurntSushi on 2018-01-31 02:55_

I'm going to close this. My feeling is that this isn't ripgrep's responsibility, and that it should be possible to pipe output of ripgrep into another program that does this sanitization for you. One might argue that this limits the utility of such a feature since you generally don't know whether your terminal is going to get hit with a bunch of control sequences. But this would be true if we did it in ripgrep as well. In particular, there is no way we could enable such a filter by default because of the performance impact that would have. So it would need to be opt-in.

---

_Closed by @BurntSushi on 2018-01-31 02:55_

---
