```yaml
number: 3095
title: Unzip fails on ripgrep-14.1.1-x86_64-pc-windows-msvc.zip
type: issue
state: closed
author: friendly
labels:
  - invalid
assignees: []
created_at: 2025-07-06T19:41:37Z
updated_at: 2025-07-06T20:30:20Z
url: https://github.com/BurntSushi/ripgrep/issues/3095
synced_at: 2026-01-12T16:13:25Z
```

# Unzip fails on ripgrep-14.1.1-x86_64-pc-windows-msvc.zip

---

_@friendly_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

rpigrep 14.1.1

### How did you install ripgrep?

I simply downloaded the zip file and tried to unzip it

### What operating system are you using ripgrep on?

Windows 10

### Describe your bug.

I downloaded `ripgrep-14.1.1-x86_64-pc-windows-msvc.zip` to install on my Windows 10 laptop. When I open this with unzip, I see what looks like a proper set of folders and files.

<img width="890" height="352" alt="Image" src="https://github.com/user-attachments/assets/b4560d66-9234-4bf2-b4b4-5d6c32a12e42" />

But cliciking Unzip gives errors whatever I try (e.g., unzipping the whole file or just the separate folders)


```
Extracting to "C:\Dropbox\incoming\ripgrep-14.1.1-x86_64-pc-windows-msvc\"
Use Path: yes   Overlay Files: no
Extracting ripgrep-14.1.1-x86_64-pc-windows-msvc\complete\rg.bash
Irrecoverable Error:  unknown exception.
```

Did I download the right file?
I've never encountered this problem before


### What are the steps to reproduce the behavior?

NA

### What is the actual behavior?

NA

### What is the expected behavior?

Should just unzip

---

_Comment by @BurntSushi on 2025-07-06 19:45_

Sorry, but I can't help with this. I'm not a Windows user and this looks like a Windows user error. I would suggest seeking help in a Windows specific forum.

---

_Closed by @BurntSushi on 2025-07-06 19:45_

---

_Label `invalid` added by @BurntSushi on 2025-07-06 19:45_

---

_Comment by @ltrzesniewski on 2025-07-06 20:25_

I tested it just in case, everything's OK:

<img width="909" height="568" alt="Image" src="https://github.com/user-attachments/assets/d0832a28-70b3-4c89-ab58-afcebcf74556" />

I'd say either you have some old zip software, or your antivirus gets in the way.

---
