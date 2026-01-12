```yaml
number: 11723
title: "Hashes in `RECORD` don't match the actual file hashes"
type: issue
state: closed
author: chrisrodrigue
labels:
  - bug
assignees: []
created_at: 2025-02-23T14:40:59Z
updated_at: 2025-02-23T23:53:46Z
url: https://github.com/astral-sh/uv/issues/11723
synced_at: 2026-01-12T16:00:44Z
```

# Hashes in `RECORD` don't match the actual file hashes

---

_@chrisrodrigue_

### Summary

The SHA-256 hashes in `.venv/Lib/site-packages/<pkg>-<ver>.dist-info/RECORD` do not match the SHA-256 hashes of files in `.venv/Lib/site-packages/<pkg>`.

I verified this on Windows by running `certutil -hashfile <path/to/file/in/venv> sha256` and checking it against the hash for that file shown in the `RECORD` file. 

I'm not sure why there is a discrepancy. I quickly tested out changing line endings from `\n` to `\r\n` in a module, but that did not seem to bring the hash into alignment.

Some background info from [PEP 427](https://peps.python.org/pep-0427/)
>A wheel installer is not required to understand digital signatures but MUST verify the hashes in RECORD against the extracted file contents. When the installer checks file hashes against RECORD, a separate signature checker only needs to establish that RECORD matches the signature.

### Platform

Windows 11 x86_64

### Version

uv 0.6.2

### Python version

Python 3.13.2

---

_Label `bug` added by @chrisrodrigue on 2025-02-23 14:40_

---

_Comment by @chrisrodrigue on 2025-02-23 14:43_

I also used `hashlib` in the Python standard library, which yielded the same SHA-256 hash as `certutil`.

---

_Renamed from "Hashes in `RECORD` don't match hashes in `.venv/Lib/site-packages/<pkg>`" to "Hashes in `RECORD` don't match the actual file hashes" by @chrisrodrigue on 2025-02-23 15:30_

---

_Comment by @konstin on 2025-02-23 15:47_

Can you share a reproducible example? Does the same occur with pip?

We found that pip doesn't check the hash values and occasionally, there are packages with invalid hashes in the RECORD (such as some versions of torch). To ensure that the correct distribution gets installed, we instead use hashes on the entire source dist or wheel file. Those get stored in requirements.txt with `--hash` or in `uv.lock` (`hash = "sha256:`). Those external hashes provide a stronger security than the internal `RECORD` hashes. (The original idea of PEP 427 was that there would be signature files next to the RECORD that verify the RECORD, but those are not used, removing the use case for verifying the RECORD)

---

_Comment by @chrisrodrigue on 2025-02-23 16:28_

>Does the same occur with pip?

Yeah, I just confirmed that the same thing happens with pip.

>Can you share a reproducible example? 

Yeah sure thing 👍

1. Run `uv add pyserial` in a project

2. Go to `.venv\Lib\site-packages\pyserial-3.5.dist-info\RECORD` and note the checksum of the last file 

```txt
...
serial/win32.py,sha256=lk6rod9mHkqzgchmaqB3ygiTkmAWTNQ00IJ985ZjvTI,11138
```

3. Generate the SHA256 of the file 

```console
PS C:\Users\user\dev\albatross> certutil -hashfile .venv\Lib\site-packages\serial\win32.py sha256   
SHA256 hash of .venv\Lib\site-packages\serial\win32.py:
964eaba1df661e4ab381c8666aa077ca08939260164cd434d0827df39663bd32
CertUtil: -hashfile command completed successfully.
```

I completely agree with you about the stronger security with the checksums of the wheels. Unfortunately, the wheels no longer exist in the context of a virtual environment, so if all you have is a virtual environment, there doesn't seem to be any way to guarantee the integrity of the files in the virtual environment using the data from RECORD, which seems to have incorrect hashes. 

uv might be able to fix this by adding its own file to `*.dist-info` or `site-packages` with the correct file checksums (with the same file listing as RECORD, but maybe in TOML format)... but calculating the correct checksums for files in the venv might affect the performance critical path.

---

_Comment by @chrisrodrigue on 2025-02-23 17:52_

@konstin 

Figured it out. The SHA-256 in `RECORD` is base64 encoded 😄

---

_Comment by @chrisrodrigue on 2025-02-23 18:06_

Quick sanity check

```bash
$ python -c "import base64; print(base64.b64encode(bytes.fromhex('964eaba1df661e4ab381c8666aa077ca08939260164cd434d0827df39663bd32')).decode('utf-8'))"
lk6rod9mHkqzgchmaqB3ygiTkmAWTNQ00IJ985ZjvTI=
```

https://www.programiz.com/online-compiler/1ds1srnDChjwX

Looks like the final `=` was omitted in RECORD.


---

_Closed by @charliermarsh on 2025-02-23 23:53_

---
