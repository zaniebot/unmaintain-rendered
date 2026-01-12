```yaml
number: 17135
title: create-python-mirror has many download failures
type: issue
state: open
author: keestux
labels:
  - bug
assignees: []
created_at: 2025-12-15T15:28:36Z
updated_at: 2025-12-17T15:23:16Z
url: https://github.com/astral-sh/uv/issues/17135
synced_at: 2026-01-12T16:02:44Z
```

# create-python-mirror has many download failures

---

_@keestux_

### Summary

`create-python-mirror.py` has a problem to download all requested files without error.

```sh
$ uv run --script ./scripts/create-python-mirror.py --name cpython --arch x86_64 --os linux --target /MIRRORS/misc-mirrors/python-standalone
...
Downloading:  59%|██████████████████████████▍                  | 881/1502 [12:09<05:56,  1.74file/s]2025-12-15 16:10:21,429 - ERROR - Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20220502/cpython-3.9.12%2B20220502-x86_64_v4-unknown-linux-gnu-install_only.tar.gz: 
Downloading: 100%|████████████████████████████████████████████| 1502/1502 [21:11<00:00,  1.18file/s]
Successfully downloaded: 1435 files.
Failed downloads:
- https://github.com/astral-sh/python-build-standalone/releases/download/20221106/cpython-3.10.8%2B20221106-x86_64-unknown-linux-gnu-install_only.tar.gz: 
- https://github.com/astral-sh/python-build-standalone/releases/download/20251209/cpython-3.14.2%2B20251209-x86_64_v3-unknown-linux-gnu-debug-full.tar.zst: 
...
```

If i run the command again it may (or may not) download the missing files.

### Platform

Linux 6.8.0-52-generic x86_64 GNU/Linux

### Version

uv 0.8.14

### Python version

Python 3.13.7

---

_Label `bug` added by @keestux on 2025-12-15 15:28_

---

_Comment by @keestux on 2025-12-15 15:52_

If I run the command again, it may succeed.

Besides, the script does not return an error (exit code non zero) if it failed some downloads.

---

_Comment by @konstin on 2025-12-16 09:04_

We should add retries on network errors to the script.

---

_Comment by @keestux on 2025-12-16 09:32_

@konstin Do you happen to know why I don't see the `str(e)` in the output? I just see the url
```txt
Downloading:   9%|████▏                                          | 13/144 [00:05<00:31,  4.21file/s]2025-12-15 07:08:33,864 - ERROR - Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20251202/cpython-3.14.1%2B20251202-x86_64-pc-windows-msvc-install_only_stripped.tar.gz: 
```

```python
    except Exception as e:
        error_msg = f"Failed to download {url}: {str(e)}"
        logger.error(error_msg)
        errors.append((url, str(e)))
        progress_bar.update(1)
        return False
```

---

_Comment by @konstin on 2025-12-16 10:13_

Not sure, maybe tqdm is removing something? Otherwise you could set a breakpoint in the line and inspect `e`.

---

_Comment by @keestux on 2025-12-17 14:52_

What I've seen so far is that the `Exception`'s were `ConnectTimeout` and `ReadTimeout`. The default seems to be 5 seconds [1].

Perhaps we can set the timeout to 30s, or something.
```patch
@@ -207,7 +207,7 @@ async def download_files(
     urls: Set[Tuple[str, Optional[str]]], target: Path, max_concurrent: int
 ):
     """Download files with a limit on concurrent downloads using httpx."""
-    async with httpx.AsyncClient(follow_redirects=True) as client:
+    async with httpx.AsyncClient(follow_redirects=True, timeout=30.0) as client:
         progress_bar = tqdm(total=len(urls), desc="Downloading", unit="file")
         sem = asyncio.Semaphore(max_concurrent)
         errors: List[Tuple[str, str]] = []  # To collect errors
```

[1] https://www.python-httpx.org/advanced/timeouts/

---

_Comment by @konstin on 2025-12-17 15:23_

A higher default timeout makes sense for large and parallel downloads, we can add that to the script.

---
