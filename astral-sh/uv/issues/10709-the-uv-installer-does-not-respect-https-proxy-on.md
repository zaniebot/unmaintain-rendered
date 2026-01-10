---
number: 10709
title: The uv installer does not respect HTTPS_PROXY on Windows
type: issue
state: closed
author: deffi
labels:
  - windows
  - releases
assignees: []
created_at: 2025-01-17T13:38:36Z
updated_at: 2025-09-17T06:02:16Z
url: https://github.com/astral-sh/uv/issues/10709
synced_at: 2026-01-10T01:24:57Z
---

# The uv installer does not respect HTTPS_PROXY on Windows

---

_Issue opened by @deffi on 2025-01-17 13:38_

## Summary

I'm on Windows 11 behind a corporate proxy that requires authentication. The proxy is configured via environment variable: `https_proxy=http://user:pass@proxy.company:8080` (confidential information redacted) and all required SSL certificates are installed.

`uv self update` fails:
```
C:\>uv self update 0.5.19
info: Checking for updates...
error: The installation failed. Output from the installer: Downloading uv 0.5.19 (x86_64-pc-windows-msvc)
Exception calling "DownloadFile" with "2" argument(s): "The remote server returned an error: (407) Proxy Authentication Required."
```

## Setup

I'm running the latest version of `uv` (0.5.20) and it can download stuff just fine; for example:
```
C:\>echo %https_proxy%
http://user:pass@proxy.company:8080

C:\>uv tool install cowsay
Resolved 1 package in 799ms
Prepared 1 package in 743ms
Installed 1 package in 10ms
 + cowsay==6.1
Installed 1 executable: cowsay.exe
```

My `uv.toml`:
```
native-tls=true
```


## Issue

When I try to update `uv` to a different version from what is currently installed, I get an error message:
```
C:\>echo %https_proxy%
http://user:pass@proxy.company:8080

C:\>uv --verbose self update 0.5.19
DEBUG uv 0.5.20 (1c17662b3 2025-01-15)
info: Checking for updates...
error: The installation failed. Output from the installer: Downloading uv 0.5.19 (x86_64-pc-windows-msvc)
Exception calling "DownloadFile" with "2" argument(s): "The remote server returned an error: (407) Proxy Authentication Required."
```


## Additional observations

What *does* work is the check which versions are available (or which version is the latest):
```
C:\>echo %https_proxy%
http://user:pass@proxy.company:8080

C:\>uv --verbose self update
DEBUG uv 0.5.20 (1c17662b3 2025-01-15)
info: Checking for updates...
success: You're on the latest version of uv (v0.5.20)

C:\>uv --verbose self update 0.0.0
DEBUG uv 0.5.20 (1c17662b3 2025-01-15)
info: Checking for updates...
error: The version 0.0.0 was not found for the app uv in workspace uv
```

I can confirm that proxy authentication is indeed read from `%https_proxy%` by removing the credentials and observing that it fails (but note that the error message is different):
```
C:\>set https_proxy=http://proxy.company:8080

C:\>uv self update
info: Checking for updates...
error: error sending request for url (https://api.github.com/repos/astral-sh/uv/releases)
  Caused by: client error (Connect)
  Caused by: proxy authentication required
```

On Linux (WSL *on the same machine*), `uv self update` works as expected with the same configuration:
```
$ echo $https_proxy
http://user:pass@proxy.company:8080/

$ uv self update 0.5.19
info: Checking for updates...
success: Upgraded uv from v0.5.20 to v0.5.19! https://github.com/astral-sh/uv/releases/tag/0.5.19
```

For good measure, I also have the environment variables `http_proxy`, `HTTP_PROXY`, and `HTTPS_PROXY` set to the same value as `https_proxy`.


## Versions

Windows 11, Version 23H2 (OS Build 22631.4602)
uv 0.5.20 (1c17662b3 2025-01-15)


---

_Comment by @zanieb on 2025-01-17 18:41_

Thanks for the report. The updater is maintained over in axodotdev — we'll need to see what they're doing for client configuration and proxying as I believe it is outside our control.

---

_Label `windows` added by @zanieb on 2025-01-17 18:41_

---

_Label `releases` added by @zanieb on 2025-01-17 18:41_

---

_Comment by @notatallshaw on 2025-01-17 21:49_

Looks like there's an experimental opt-in for reading system certificate store: https://github.com/axodotdev/axoupdater/pull/136

---

_Referenced in [astral-sh/uv#10724](../../astral-sh/uv/issues/10724.md) on 2025-01-17 22:56_

---

_Comment by @deffi on 2025-01-18 18:31_

Using the system certificate store is crucial if you're behind a corporate proxy, but I'm not sure whether this, by itself, would fix the issue at hand.

The error message is specifically about proxy authentication (to my understanding, this means passing a username and a password to the proxy). Could such a message be caused by a missing certificate?


---

_Comment by @deffi on 2025-01-20 16:20_

After some investigation, I conclude that this is the fault of neither uv proper nor axoupdater.

Here's what happens:
  * `uv self update` downloads and runs the installer script (`uv-installer.ps1`) for the requested version
  * The (Windows version of the) installer script invokes `Net.Webclient.downloadFile` to download uv proper
  * `Net.Webclient.downloadFile` is unaware of the proxy configuration in `$HTTPS_PROXY` and therefore does not use proxy authentication

This explains why...
  * ...the issue is not observed on Linux (where the installer script uses curl or wget, both of which are aware of `$HTTPS_PROXY`)
  * ...the message is different from the one that is shown when the credentials are removed from `$HTTPS_PROXY` (as described above)
  * ...the message is localized with the configured language of Windows (as omitted above)


---

_Comment by @deffi on 2025-01-20 16:23_

The question therefore becomes:
  * Can/should `uv-installer.ps1` be made aware of `$HTTPS_PROXY`?
  * Alternatively, can Net.Webclient be configured globally to use proxy authentication, analogous to how curl can be configured using `$HTTPS_PROXY`?

Since all other uv commands use `$HTTPS_PROXY` (or `$https_proxy` if the former does not exist), it seems plausible that this should also work for `uv self update`.

---

_Comment by @fakerms on 2025-09-05 08:27_

Any workaround for this issue?
Just cannot install uv on my PC.

---

_Comment by @konstin on 2025-09-05 09:36_

CC @Gankra 

---

_Comment by @zanieb on 2025-09-05 13:29_

I think the diagnosis that `Net.Webclient.downloadFile` is at fault is key. Here's a brief LLM of possible fixes https://claude.ai/share/e85165fd-1bb7-45cc-ab51-28749dd430ca

---

_Referenced in [astral-sh/uv#15696](../../astral-sh/uv/issues/15696.md) on 2025-09-05 13:29_

---

_Renamed from "Proxy authentication doesn't work for `uv self update` on Windows" to "The uv installer does not respect HTTPS_PROXY on Windows" by @zanieb on 2025-09-05 13:35_

---

_Referenced in [axodotdev/cargo-dist#2075](../../axodotdev/cargo-dist/pulls/2075.md) on 2025-09-05 13:35_

---

_Comment by @zanieb on 2025-09-06 11:57_

We're now trying to figure out what the solution is over in https://github.com/axodotdev/cargo-dist/pull/2075, if someone has any Windows proxy API expertise.

---

_Referenced in [axodotdev/cargo-dist#2078](../../axodotdev/cargo-dist/pulls/2078.md) on 2025-09-06 20:27_

---

_Comment by @mistydemeo on 2025-09-07 18:36_

I'll have the fix for this out in the next cargo-dist shortly.

---

_Comment by @mistydemeo on 2025-09-07 21:03_

Released in cargo-dist 0.30.0.

---

_Comment by @zanieb on 2025-09-08 12:48_

Thanks @mistydemeo and @zsol !

---

_Closed by @zanieb on 2025-09-08 12:48_

---

_Comment by @zanieb on 2025-09-08 12:49_

(we'll need to upgrade before this is fixed)

---

_Referenced in [astral-sh/uv#15807](../../astral-sh/uv/pulls/15807.md) on 2025-09-12 10:45_

---

_Comment by @fakerms on 2025-09-17 05:48_

Waiting #15798 

---

_Comment by @fakerms on 2025-09-17 06:02_

My current workaround is 
1. Download <https://astral.sh/uv/install.ps1> as `uv-installer.ps1`
2. Manually apply this PR: https://github.com/axodotdev/cargo-dist/pull/2078
3. Set HTTPS_PROXY in terminal
4. Run `powershell -ExecutionPolicy ByPass -c "iex ./uv-installer.ps1"`

---
