```yaml
number: 10535
title: Will the cache be used if the package with the same version number is installed in different Python version virtual environments?
type: issue
state: closed
author: jarlor
labels:
  - question
  - cache
assignees: []
created_at: 2025-01-12T05:59:20Z
updated_at: 2025-01-13T05:23:13Z
url: https://github.com/astral-sh/uv/issues/10535
synced_at: 2026-01-12T16:00:15Z
```

# Will the cache be used if the package with the same version number is installed in different Python version virtual environments?

---

_@jarlor_

I initialized two uv projects. Project A uses python3.11 via `uv init -p 3.11`. Project B uses python 3.10 via `uv init -p 3.10`.

I installed transformers==4.48.0 in project A via `uv add transformers==4.48.0`. After the installation was successful, I could see that transformer==4.48.0 was already in the cache directory.

I continued to try to install transformers==4.48.0 in project B via `uv add transformers==4.48.0`. I found that it **re-downloaded** transformers==4.48.0 instead of using the cache.

I want to know if this is normal? Can I use any command line to control the use of cache instead of re-downloading when installing transformers==4.48.0 in B.

---

_Comment by @charliermarsh on 2025-01-12 16:07_

How did you conclude that it's re-downloading transformers==4.48.0?

---

_Comment by @charliermarsh on 2025-01-12 20:59_

So, if I do this in two different projects, the first shows:

```
Resolved 19 packages in 243ms
Prepared 9 packages in 594ms
Installed 17 packages in 73ms
 + certifi==2024.12.14
 + charset-normalizer==3.4.1
 + filelock==3.16.1
 + fsspec==2024.12.0
 + huggingface-hub==0.27.1
 + idna==3.10
 + numpy==2.2.1
 + packaging==24.2
 + pyyaml==6.0.2
 + regex==2024.11.6
 + requests==2.32.3
 + safetensors==0.5.2
 + tokenizers==0.21.0
 + tqdm==4.67.1
 + transformers==4.48.0
 + typing-extensions==4.12.2
 + urllib3==2.3.0
```

Then the second hows:

```
Resolved 19 packages in 74ms
Prepared 3 packages in 113ms
Installed 15 packages in 28ms
 + certifi==2024.12.14
 + charset-normalizer==3.4.1
 + filelock==3.16.1
 + fsspec==2024.12.0
 + huggingface-hub==0.27.1
 + idna==3.10
 + pyyaml==6.0.2
 + regex==2024.11.6
 + requests==2.32.3
 + safetensors==0.5.2
 + tokenizers==0.21.0
 + tqdm==4.67.1
 + transformers==4.48.0
 + typing-extensions==4.12.2
 + urllib3==2.3.0
```

The `Prepared 3 packages` indicates that only three new packages were downloaded -- the rest came from the cache. This makes sense, since _some_ of the dependencies are probably specific to the Python version, and so we have to download new builds for Python 3.11 when re-running `uv add` in the 3.11 project.

---

_Label `question` added by @charliermarsh on 2025-01-13 02:15_

---

_Label `cache` added by @charliermarsh on 2025-01-13 02:15_

---

_Comment by @jarlor on 2025-01-13 05:23_

Thank for u reply！
Actually i don't know `Prepared xx packages` means xx packages were downloaded. 
I remember that when I was installing in project B, the progress bar of the download package flashed by. Because the speed was so fast, I couldn't tell whether it was downloading additional new packages or using the cache.

---

_Closed by @jarlor on 2025-01-13 05:23_

---
