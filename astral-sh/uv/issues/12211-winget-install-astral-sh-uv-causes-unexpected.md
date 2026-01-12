```yaml
number: 12211
title: "`winget install astral-sh.uv` causes unexpected error"
type: issue
state: open
author: kk-ntt
labels:
  - bug
  - external
assignees: []
created_at: 2025-03-17T02:15:38Z
updated_at: 2025-03-17T16:55:38Z
url: https://github.com/astral-sh/uv/issues/12211
synced_at: 2026-01-12T16:00:58Z
```

# `winget install astral-sh.uv` causes unexpected error

---

_@kk-ntt_

### Summary

> コマンドの実行中に予期しないエラーが発生しました:
> Unicode �����̃}�b�s���O���^�[�Q�b�g�̃}���`�o�C�g �R�[�h �y�[�W�ɂ���܂���B

expected behavior: install normally

### Platform

Windows 11 x86_64 (Japanese)

### Version

uv 0.6.6

### Python version

Python 3.13.2

---

_Label `bug` added by @kk-ntt on 2025-03-17 02:15_

---

_Comment by @konstin on 2025-03-17 10:18_

CC @Gankra 

---

_Comment by @Gankra on 2025-03-17 14:12_

"An unexpected error occurred while executing the command" followed by mojibake sure sounds like a unicode handling bug, but it would have to be in winget or the winget package definition -- literally none of our code is invoked here. I wonder if something is wrong with the generated locale files (https://github.com/microsoft/winget-pkgs/blob/master/manifests/a/astral-sh/uv/0.6.3/astral-sh.uv.locale.en-US.yaml)

---

_Label `external` added by @charliermarsh on 2025-03-17 14:13_

---

_Comment by @charliermarsh on 2025-03-17 14:13_

If it's entirely outside of our code, then it may make sense just to tag the winget folks and close here.

---

_Comment by @FishAlchemist on 2025-03-17 16:48_

Actually, winget issues should be opened on winget-pkgs.
https://github.com/microsoft/winget-pkgs/issues/new/choose

After you open the issue, just mention the maintainer of that version
@SpecterShell @spectopo (Hello, I don't know which one to mention, either.)

Edit:
Well, I'm not able to recreate that problem on my computer, and it's not running Japanese

---
