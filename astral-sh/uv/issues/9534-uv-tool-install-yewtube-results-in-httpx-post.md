---
number: 9534
title: uv tool install yewtube results in httpx post failure
type: issue
state: closed
author: feoh
labels:
  - question
assignees: []
created_at: 2024-11-29T22:52:50Z
updated_at: 2025-01-29T15:37:27Z
url: https://github.com/astral-sh/uv/issues/9534
synced_at: 2026-01-10T01:24:42Z
---

# uv tool install yewtube results in httpx post failure

---

_Issue opened by @feoh on 2024-11-29 22:52_

OS: MacOS Sonoma 14.6.1
UV version:
```
uv 0.5.5 (95cd8b8b3 2024-11-27)
﻿﻿﻿uv-tool 0.5.5 (95cd8b8b3 2024-11-27)
```

running `pipx install yewtube` yields a 'yt' script that lets you search for videos to play, but when you search with the uv tool install version, you get:

```

Error fetching data. Possible network issue.
post() got an unexpected keyword argument 'proxies'
```

Running with --debug yields the traceback:

```
-----------------------------------------------------------------------
~ » yt --debug                                        cpatti@rocinante
yewtube version    : 2.12.0
yt_dlp version     : 2024.11.18
Python version     : 3.12.7 (main, Oct  1 2024, 02:05:46) [Clang 15.0.0 (clang-1500.3.9.4)]
Processor          : arm
Machine type       : arm64
Architecture       : 64bit,
Platform           : macOS-14.6.1-arm64-arm-64bit
sys.stdout.enc     : utf-8
default enc        : utf-8
Config dir         : /Users/cpatti/.config/mps-youtube
dbus               : None
glib               : False
env:TERM           : screen-256color
env:SHELL          : /bin/zsh
env:LANG           : en_US.UTF-8
could not load MPRIS interface. missing libraries.
--




Enter /search-term to search or [h]elp
> /VISION
Traceback (most recent call last):
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/mps_youtube/main.py", line 73, in matchfunction
    func(*matches)
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/mps_youtube/commands/search.py", line 310, in search
    wdata = pafy.search_videos(term, int(config.PAGES.get))
            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/mps_youtube/pafy.py", line 73, in search_videos
    videosSearch = VideosSearch(query, limit=50)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/youtubesearchpython/search.py", line 148, in __init__
    self.sync_create()
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/youtubesearchpython/core/search.py", line 29, in sync_create
    self._makeRequest()
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/youtubesearchpython/core/search.py", line 51, in _makeRequest
    request = self.syncPostRequest()
              ^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/youtubesearchpython/core/requests.py", line 20, in syncPostRequest
    return httpx.post(
           ^^^^^^^^^^^
TypeError: post() got an unexpected keyword argument 'proxies'
--

Traceback (most recent call last):
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/mps_youtube/main.py", line 73, in matchfunction
    func(*matches)
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/mps_youtube/commands/search.py", line 310, in search
    wdata = pafy.search_videos(term, int(config.PAGES.get))
            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/mps_youtube/pafy.py", line 73, in search_videos
    videosSearch = VideosSearch(query, limit=50)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/youtubesearchpython/search.py", line 148, in __init__
    self.sync_create()
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/youtubesearchpython/core/search.py", line 29, in sync_create
    self._makeRequest()
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/youtubesearchpython/core/search.py", line 51, in _makeRequest
    request = self.syncPostRequest()
              ^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/cpatti/.local/share/uv/tools/yewtube/lib/python3.12/site-packages/youtubesearchpython/core/requests.py", line 20, in syncPostRequest
    return httpx.post(
           ^^^^^^^^^^^
TypeError: post() got an unexpected keyword argument 'proxies'

Error fetching data. Possible network issue.
post() got an unexpected keyword argument 'proxies'
>
```

Thank you for such an amazing project!

---

_Comment by @samypr100 on 2024-11-30 01:08_

Based on what I'm seeing, I think the issue is with `httpx==0.28.x` which was released on Nov 28th. See [announcement](https://github.com/encode/httpx/releases/tag/0.28.0) which states `The deprecated proxies argument has now been removed.
`. Its likely the case that your pipx install is still using an older version of httpx 😅 

---

_Comment by @samypr100 on 2024-11-30 01:12_

This generally would require a fix in `yewtube`, but in the mean time you can resolve your issue by using `uv tool install --with httpx==0.27.2 yewtube`

---

_Label `question` added by @samypr100 on 2024-11-30 01:13_

---

_Comment by @feoh on 2024-11-30 16:46_

Thanks so much for the quick and helpful response!

You're right. I'll use the work-around for now and make a PR on their end.

---

_Closed by @feoh on 2024-11-30 16:46_

---

_Comment by @feoh on 2024-11-30 16:54_

One more detail JIC anyone else sees this issue. yewtube also uses pip as a standalone tool, so in order to get this to a working state I had to run:

```
uv tool install --with httpx==0.27.2,pip yewtube
```


---

_Comment by @worldpoop on 2024-12-01 13:06_

I'm getting the same error on Mac with homebrew.  :/

---

_Comment by @xuwv38 on 2025-01-19 20:09_

can you please explain how to solve this problem because I too have it and I don't understand the httpx and uv-tool you talk about, sorry but I'd like to thank you for your great work

---

_Comment by @feoh on 2025-01-19 21:09_

> can you please explain how to solve this problem because I too have it and I don't understand the httpx and uv-tool you talk about, sorry but I'd like to thank you for your great work

You literally type what I pasted above. That will fix this problem.

---

_Comment by @xuwv38 on 2025-01-20 11:11_

do I paste this interminal after pipx please because this is what I get
pipx: error: argument command: invalid choice: 'uv' (choose from 'install', 'inject', 'upgrade', 'upgrade-all', 'uninstall', 'uninstall-all', 'reinstall', 'reinstall-all', 'list', 'run', 'runpip', 'ensurepath', 'environment', 'completions')
sorry for the disturbance

---

_Comment by @feoh on 2025-01-29 15:37_

> do I paste this interminal after pipx please because this is what I get pipx: error: argument command: invalid choice: 'uv' (choose from 'install', 'inject', 'upgrade', 'upgrade-all', 'uninstall', 'uninstall-all', 'reinstall', 'reinstall-all', 'list', 'run', 'runpip', 'ensurepath', 'environment', 'completions') sorry for the disturbance

No. pipx is a different way to accomplish the same thing. You don't want to cross the streams :)

Literally type or paste the string:

```
uv tool install --with httpx==0.27.2,pip yewtube
```

into your shell. No pipx. Nothing else. Just that.

---

_Referenced in [mps-youtube/yewtube#1307](../../mps-youtube/yewtube/issues/1307.md) on 2025-03-11 13:48_

---
