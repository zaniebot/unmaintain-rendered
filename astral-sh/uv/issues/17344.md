```yaml
number: 17344
title: "The latest version 0.9.22 doesn't work on Windows"
type: issue
state: closed
author: jetunc
labels:
  - external
assignees: []
created_at: 2026-01-07T09:19:56Z
updated_at: 2026-01-08T14:36:55Z
url: https://github.com/astral-sh/uv/issues/17344
synced_at: 2026-01-10T03:11:36Z
```

# The latest version 0.9.22 doesn't work on Windows

---

_Issue opened by @jetunc on 2026-01-07 09:19_

### Summary

I got this error:

```
info: Checking for updates...
error: The installation failed. Output from the installer:
Downloading uv 0.9.22 (x86_64-pc-windows-msvc)
Installing to C:\Users\<user>\.cargo\bin
  uv.exe

We encountered an error trying to perform the installation; please review the error messages below.

Operation did not complete successfully because the file contains a virus or potentially unwanted software.
```

I also tried to install it on another Windows machine and encountered the same issue, so I had to downgrade to version 0.9.21.

### Platform

Windows 11 64 bit

### Version

uv 0.9.22

### Python version

Python 3.11

---

_Label `bug` added by @jetunc on 2026-01-07 09:19_

---

_Label `bug` removed by @konstin on 2026-01-07 09:20_

---

_Label `external` added by @konstin on 2026-01-07 09:20_

---

_Comment by @konstin on 2026-01-07 09:22_

This is a false positive by the antivirus software. The uv binary is fine, the AV software is detection is wrong in blocking the installation and showing that message.

---

_Comment by @EliteTK on 2026-01-07 11:25_

Seems specifically isolated to uvw.exe and uvx.exe?

* [uv-x86_64-pc-windows-msvc.zip](https://www.virustotal.com/gui/file/93a0a244f26eec208d8ea8077e3de743d9687ad76c190342722f1f57fa629457)
* [uv.exe](https://www.virustotal.com/gui/file/4995122c0fe34e8c5542fb59e9b095f70e4a8d1edf591fa0ac2caadd6843c4bb)
* [uvw.exe](https://www.virustotal.com/gui/file/15d15558a8c4ae547b58be4e1aa4e13391878edc7e502ace1bbd44c3bb26e97a)
* [uvx.exe](https://www.virustotal.com/gui/file/7ce068699d691b0b818a390a4c3e3294c4a0e3526a7487d40e635f5d44573a2a)

Also it looks like uvw.exe and uvx.exe have been getting flagged by a couple of others even earlier.

It might be worth submitting the files for malware analysis via https://www.microsoft.com/en-us/wdsi/filesubmission as windows defender complaining about uv is likely to affect almost all windows users of uv...

---

_Comment by @tanderberg on 2026-01-07 18:33_

Windows Security quarantined uvw.exe and uvx.exe as soon as I copied them from the release zip. Both are classified as "Severe" threats, with Details stating that "This program is dangerous and executes commands from an attacker."

uvw.exe is reported to contain Trojan:Win64/Malgent!MSR
For uvx.exe it's Trojan:Win32/Wacatac.C!ml

Updating with uv pip seems to work without issue, but this spooked me enough to launch a full virus scan and switch to working on another machine while it runs.

---

_Comment by @zanieb on 2026-01-07 18:54_

`uvx` and `uvw` both just invoke `uv`. This can be falsely detected as a malicious pattern.

You can see the source code, there's nothing malicious here

- https://github.com/astral-sh/uv/blob/main/crates/uv/src/bin/uvx.rs
- https://github.com/astral-sh/uv/blob/main/crates/uv/src/bin/uvw.rs

---

_Comment by @tanderberg on 2026-01-07 20:59_

Finding a virus in the source would be really bad; it would open the possibility that a developer's machine is infected. But it's not the first thing I would look for. There is a chain from repo to release. The source is compiled and zipped up somewhere, presumably on a dedicated server. If that server is compromised, anything built on it could be too. Without knowing the status of the whole chain, I can hope that this is a false positive, but I can't know so.

---

_Comment by @woodruffw on 2026-01-07 21:05_

> Without knowing the status of the whole chain, I can hope that this is a false positive, but I can't know so.

The build and release process is part of uv's public CI; you can review directly within uv's own source tree.

If you're concerned about the integrity of the assets you've received, we'd recommend (1) verifying their artifact attestations, and (2) cross-checking their digests against the digests in the official release. Information for both of those are in the release notes, e.g.:

https://github.com/astral-sh/uv/releases/tag/0.9.22

---

_Comment by @tanderberg on 2026-01-07 23:54_

Verifying attestations and cross-checking digests can tell me that I do indeed have the official release. I don't think it can tell me that the official release was compiled on a virus-free machine.

Does the release workflow include a Windows installation test? I had a quick (maybe too quick) look and didn't see one. Having one should have caught the problem, whether it's a false positive or not, but the CI widget says "passing".

---

_Comment by @woodruffw on 2026-01-08 00:04_

> I don't think it can tell me that the official release was compiled on a virus-free machine.

Unfortunately, I don't think anything can tell you this with perfect confidence.


> Does the release workflow include a Windows installation test? I had a quick (maybe too quick) look and didn't see one. Having one should have caught the problem, whether it's a false positive or not, but the CI widget says "passing".

Yeah, we test the Windows binaries we build like with other release binaries. However, I'm pretty sure this won't ever catch false positives, since GitHub intentionally disables Windows Defender on their Windows runners:

https://github.com/actions/runner-images/blob/a3ef6b2b8f38411b1f9938d5da919c2166fbab8b/images/windows/scripts/build/Configure-WindowsDefender.ps1

(And I think their reasons for disabling it are sound: a CI/CD runner executes all kinds of arbitrary network-sourced code by design, so running Defender would cause a lot of false positive noise and frustration for users.)


---

_Comment by @techvslife on 2026-01-08 00:18_

fwiw I updated to 0.9.22 via winget. I scanned the three new uv executables with windows defender on win11, using the latest definitions. defender gave me an all clear. (edit: Maybe post a hash and see if you really have the same uv files? or run Defender again after a new update of definitions.)

---

_Comment by @zanieb on 2026-01-08 00:32_

I'm not sure what you want us to do here. We are very serious about ensuring our build and release pipeline is secure. These false positives are quite common and honestly beyond our control. At this point, we need more evidence than heuristic-based virus alarms to take a report of this kind seriously.

> fwiw I updated to 0.9.22 via winget. I scanned the three new uv executables with windows defender on win11, using the latest definitions. defender gave me an all clear. 

It's very common for this to just be resolved after they do additional analysis or the reputation of the file improves.

---

_Comment by @tanderberg on 2026-01-08 13:13_

> defender gave me an all clear

I can confirm. With Defender update KB2267602 all three files pass the scan. (Last night the full scan I ran also quarantined the files installed with pip, which had not triggered an immediate reaction; Defender is evidently more relaxed about pip than about files copied from a downloaded zip.)

> I'm not sure what you want us to do here.

If you were an average small shop I would have suggested telling Microsoft that they probably have a false positive. You are now so deservedly big that they noticed and fixed it within a couple of days. For the same reason (your actions now affect lots of users), it would have been better for everybody involved if the issue had been resolved before release. Checking that release candidates work on a real machine with Defender enabled does not seem like an unreasonable extra effort.

---

_Comment by @woodruffw on 2026-01-08 14:25_

> Checking that release candidates work on a real machine with Defender enabled does not seem like an unreasonable extra effort.

I don't think there's a sound way for us to do this, since Defender is a black box that can change its detections on the fly. I'm pretty sure Microsoft also uses a behavioral model around detections, so ironically "smoke testing" with their AV can actually trigger it beyond what it would otherwise detect.

(And this doesn't generalize across other AV software/vendors.)

I think the long term solution here is that we'll need to provide signatures on our releases that Windows is aware of (Windows doesn't know how to check or trust GitHub annotations, another small irony given their parent company). That's something we're looking into.

---

_Closed by @zanieb on 2026-01-08 14:36_

---
