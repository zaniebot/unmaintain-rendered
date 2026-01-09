---
number: 2670
title: uv pip install torch takes forever from asia
type: issue
state: open
author: ArshanKhanifar
labels:
  - needs-mre
assignees: []
created_at: 2024-03-26T14:43:16Z
updated_at: 2024-03-26T16:24:30Z
url: https://github.com/astral-sh/uv/issues/2670
synced_at: 2026-01-07T13:12:17-06:00
---

# uv pip install torch takes forever from asia

---

_Issue opened by @ArshanKhanifar on 2024-03-26 14:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

My colleague is from Asia (Hong Kong) and `uv pip install` takes forever from his location.
* `python` = `3.11`
*  freshly installed `uv` 
* macbook M3 
![image](https://github.com/astral-sh/uv/assets/10492324/3879c922-2a7a-4418-813a-852fd18bf6ac)

---

_Label `needs-mre` added by @konstin on 2024-03-26 14:58_

---

_Comment by @zanieb on 2024-03-26 14:58_

Could you provide some more information like logs from the installation command (with the `-v` flag) and the internet speed available?

---

_Comment by @ArshanKhanifar on 2024-03-26 15:02_

Adding my colleague to this thread, he'll be able to provide more info.


---

_Comment by @changwally on 2024-03-26 16:17_


Hi @zanieb, 
My internet speed is 400 Mbs / dl. The install basically hangs for most packages. 
As an example, the following is the logs when I try to install torch: 
```
% uv pip install -v torch  
INFO Found a virtualenv through VIRTUAL_ENV at: /Users/xxxxxx/yyyyyyyyyy/.venv
DEBUG Cached interpreter info for Python 3.11.4, skipping probing: .venv/bin/python
DEBUG Using Python 3.11.4 environment at .venv/bin/python
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.11.4
DEBUG Adding direct dependency: torch*
DEBUG No cache entry for: https://pypi.org/simple/torch/
DEBUG No credentials found for: https://pypi.org/simple/torch/
DEBUG Searching for a compatible version of torch (*)
DEBUG Selecting: torch==2.2.1 (torch-2.2.1-cp311-none-macosx_11_0_arm64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/3e/b9/256ab23c859cbcd7d6fb7cb46417a07eac817881a0a68df8ea0c18f45221/torch-2.2.1-cp311-none-macosx_11_0_arm64.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/3e/b9/256ab23c859cbcd7d6fb7cb46417a07eac817881a0a68df8ea0c18f45221/torch-2.2.1-cp311-none-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency: filelock*
DEBUG Adding transitive dependency: typing-extensions>=4.8.0
DEBUG Adding transitive dependency: sympy*
DEBUG Adding transitive dependency: networkx*
DEBUG Adding transitive dependency: jinja2*
DEBUG Adding transitive dependency: fsspec*
DEBUG No cache entry for: https://pypi.org/simple/filelock/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/filelock/
DEBUG No credentials found for: https://pypi.org/simple/filelock/
DEBUG No cache entry for: https://pypi.org/simple/typing-extensions/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/typing-extensions/
DEBUG No credentials found for: https://pypi.org/simple/typing-extensions/
DEBUG No cache entry for: https://pypi.org/simple/sympy/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/sympy/
DEBUG No credentials found for: https://pypi.org/simple/sympy/
DEBUG No cache entry for: https://pypi.org/simple/jinja2/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/jinja2/
DEBUG No credentials found for: https://pypi.org/simple/jinja2/
DEBUG No cache entry for: https://pypi.org/simple/networkx/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/networkx/
DEBUG No credentials found for: https://pypi.org/simple/networkx/
DEBUG No cache entry for: https://pypi.org/simple/fsspec/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/fsspec/
DEBUG No credentials found for: https://pypi.org/simple/fsspec/
DEBUG Searching for a compatible version of filelock (*)
DEBUG Selecting: filelock==3.13.3 (filelock-3.13.3-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8b/69/acdf492db27dea7be5c63053230130e0574fd8a376de3555d5f8bbc3d3ad/filelock-3.13.3-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/8b/69/acdf492db27dea7be5c63053230130e0574fd8a376de3555d5f8bbc3d3ad/filelock-3.13.3-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/8b/69/acdf492db27dea7be5c63053230130e0574fd8a376de3555d5f8bbc3d3ad/filelock-3.13.3-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/30/6d/6de6be2d02603ab56e72997708809e8a5b0fbfee080735109b40a3564843/Jinja2-3.1.3-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/30/6d/6de6be2d02603ab56e72997708809e8a5b0fbfee080735109b40a3564843/Jinja2-3.1.3-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/30/6d/6de6be2d02603ab56e72997708809e8a5b0fbfee080735109b40a3564843/Jinja2-3.1.3-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d2/05/e6600db80270777c4a64238a98d442f0fd07cc8915be2a1c16da7f2b9e74/sympy-1.12-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/d2/05/e6600db80270777c4a64238a98d442f0fd07cc8915be2a1c16da7f2b9e74/sympy-1.12-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/d2/05/e6600db80270777c4a64238a98d442f0fd07cc8915be2a1c16da7f2b9e74/sympy-1.12-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d5/f0/8fbc882ca80cf077f1b246c0e3c3465f7f415439bdea6b899f6b19f61f70/networkx-3.2.1-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/d5/f0/8fbc882ca80cf077f1b246c0e3c3465f7f415439bdea6b899f6b19f61f70/networkx-3.2.1-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/d5/f0/8fbc882ca80cf077f1b246c0e3c3465f7f415439bdea6b899f6b19f61f70/networkx-3.2.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of typing-extensions (>=4.8.0)
DEBUG Selecting: typing-extensions==4.10.0 (typing_extensions-4.10.0-py3-none-any.whl)
DEBUG Searching for a compatible version of sympy (*)
DEBUG Selecting: sympy==1.12 (sympy-1.12-py3-none-any.whl)
DEBUG Adding transitive dependency: mpmath>=0.19
DEBUG Searching for a compatible version of networkx (*)
DEBUG Selecting: networkx==3.2.1 (networkx-3.2.1-py3-none-any.whl)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.3 (Jinja2-3.1.3-py3-none-any.whl)
DEBUG Adding transitive dependency: markupsafe>=2.0
DEBUG Searching for a compatible version of fsspec (*)
DEBUG Selecting: fsspec==2024.3.1 (fsspec-2024.3.1-py3-none-any.whl)
DEBUG No cache entry for: https://pypi.org/simple/mpmath/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/mpmath/
DEBUG No credentials found for: https://pypi.org/simple/mpmath/
DEBUG No cache entry for: https://pypi.org/simple/markupsafe/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/markupsafe/
DEBUG No credentials found for: https://pypi.org/simple/markupsafe/
DEBUG Searching for a compatible version of mpmath (>=0.19)
DEBUG Selecting: mpmath==1.3.0 (mpmath-1.3.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/11/e7/291e55127bb2ae67c64d66cef01432b5933859dfb7d6949daa721b89d0b3/MarkupSafe-2.1.5-cp311-cp311-macosx_10_9_universal2.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/11/e7/291e55127bb2ae67c64d66cef01432b5933859dfb7d6949daa721b89d0b3/MarkupSafe-2.1.5-cp311-cp311-macosx_10_9_universal2.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/11/e7/291e55127bb2ae67c64d66cef01432b5933859dfb7d6949daa721b89d0b3/MarkupSafe-2.1.5-cp311-cp311-macosx_10_9_universal2.whl.metadata
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==2.1.5 (MarkupSafe-2.1.5-cp311-cp311-macosx_10_9_universal2.whl)
Resolved 9 packages in 1.00s
DEBUG Identified uncached requirement: filelock ==3.13.3
DEBUG Identified uncached requirement: fsspec ==2024.3.1
DEBUG Identified uncached requirement: jinja2 ==3.1.3
DEBUG Identified uncached requirement: markupsafe ==2.1.5
DEBUG Identified uncached requirement: mpmath ==1.3.0
DEBUG Identified uncached requirement: networkx ==3.2.1
DEBUG Identified uncached requirement: sympy ==1.12
DEBUG Identified uncached requirement: torch ==2.2.1
DEBUG Identified uncached requirement: typing-extensions ==4.10.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/3e/b9/256ab23c859cbcd7d6fb7cb46417a07eac817881a0a68df8ea0c18f45221/torch-2.2.1-cp311-none-macosx_11_0_arm64.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/3e/b9/256ab23c859cbcd7d6fb7cb46417a07eac817881a0a68df8ea0c18f45221/torch-2.2.1-cp311-none-macosx_11_0_arm64.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/3e/b9/256ab23c859cbcd7d6fb7cb46417a07eac817881a0a68df8ea0c18f45221/torch-2.2.1-cp311-none-macosx_11_0_arm64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d2/05/e6600db80270777c4a64238a98d442f0fd07cc8915be2a1c16da7f2b9e74/sympy-1.12-py3-none-any.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/d2/05/e6600db80270777c4a64238a98d442f0fd07cc8915be2a1c16da7f2b9e74/sympy-1.12-py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/d2/05/e6600db80270777c4a64238a98d442f0fd07cc8915be2a1c16da7f2b9e74/sympy-1.12-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d5/f0/8fbc882ca80cf077f1b246c0e3c3465f7f415439bdea6b899f6b19f61f70/networkx-3.2.1-py3-none-any.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/d5/f0/8fbc882ca80cf077f1b246c0e3c3465f7f415439bdea6b899f6b19f61f70/networkx-3.2.1-py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/d5/f0/8fbc882ca80cf077f1b246c0e3c3465f7f415439bdea6b899f6b19f61f70/networkx-3.2.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/30/6d/6de6be2d02603ab56e72997708809e8a5b0fbfee080735109b40a3564843/Jinja2-3.1.3-py3-none-any.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/30/6d/6de6be2d02603ab56e72997708809e8a5b0fbfee080735109b40a3564843/Jinja2-3.1.3-py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/30/6d/6de6be2d02603ab56e72997708809e8a5b0fbfee080735109b40a3564843/Jinja2-3.1.3-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/11/e7/291e55127bb2ae67c64d66cef01432b5933859dfb7d6949daa721b89d0b3/MarkupSafe-2.1.5-cp311-cp311-macosx_10_9_universal2.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/11/e7/291e55127bb2ae67c64d66cef01432b5933859dfb7d6949daa721b89d0b3/MarkupSafe-2.1.5-cp311-cp311-macosx_10_9_universal2.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/11/e7/291e55127bb2ae67c64d66cef01432b5933859dfb7d6949daa721b89d0b3/MarkupSafe-2.1.5-cp311-cp311-macosx_10_9_universal2.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8b/69/acdf492db27dea7be5c63053230130e0574fd8a376de3555d5f8bbc3d3ad/filelock-3.13.3-py3-none-any.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/8b/69/acdf492db27dea7be5c63053230130e0574fd8a376de3555d5f8bbc3d3ad/filelock-3.13.3-py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/8b/69/acdf492db27dea7be5c63053230130e0574fd8a376de3555d5f8bbc3d3ad/filelock-3.13.3-py3-none-any.whl

```

---
