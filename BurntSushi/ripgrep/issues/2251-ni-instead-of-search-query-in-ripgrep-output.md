```yaml
number: 2251
title: "`ni` instead of search query in ripgrep output"
type: issue
state: closed
author: sigseg5
labels:
  - invalid
assignees: []
created_at: 2022-06-28T20:06:29Z
updated_at: 2022-06-28T21:58:58Z
url: https://github.com/BurntSushi/ripgrep/issues/2251
synced_at: 2026-01-12T16:13:24Z
```

# `ni` instead of search query in ripgrep output

---

_@sigseg5_

#### What version of ripgrep are you using?

ripgrep 13.0.0.
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


#### How did you install ripgrep?

via cargo install

#### What operating system are you using ripgrep on?

OS: Pop_OS 22.04 LTS

Kernel: Linux 5.17.5-76051705-generic #202204271406\~1653440576\~22.04\~6277a18 SMP PREEMPT Wed May 25 01 x86_64 x86_64 x86_64 GNU/Linux

Terminal: alacritty 0.10.1-rc1

#### Describe your bug.

During the performance measurements ripgrep noticed a strange feature when working with a large number of files in the project.
If I run ripgrep with the absurd query (`rg -rni "a"`) in the [pytorch](https://github.com/pytorch/pytorch) repository folder with all installed dependencies, I see something strange in the output.
The search works correctly, but the results show a `ni` instead of an `a` on each line.
For example the file `tools/vscode_settings.py` has this content, but ripgrep displays it incorrectly.

![ripgrep output](https://user-images.githubusercontent.com/36568961/176274840-734a2062-3f77-4e67-b2db-a85569ea18ef.png)
ripgrep output

![original file](https://user-images.githubusercontent.com/36568961/176274582-1103256e-f4d8-4bcc-b2ee-2c423d57398b.png)
original file



---

_Comment by @BurntSushi on 2022-06-28 20:59_

> The search works correctly, but the results show a `ni` instead of an `a` on each line.

Why do you expect otherwise? You're telling ripgrep to replace matches with `ni`.

I would encourage you to read the docs for the `-r/--replace` flag.

---

_Closed by @sigseg5 on 2022-06-28 21:18_

---

_Comment by @BurntSushi on 2022-06-28 21:58_

My guess is that you thought `-r` meant "recursive" search, which is what it means with standard grep tools. The difference with ripgrep is that it always recursive search, so there is no option to "enable" it.

---

_Label `invalid` added by @BurntSushi on 2022-06-28 21:58_

---
