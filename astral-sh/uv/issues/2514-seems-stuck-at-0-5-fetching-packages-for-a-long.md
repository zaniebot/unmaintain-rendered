---
number: 2514
title: "Seems stuck at ░░░░░░░░░░░░░░░░░░░░ [0/5] Fetching packages...   for a long time"
type: issue
state: closed
author: dheerajck
labels:
  - duplicate
  - needs-mre
assignees: []
created_at: 2024-03-18T16:18:10Z
updated_at: 2024-04-02T18:16:46Z
url: https://github.com/astral-sh/uv/issues/2514
synced_at: 2026-01-10T01:23:18Z
---

# Seems stuck at ░░░░░░░░░░░░░░░░░░░░ [0/5] Fetching packages...   for a long time

---

_Issue opened by @dheerajck on 2024-03-18 16:18_

I am not sure if this is a bug or an ui/ux issue
```
uv pip install -r requirements.txt 
Resolved 426 packages in 164ms
░░░░░░░░░░░░░░░░░░░░ [0/5] Fetching packages...
```
its stuck here for a long time, this doesnt happen with `pip install -r requirements.txt `,
pip downloads and install packages way faster but uv doesnt  so this isn an internet speed issue



---

_Comment by @dheerajck on 2024-03-18 16:20_

It would be great if we can have a better progress bar, and if we can make downloads faster(I think this has to do with some uv internals which handles downloads)

---

_Comment by @zanieb on 2024-03-18 16:21_

Could you please share output with the `-v` flag as well as your requirements file?

---

_Comment by @zanieb on 2024-03-18 16:21_

What does a long time mean in concrete terms?

---

_Comment by @Mateleo on 2024-03-19 10:24_

Same for me, problem is when downloading Pytorch
![image](https://github.com/astral-sh/uv/assets/25607142/27246a6c-ac5c-419b-86f4-555df646f0ca)


---

_Comment by @dheerajck on 2024-03-20 18:13_

I have similar messages in the logs,

I am even getting timeout error sometimes
```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: nvidia-cudnn-cu12==8.9.2.26
  Caused by: Failed to extract archive
  Caused by: Failed to download distribution due to network timeout. Try increasing UV_HTTP_TIMEOUT (current value: 300s).
```

Can I know why do we have a TIMEOUT, does pip have something similar,

if progress bar can show something like this with the progress bar

░░░░░░░░░░░░░░░░░░░░  10%, 10MBPS

it will be helpful

---

_Label `needs-mre` added by @zanieb on 2024-03-20 18:17_

---

_Comment by @zanieb on 2024-03-20 18:18_

We're going to need more information to reproduce this. Please see issues marked as [great writeup](https://github.com/astral-sh/uv/issues?q=is%3Aissue+label%3A%22great+writeup%22+) for example.

---

_Comment by @kevinhu on 2024-03-27 22:05_

I was running into this error too, so I tried vanilla pip to see if that could help diagnose the error. It looks like several of the Nvidia packages simply take really long to download and are exceeding the HTTP timeout. I ran the command on a GCP VM that has 500mbps+ bandwidth, but I was capped at 1mbps on a few of the downloads, which ended up taking several minutes to complete.

If uv could show a per-package download speed or process indicator for slow downloads, that would be super helpful for avoiding confusion. Right now, it's hard to tell if the timeout is failing because the download speed is too slow or if there is an error finding the distribution in the first place.

---

_Comment by @charliermarsh on 2024-03-27 22:10_

Yeah the Nvidia packages are enormous, as is Torch (hundreds of megabytes). My best guess here is that we're running into issues by making too many parallel requests to the index, and thus being served any given request too slowly to complete under the timeout (or something like that). Can you share more on what you did to hit this? E.g., what was the `requirements.txt`? Were you using any extra indexes, or just PyPI?

---

_Comment by @kevinhu on 2024-03-27 23:20_

I'm only using pypi—I encountered the timeout when installing torch alone:

```sh
uv venv
source .venv/bin/activate
uv pip install torch
```

---

_Comment by @zanieb on 2024-03-28 02:50_

For reference, we have some issues related to this:

- #1209 
- #1921 

---

_Comment by @dheerajck on 2024-04-02 17:53_

```
uv --version
uv 0.1.27
```
```
time uv pip install nvidia-cudnn-cu12==8.9.2.26 -v
INFO Found a virtualenv through VIRTUAL_ENV at: ~/.uvvenv
DEBUG Probing interpreter info for: ~/.uvvenv/bin/python
DEBUG Found Python 3.11.6 for: ~/.uvvenv/bin/python
DEBUG Using Python 3.11.6 environment at .uvvenv/bin/python
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.11.6
DEBUG Adding direct dependency: nvidia-cudnn-cu12==8.9.2.26
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cudnn-cu12/
DEBUG No credentials found for: https://pypi.org/simple/nvidia-cudnn-cu12/
DEBUG Searching for a compatible version of nvidia-cudnn-cu12 (==8.9.2.26)
DEBUG Selecting: nvidia-cudnn-cu12==8.9.2.26 (nvidia_cudnn_cu12-8.9.2.26-py3-none-manylinux1_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ff/74/a2e2be7fb83aaedec84f391f082cf765dfb635e7caa9b49065f73e4835d8/nvidia_cudnn_cu12-8.9.2.26-py3-none-manylinux1_x86_64.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/ff/74/a2e2be7fb83aaedec84f391f082cf765dfb635e7caa9b49065f73e4835d8/nvidia_cudnn_cu12-8.9.2.26-py3-none-manylinux1_x86_64.whl.metadata
DEBUG Adding transitive dependency: nvidia-cublas-cu12*
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cublas-cu12/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/nvidia-cublas-cu12/
DEBUG No credentials found for: https://pypi.org/simple/nvidia-cublas-cu12/
DEBUG Searching for a compatible version of nvidia-cublas-cu12 (*)
DEBUG Selecting: nvidia-cublas-cu12==12.4.2.65 (nvidia_cublas_cu12-12.4.2.65-py3-none-manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f2/44/fdfd1a9f54167714d2c3dbc1bb1cb030340d6fb936a1dab9ef60999e3f73/nvidia_cublas_cu12-12.4.2.65-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/f2/44/fdfd1a9f54167714d2c3dbc1bb1cb030340d6fb936a1dab9ef60999e3f73/nvidia_cublas_cu12-12.4.2.65-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/f2/44/fdfd1a9f54167714d2c3dbc1bb1cb030340d6fb936a1dab9ef60999e3f73/nvidia_cublas_cu12-12.4.2.65-py3-none-manylinux2014_x86_64.whl.metadata
Resolved 2 packages in 860ms
DEBUG Identified uncached requirement: nvidia-cublas-cu12==12.4.2.65
DEBUG Identified uncached requirement: nvidia-cudnn-cu12==8.9.2.26
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ff/74/a2e2be7fb83aaedec84f391f082cf765dfb635e7caa9b49065f73e4835d8/nvidia_cudnn_cu12-8.9.2.26-py3-none-manylinux1_x86_64.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/ff/74/a2e2be7fb83aaedec84f391f082cf765dfb635e7caa9b49065f73e4835d8/nvidia_cudnn_cu12-8.9.2.26-py3-none-manylinux1_x86_64.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/ff/74/a2e2be7fb83aaedec84f391f082cf765dfb635e7caa9b49065f73e4835d8/nvidia_cudnn_cu12-8.9.2.26-py3-none-manylinux1_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f2/44/fdfd1a9f54167714d2c3dbc1bb1cb030340d6fb936a1dab9ef60999e3f73/nvidia_cublas_cu12-12.4.2.65-py3-none-manylinux2014_x86_64.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/f2/44/fdfd1a9f54167714d2c3dbc1bb1cb030340d6fb936a1dab9ef60999e3f73/nvidia_cublas_cu12-12.4.2.65-py3-none-manylinux2014_x86_64.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/f2/44/fdfd1a9f54167714d2c3dbc1bb1cb030340d6fb936a1dab9ef60999e3f73/nvidia_cublas_cu12-12.4.2.65-py3-none-manylinux2014_x86_64.whl
Downloaded 2 packages in 1m 04s
Installed 2 packages in 1ms
 + nvidia-cublas-cu12==12.4.2.65
 + nvidia-cudnn-cu12==8.9.2.26

________________________________________________________
Executed in   64.90 secs    fish           external
   usr time    6.83 secs  271.00 micros    6.83 secs
   sys time    4.25 secs   77.00 micros    4.25 secs

```

```
python -m pip --version
pip 23.2
```
```
time pip install nvidia-cudnn-cu12==8.9.2.26 -v
Using pip 23.2 from ~/.venv/lib/python3.11/site-packages/pip (python 3.11)
Collecting nvidia-cudnn-cu12==8.9.2.26
  Obtaining dependency information for nvidia-cudnn-cu12==8.9.2.26 from https://files.pythonhosted.org/packages/ff/74/a2e2be7fb83aaedec84f391f082cf765dfb635e7caa9b49065f73e4835d8/nvidia_cudnn_cu12-8.9.2.26-py3-none-manylinux1_x86_64.whl.metadata
  Downloading nvidia_cudnn_cu12-8.9.2.26-py3-none-manylinux1_x86_64.whl.metadata (1.6 kB)
Collecting nvidia-cublas-cu12 (from nvidia-cudnn-cu12==8.9.2.26)
  Obtaining dependency information for nvidia-cublas-cu12 from https://files.pythonhosted.org/packages/f2/44/fdfd1a9f54167714d2c3dbc1bb1cb030340d6fb936a1dab9ef60999e3f73/nvidia_cublas_cu12-12.4.2.65-py3-none-manylinux2014_x86_64.whl.metadata
  Downloading nvidia_cublas_cu12-12.4.2.65-py3-none-manylinux2014_x86_64.whl.metadata (1.5 kB)
Downloading nvidia_cudnn_cu12-8.9.2.26-py3-none-manylinux1_x86_64.whl (731.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 731.7/731.7 MB 4.7 MB/s eta 0:00:00
Downloading nvidia_cublas_cu12-12.4.2.65-py3-none-manylinux2014_x86_64.whl (363.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 363.0/363.0 MB 7.2 MB/s eta 0:00:00
Installing collected packages: nvidia-cublas-cu12, nvidia-cudnn-cu12
Successfully installed nvidia-cublas-cu12-12.4.2.65 nvidia-cudnn-cu12-8.9.2.26

________________________________________________________
Executed in   61.87 secs    fish           external
   usr time    9.64 secs  148.00 micros    9.64 secs
   sys time    7.22 secs  138.00 micros    7.22 secs

```

---

_Comment by @dheerajck on 2024-04-02 17:55_

Can you explain this log  
`DEBUG No credentials found for: https://pypi.org/simple/nvidia-cudnn-cu12/`

---

_Comment by @dheerajck on 2024-04-02 18:04_

Also as asked above, adding a proper progress bar and download speed
like this `10%, 10MBPS`
or like this `10MB/100MB, 10MBPS`
will be really helpful when downloading large packages

and I personally never want timeout just because download is slow, because that doesnt help, unless this somehow make download faster next time(this means download was slow because of a temp issue with uv download), timeout when connection is idle for x seconds makes sense to me

---

_Comment by @zanieb on 2024-04-02 18:16_

>  DEBUG No credentials found for: https://pypi.org/simple/nvidia-cudnn-cu12/

This means we did not find any credentials for that URL and consequently did not populate them. It doesn't matter here since that's a public index.

>  adding a proper progress bar and download speed

Yep this is tracked in #1209 

> I personally never want timeout just because download is slow

The timeout should not occur during download, just during connection problems. See #1921 for more discussion.

I'm going to close this in favor of #1209 which is more actionable.



---

_Closed by @zanieb on 2024-04-02 18:16_

---

_Label `duplicate` added by @zanieb on 2024-04-02 18:16_

---
