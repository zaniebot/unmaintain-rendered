```yaml
number: 14936
title: How to use UV on a private company jupiter notebook server?
type: issue
state: open
author: frbelotto
labels:
  - question
assignees: []
created_at: 2025-07-28T12:21:54Z
updated_at: 2025-07-28T12:21:54Z
url: https://github.com/astral-sh/uv/issues/14936
synced_at: 2026-01-12T16:01:59Z
```

# How to use UV on a private company jupiter notebook server?

---

_@frbelotto_

### Question

Hello guys!
sorry if my question os dumb, but I tried many times and couldn't make it work. 

I work on a company where we can use Python (py or ipynb files) to create scripts for data exploration, ETLs or any other use. The company provide a isolated "virtual machine" for each project with many pre installed packages.

I need to use some others and would like to use UV as solution to keep track of what I´ve installed (differently from what was previews installed!)

The only way I can install UV is using pip, the virtual environment does not provide brew, scoop or wget. 

```
pip install uv
Defaulting to user installation because normal site-packages is not writeable
Installing collected packages: uv
Successfully installed uv-0.8.3
```

So on, I would like do add packages by using UV but :

1. If I use uv add, for example :
- The package is installed, 
```
nanny@jupyter-XXXXX:/projeto/XXXX/workdir$ uv init
Initialized project `workdir`
nanny@jupyter-XXXX:/projeto/XXXX/workdir$ uv add impyla
Using CPython 3.11.0 interpreter at: /usr/local/bin/python3.11
Creating virtual environment at: .venv
Resolved 7 packages in 1.27s
      Built thrift==0.16.0
      Built pure-sasl==0.6.2
Prepared 6 packages in 5.09s
░░░░░░░░░░░░░░░░░░░░ [0/6] Installing wheels...                                                                                                                                                                                      warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 6 packages in 736ms
 + bitarray==2.9.3
 + impyla==0.21.0
 + pure-sasl==0.6.2
 + six==1.17.0
 + thrift==0.16.0
 + thrift-sasl==0.4.3
```

But my jupiter does not recognize it
```
import impyla
---------------------------------------------------------------------------
ModuleNotFoundError                       Traceback (most recent call last)
Cell In[9], line 1
----> 1 import impyla

ModuleNotFoundError: No module named 'impyla'
```
I suppose this happens because uv automatically created a .venv (hidden!) to be used as python kernel, but my company Jupiter is automatically set to use it defaults kernel and I cant change

<img width="296" height="172" alt="Image" src="https://github.com/user-attachments/assets/cae864b1-d42b-45b6-bbf4-3b1c62f4e418" />

2. Trying to use uv the same way I would use on google colab as I saw [here](https://github.com/astral-sh/uv/issues/2469) does not work (and, would not keep controle of dependencies on pyproject.toml):
```
nanny@jupyter-XXXX:/projeto/XXXX/workdir$ uv pip install --system impyla
Using Python 3.11.0 environment at: /usr/local
Resolved 6 packages in 208ms
error: Failed to install: thrift_sasl-0.4.3-py2.py3-none-any.whl (thrift-sasl==0.4.3)
  Caused by: failed to create directory `/usr/local/lib/python3.11/site-packages/thrift_sasl-0.4.3.dist-info`: Permission denied (os error 13)
```

3. So I tried to understand how a read only folder could install libs when I use "only pip":
```
nanny@jupyter-XXXXX:/projeto/XXXX/workdir$ which uv
/projeto/libs/bin/uv
nanny@jupyter-XXXX:/projeto/XXXXa/workdir$ which pip
/projeto/libs/bin/pip
nanny@jupyter-XXXXX:/projeto/XXXX/workdir$ cd /projeto/libs/bin/
nanny@jupyter-XXXXX:/projeto/libs/bin$ ls
pip  pip3  pip3.11  uv  uvx
```
And using pip
```
nanny@jupyter-XXXX:/projeto/libs/bin$ pip install ibm_db
Defaulting to user installation because normal site-packages is not writeable
Installing collected packages: ibm_db
Successfully installed ibm_db-3.2.6
nanny@jupyter-XXXX:/projeto/libs/bin$ ls
pip  pip3  pip3.11  uv  uvx
```

Removing ibm_db that used default pip shows me where they are installed!
```
nanny@jupyter-XXXX:/projeto/libs/lib/python3.11/site-packages$ pip uninstall ibm_db
Found existing installation: ibm_db 3.2.6
Uninstalling ibm_db-3.2.6:
  Would remove:
    /projeto/libs/lib/python3.11/site-packages/certs/*
    /projeto/libs/lib/python3.11/site-packages/clidriver/*
    /projeto/libs/lib/python3.11/site-packages/ibm_db-3.2.6.dist-info/*
    /projeto/libs/lib/python3.11/site-packages/ibm_db.cpython-311-x86_64-linux-gnu.so
    /projeto/libs/lib/python3.11/site-packages/ibm_db.libs/libcom_err-2abe824b.so.2.1
    /projeto/libs/lib/python3.11/site-packages/ibm_db.libs/libgssapi_krb5-497db0c6.so.2.2
    /projeto/libs/lib/python3.11/site-packages/ibm_db.libs/libk5crypto-b1f99d5c.so.3.1
    /projeto/libs/lib/python3.11/site-packages/ibm_db.libs/libkeyutils-dfe70bd6.so.1.5
    /projeto/libs/lib/python3.11/site-packages/ibm_db.libs/libkrb5-fcafa220.so.3.3
    /projeto/libs/lib/python3.11/site-packages/ibm_db.libs/libkrb5support-d0bcff84.so.0.1
    /projeto/libs/lib/python3.11/site-packages/ibm_db.libs/liblzma-004595ca.so.5.2.2
    /projeto/libs/lib/python3.11/site-packages/ibm_db.libs/libpcre-9513aab5.so.1.2.0
    /projeto/libs/lib/python3.11/site-packages/ibm_db.libs/libselinux-0922c95c.so.1
    /projeto/libs/lib/python3.11/site-packages/ibm_db.libs/libxml2-3998bec4.so.2.9.1
    /projeto/libs/lib/python3.11/site-packages/ibm_db_ctx.py
    /projeto/libs/lib/python3.11/site-packages/ibm_db_dbi.py
    /projeto/libs/lib/python3.11/site-packages/ibm_db_tests/*
    /projeto/libs/lib/python3.11/site-packages/ibmdb_tests.py
    /projeto/libs/lib/python3.11/site-packages/opt/python/cp311-cp311/lib/python3.11/site-packages/CHANGES.md
    /projeto/libs/lib/python3.11/site-packages/opt/python/cp311-cp311/lib/python3.11/site-packages/LICENSE
    /projeto/libs/lib/python3.11/site-packages/opt/python/cp311-cp311/lib/python3.11/site-packages/README.md
    /projeto/libs/lib/python3.11/site-packages/opt/python/cp311-cp311/lib/python3.11/site-packages/config.py.sample
    /projeto/libs/lib/python3.11/site-packages/testfunctions.py
```

Really don´t know what else to do!
- I cant use .Venv
- I cant write on system folder
- Using uv pip can let me use "target" folder but does not track packages!


### Platform

Linux (dont know for sure!)

### Version

0.83

---

_Label `question` added by @frbelotto on 2025-07-28 12:21_

---
