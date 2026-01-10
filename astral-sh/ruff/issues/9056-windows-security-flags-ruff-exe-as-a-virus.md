```yaml
number: 9056
title: "Windows Security flags `ruff.exe` as a virus"
type: issue
state: closed
author: s-banach
labels: []
assignees: []
created_at: 2023-12-08T19:15:10Z
updated_at: 2023-12-14T16:54:08Z
url: https://github.com/astral-sh/ruff/issues/9056
synced_at: 2026-01-10T11:09:51Z
```

# Windows Security flags `ruff.exe` as a virus

---

_Issue opened by @s-banach on 2023-12-08 19:15_

I'm not sure what you can do about this, but my work computer just decided to flag ruff as a virus and delete it.
Don't know if "enterprise" windows has an exceptionally tight virus scanner or if consumer windows will start doing this to people as well.

---

_Comment by @zanieb on 2023-12-08 19:59_

Thanks for reporting but there's unfortunately not much we can do here.

Similar reports at https://github.com/astral-sh/ruff-vscode/issues/196 and https://github.com/astral-sh/ruff-lsp/issues/134

Could you provide more details on the virus report?

---

_Comment by @FishAlchemist on 2023-12-09 01:35_

## ruff-x86_64-pc-windows-msvc
* v0.0.272
https://www.virustotal.com/gui/file/172f00962b8b4279c857e9c4ccb6c7ea848b35f0f4176cbb9cf2b1194be82504
* v0.0.273 [1 security vendor and no sandboxes flagged this file as malicious]
https://www.virustotal.com/gui/file/e802c866f7797bb82a8f3438271b591a2921f603437c3718f84561b22a4ef630

What's new in 0.0.273? Things that might be identified as viruses?
https://github.com/astral-sh/ruff/compare/v0.0.272...v0.0.273
I uploaded it manually so I didn't scan every version.
### Record
ruff-0.0.260.exe 
https://www.virustotal.com/gui/file/fc45f6da96761d0cb459bd156b4c47b7916f8f0fb6edf0d1b3bd863ce2614ed3?nocache=1
ruff-0.0.270.exe
https://www.virustotal.com/gui/file/34a876c15763bb6b073f94b400ecd529cdf9cb6dffd97cb67f5917f299c5d438?nocache=1
ruff-0.0.272.exe
https://www.virustotal.com/gui/file/172f00962b8b4279c857e9c4ccb6c7ea848b35f0f4176cbb9cf2b1194be82504?nocache=1
ruff-0.0.273.exe [malicious]
https://www.virustotal.com/gui/file/e802c866f7797bb82a8f3438271b591a2921f603437c3718f84561b22a4ef630?nocache=1
ruff-0.0.275.exe [malicious]
https://www.virustotal.com/gui/file/43daea66d30979a2c87a34f64b5936b8ac098233f6fecd803f2dfb994b3d5bd5?nocache=1
ruff-0.0.280.exe [malicious]
https://www.virustotal.com/gui/file/fe32afdc2981c0fea669c3f96c440a5861538e6fed4ec532211f6d65cfbf760b?nocache=1

## RustPython [0fab6e606379cfda346c82fa2f3960d0449b40e9](https://github.com/RustPython/RustPython/tree/0fab6e606379cfda346c82fa2f3960d0449b40e9)
(1 security vendor and no sandboxes flagged this file as malicious)
https://www.virustotal.com/gui/file/b9fc8c8f4717b6fa34a75fb3a34e9327ae7a98d655de3de0d48341d7f30400a9?nocache=1

---

_Comment by @zanieb on 2023-12-09 03:33_

Thanks! Does this also apply to our most recent version (0.1.7)?

---

_Comment by @FishAlchemist on 2023-12-09 04:20_

> Thanks! Does this also apply to our most recent version (0.1.7)?

(1 security vendor and no sandboxes flagged this file as malicious)
https://www.virustotal.com/gui/file/dad6ff8ec821555b9793c3448f7e84c82c20330f5c089dadae7c2d26d4d2af84

---

_Comment by @Avasam on 2023-12-14 06:40_

Executables that read and modify code are likely to be flagged as potential malware. Especially if written in a lower-level language. As more users download the executable, and manually flag it as non-dangerous, then antivirus/antimalware understand it's a false-positive for that specific executable's signature. This is normal and has a chance to happen every new release, where a "new executable" is "rapidly spreading" (from an antivirus' PoV).

The best Ruff devs can do really is try to strike a deal with popular antivirus makers to send new releases to be excluded.

---

_Comment by @charliermarsh on 2023-12-14 16:54_

I'm gonna close this for now given that it's hard for us to take action on it unfortunately.

---

_Closed by @charliermarsh on 2023-12-14 16:54_

---
