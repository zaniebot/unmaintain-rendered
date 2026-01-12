```yaml
number: 9144
title: Windows security check issues from UV 0.5.1 to 0.5.2.
type: issue
state: closed
author: FishAlchemist
labels:
  - windows
assignees: []
created_at: 2024-11-15T09:01:34Z
updated_at: 2024-12-10T16:47:03Z
url: https://github.com/astral-sh/uv/issues/9144
synced_at: 2026-01-12T15:59:43Z
```

# Windows security check issues from UV 0.5.1 to 0.5.2.

---

_@FishAlchemist_

### winget-pkgs
* https://github.com/microsoft/winget-pkgs/pull/190127 (x64 Windows Only)
* https://github.com/microsoft/winget-pkgs/pull/191664
* https://github.com/microsoft/winget-pkgs/pull/192548
* https://github.com/microsoft/winget-pkgs/pull/192554 (Passed security check)
### PyPI
* https://github.com/astral-sh/uv/issues/9143
----------
### Scan by VirusTotal
* [Version 0.5.1](https://github.com/astral-sh/uv/releases/tag/0.5.1) (Releases)
    * x64 Windows
        * [uv0.5.1-x86_64-pc-windows-msvc.zip](https://www.virustotal.com/gui/file/3dcb47a9334d7527e402eba8ba5aae3a62c77cddc3ce400f57fe2a40a621000d)
        * [uv0.5.1-x86_64-pc-windows-msvc.exe](https://www.virustotal.com/gui/file/6c64b51af850a88a32d452f72ab96e79a7d52ae1a86a0feabeee38f178212b7a)
        * [uvx0.5.1-x86_64-pc-windows-msvc.exe](https://www.virustotal.com/gui/file/abaaf5403d235b717ef24846d98a5186b540708a7b2d53da6d8de9ebfe540002)
    * x86 Windows
        * [uv0.5.1-i686-pc-windows-msvc.zip](https://www.virustotal.com/gui/file/7b0d716352f36730b3bdd40e1785e5e0299a2fa84929537c69f0b7ad9a1040e7)
        * [uv.0.5.1-i686-pc-windows-msvc.exe](https://www.virustotal.com/gui/file/b03eafa446484046a202f42999dfb1ce6782f2922f2044ce3b7bb97d4f55cc6a)
        * [uvx0.5.1-i686-pc-windows-msvc.exe](https://www.virustotal.com/gui/file/b3d0d3972217062ecb345558c98e8f5c967cc334f08ccf55d051aa6ebfbdfa60)
----------
I think it's because of the use of  ``self-replace``(https://github.com/astral-sh/uv/pull/8914). This kind of self-updating behavior, if not digitally signed, can easily be mistaken for a virus.

Microsoft actually provides a channel to upload files for analysis.
https://www.microsoft.com/en-us/wdsi/filesubmission


---

_Comment by @zanieb on 2024-11-15 14:27_

@charliermarsh Is the self-replace behavior worth dealing with this?

---

_Comment by @charliermarsh on 2024-11-15 14:31_

What’s your opinion?

---

_Comment by @zanieb on 2024-11-15 14:37_

I'm not sure. The whole "remove uv with uv" objective feels a little surprising to me. We can see if they fix this false positive before the next release?

---

_Comment by @charliermarsh on 2024-11-15 16:53_

Yeah, it's a little surprising. I think it's even worse that it fails (probably not that controversial), but it's probably not worth what we're seeing here. Maybe we tear it out and just give a better error than before?

---

_Comment by @charliermarsh on 2024-11-15 19:16_

@mitsuhiko -- Any opinion here?

---

_Label `windows` added by @charliermarsh on 2024-11-16 03:05_

---

_Comment by @FishAlchemist on 2024-11-19 17:50_

Hi UV Team,
Given that both 0.5.1 and 0.5.2 have this issue and I've removed x86 Windows support for version 0.5.1 in my pr for winget-pkgs, do you think it's still worthwhile to merge the x64 Windows version of uv 0.5.1 into winget-pkgs?
> [!NOTE]  
>I've removed the support for the x86 architecture in version 0.5.1 of winget-pkgs. This is finally allow it to pass their pipeline checks ( x86 still doesn't pass the security checks). I'm not sure if they'll approve the merge given that the new version lacks x86 installation compared to the previous one.

* https://github.com/microsoft/winget-pkgs/pull/190127

Edit:
Oh, the members of winget-pkgs have agreed to merge.

---

_Comment by @FishAlchemist on 2024-11-21 01:27_

As of now, winget-pkgs offers x64 Windows for 0.5.1 and 0.5.2, while 0.5.3 is available for both x86 and x64 Windows platforms.

---

_Comment by @zanieb on 2024-12-10 16:24_

Is this still being flagged?

---

_Comment by @FishAlchemist on 2024-12-10 16:44_

@zanieb 
I think we can close this issue for now. Since the x64 versions of winget 0.5.1 and 0.5.2 and the subsequent version x86 and x64 versions of later versions are all working fine

---

_Closed by @zanieb on 2024-12-10 16:47_

---
