```yaml
number: 12852
title: How to have a full python environment?
type: issue
state: open
author: youkaichao
labels:
  - question
assignees: []
created_at: 2025-04-13T04:58:02Z
updated_at: 2025-08-12T21:16:27Z
url: https://github.com/astral-sh/uv/issues/12852
synced_at: 2026-01-12T16:01:14Z
```

# How to have a full python environment?

---

_@youkaichao_

### Question

I'm installing a new environment with `uv venv ~/uv_envs/test --python 3.12` , it uses my system's python as the interpreter.

However, i find that my system intepreter is not complete, it does not have `/usr/include/python3.12` , but `python -c "from sysconfig import get_paths; print(get_paths()['include'])"` still reports that directory.

Can I somehow tell `uv` to download a full python environment?

I can use `sudo apt install python3.12-dev` as a workaround, but then the environment is not reproducible, i.e. it depends on the initial python interpreter i installed.

can i make `uv` take the full control, downloading a full python regardless of my initial python environment?

### Platform

Ubuntu 20.04, arm64

### Version

uv 0.5.11

---

_Label `question` added by @youkaichao on 2025-04-13 04:58_

---

_Comment by @pjrulez on 2025-04-13 07:38_

EDIT & TLDR:
'uv self update' and then I could install the latest python 3.12 version.


I can't download python 3.12.10 with uv:
uv python install 3.12.10
error: No download found for request: cpython-3.12.10-linux-aarch64-gnu

Additionally, I'm using this image: 
FROM ghcr.io/astral-sh/uv:python3.12-bookworm-slim
but it was updated from 3.12.9 which worked with my project to 3.12.10 which now doesn't work during the build process because the image now comes with 3.12.10.

For a moment I thought this was relevant. Now, I'm not so sure.

---

_Comment by @charliermarsh on 2025-04-13 14:20_

@youkaichao -- You can try passing `--python-preference only-managed` -- that will force uv to use a uv-downloaded Python.

---

_Comment by @j178 on 2025-04-14 03:04_

`--python-preference` is now hidden from user, the new convenient equivalent is the `--managed-python` flag.

---

_Comment by @youkaichao on 2025-04-14 03:39_

@charliermarsh thanks, I'm using `uv venv uv_envs/test_0414 --python 3.12 --managed-python --seed` now, is `--managed-python ` similar to `--python-preference only-managed` ?

---

_Comment by @youkaichao on 2025-04-14 03:52_

well, it seems it still links to `~/.local/share/uv/python` .

The context here, is I'm in a cluster, which has a shared file system at `/sharefs` , and I want to put uv there, without storing anything locally to that node.

Somehow uv links `~/.local/share/uv/python` to `/sharefs/uv_envs/test_0414` , and this link is broken on another node.

---

_Comment by @charliermarsh on 2025-04-14 03:54_

Yeah, virtual environments on Linux and macOS will always link to a Python by default (unlike on Windows). The standard library `python -m venv` supports a `--copies` flag which lets you copy the distribution, I think? See #2103.

---

_Comment by @youkaichao on 2025-04-14 04:00_

is it possible to have a fresh new, standalone environment, without any linking?

---

_Comment by @youkaichao on 2025-04-14 04:04_

I try to add `--link-mode copy` to `uv venv` , but the include path is still not copied into this new environment.

ultimately, what i want is `sysconfig.get_paths()["include"]` should only contain the files in the uv env, so that i can place the full environment in the shared filesystem, and all nodes in the cluster can use it.

---

_Comment by @youkaichao on 2025-04-14 04:56_

after fighting for a while, i end up with this:

```bash
export ROOT_DIR=/sharedfs/somewhere
export UV_HOME=$ROOT_DIR/uv_home
export UV_INSTALL_DIR="$ROOT_DIR/uv"
export UV_CACHE_DIR="$ROOT_DIR/uv_cache"
export XDG_DATA_HOME=$ROOT_DIR
export PATH="$PATH:$UV_INSTALL_DIR"
curl -LsSf https://astral.sh/uv/install.sh | sh

uv venv $ROOT_DIR/uv_envs/test_0416 --python 3.12 --managed-python --link-mode copy --seed
```

and then it seems all files are in the `/sharedfs` now.

---

_Comment by @liquidcarbon on 2025-04-14 12:40_

As an alternative, for the last year I've been using Pixi for base python install, and wrapping uv in `pixi run uv`.
This way I know exactly which python is being used, despite ongoing changes in uv.

Here's a pet project that wraps this: https://github.com/liquidcarbon/puppy

---

_Comment by @hypervanse on 2025-06-04 01:47_

import sys
import os
import platform
import pkgutil

def main():
    print("="*40)
    print("🩺 DoctorMyPass - Local Python Diagnostic")
    print("="*40)

    # Platform and Python version
    print(f"Python Executable: {sys.executable}")
    print(f"Python Version: {sys.version}")
    print(f"Platform: {platform.system()} {platform.release()} ({platform.version()})")
    print(f"Machine: {platform.machine()}")
    print(f"Processor: {platform.processor()}")
    print(f"Architecture: {platform.architecture()}")

    # Show sys.path
    print("\n📂 sys.path (where Python looks for modules):")
    for p in sys.path:
        print(f" - {p}")

    # List available modules
    print("\n📚 Importable Modules (minimal check):")
    modules = sorted([module.name for module in pkgutil.iter_modules()])
    for mod in modules:
        print(f" - {mod}")

    # Write the output to a file
    try:
        with open("DoctorMyPass.txt", "w") as f:
            f.write("DoctorMyPass - Local Python Diagnostic\n")
            f.write("="*40 + "\n")
            f.write(f"Python Executable: {sys.executable}\n")
            f.write(f"Python Version: {sys.version}\n")
            f.write(f"Platform: {platform.system()} {platform.release()} ({platform.version()})\n")
            f.write(f"Machine: {platform.machine()}\n")
            f.write(f"Processor: {platform.processor()}\n")
            f.write(f"Architecture: {platform.architecture()}\n")
            f.write("\nsys.path:\n")
            for p in sys.path:
                f.write(f" - {p}\n")
            f.write("\nImportable Modules:\n")
            for mod in modules:
                f.write(f" - {mod}\n")
        print("\n📝 Diagnostic saved to DoctorMyPass.txt")
    except Exception as e:
        print(f"\n⚠️ Could not write log file: {e}")

    print("="*40)
    print("🩺 DoctorMyPass - Done")
    print("="*40)

if __name__ == "__main__":
    main()


---

_Comment by @hypervanse on 2025-06-04 01:50_

results below. In my case, it's ChatGPT messing around. the humans, not the model 

=== stdout ===

========================================
🩺 DoctorMyPass - Local Python Diagnostic
========================================
Python Executable: /usr/local/python-3.13.2/bin/python3
Python Version: 3.13.2 (main, Feb 23 2025, 08:23:48) [GCC 8.3.0]
Platform: Linux 6.1.0-31-cloud-amd64 (#1 SMP PREEMPT_DYNAMIC Debian 6.1.128-1 (2025-02-07))
Machine: x86_64
Processor: 
Architecture: ('64bit', 'ELF')

📂 sys.path (where Python looks for modules):
 - /box
 - /usr/local/python-3.13.2/lib/python313.zip
 - /usr/local/python-3.13.2/lib/python3.13
 - /usr/local/python-3.13.2/lib/python3.13/lib-dynload
 - /usr/local/python-3.13.2/lib/python3.13/site-packages

📚 Importable Modules (minimal check):
 - 5bae8a57b5ef85818b48__mypyc
 - PIL
 - __future__
 - __hello__
 - __phello__
 - _aix_support
 - _android_support
 - _apple_support
 - _asyncio
 - _bisect
 - _blake2
 - _bz2
 - _codecs_cn
 - _codecs_hk
 - _codecs_iso2022
 - _codecs_jp
 - _codecs_kr
 - _codecs_tw
 - _collections_abc
 - _colorize
 - _compat_pickle
 - _compression
 - _contextvars
 - _csv
 - _ctypes
 - _ctypes_test
 - _curses
 - _curses_panel
 - _datetime
 - _dbm
 - _decimal
 - _elementtree
 - _gdbm
 - _hashlib
 - _heapq
 - _interpchannels
 - _interpqueues
 - _interpreters
 - _ios_support
 - _json
 - _lsprof
 - _lzma
 - _markupbase
 - _md5
 - _multibytecodec
 - _multiprocessing
 - _opcode
 - _opcode_metadata
 - _osx_support
 - _pickle
 - _posixshmem
 - _posixsubprocess
 - _py_abc
 - _pydatetime
 - _pydecimal
 - _pyio
 - _pylong
 - _pyrepl
 - _pyrsistent_version
 - _pytest
 - _queue
 - _random
 - _sha1
 - _sha2
 - _sha3
 - _sitebuiltins
 - _socket
 - _sqlite3
 - _ssl
 - _statistics
 - _strptime
 - _struct
 - _sysconfigdata__linux_x86_64-linux-gnu
 - _testbuffer
 - _testcapi
 - _testclinic
 - _testclinic_limited
 - _testexternalinspection
 - _testimportmultiple
 - _testinternalcapi
 - _testlimitedcapi
 - _testmultiphase
 - _testsinglephase
 - _threading_local
 - _uuid
 - _weakrefset
 - _xxtestfuzz
 - _zoneinfo
 - abc
 - absl
 - annotated_types
 - antigravity
 - argparse
 - array
 - ast
 - asyncio
 - attr
 - attrs
 - base64
 - bdb
 - binascii
 - bisect
 - bz2
 - cProfile
 - calendar
 - certifi
 - charset_normalizer
 - cmath
 - cmd
 - code
 - codecs
 - codeop
 - collections
 - colorsys
 - compileall
 - concurrent
 - configparser
 - contextlib
 - contextvars
 - contourpy
 - copy
 - copyreg
 - csv
 - ctypes
 - curses
 - cycler
 - dataclasses
 - datetime
 - dateutil
 - dbm
 - decimal
 - difflib
 - dis
 - doctest
 - email
 - encodings
 - ensurepip
 - enum
 - fcntl
 - filecmp
 - fileinput
 - fnmatch
 - fontTools
 - fractions
 - frozenlist
 - ftplib
 - functools
 - genericpath
 - getopt
 - getpass
 - gettext
 - glob
 - graphlib
 - grp
 - gzip
 - h5py
 - hashlib
 - heapq
 - hmac
 - html
 - http
 - idlelib
 - idna
 - imaplib
 - importlib
 - iniconfig
 - inspect
 - io
 - ipaddress
 - isodate
 - jmespath
 - joblib
 - json
 - jsonschema
 - jsonschema_specifications
 - jwt
 - keras
 - keyword
 - kiwisolver
 - linecache
 - locale
 - logging
 - lzma
 - mailbox
 - markdown_it
 - markupsafe
 - math
 - matplotlib
 - mdurl
 - mimetypes
 - ml_dtypes
 - mlxtend
 - mmap
 - modulefinder
 - multidict
 - multiprocessing
 - namex
 - netrc
 - ntpath
 - nturl2path
 - numbers
 - numpy
 - oauthlib
 - opcode
 - operator
 - optparse
 - optree
 - os
 - packaging
 - pandas
 - pathlib
 - pdb
 - pickle
 - pickletools
 - pip
 - pkgutil
 - platform
 - plistlib
 - pluggy
 - ply
 - poplib
 - posixpath
 - pprint
 - profile
 - propcache
 - pstats
 - pty
 - pulp
 - py
 - py_compile
 - pyasn1
 - pyclbr
 - pydantic
 - pydantic_core
 - pydoc
 - pydoc_data
 - pyexpat
 - pygments
 - pylab
 - pyomo
 - pyparsing
 - pyrsistent
 - pytest
 - pytz
 - queue
 - quopri
 - random
 - re
 - readline
 - referencing
 - reprlib
 - requests
 - requests_oauthlib
 - resource
 - rich
 - rlcompleter
 - rpds
 - rsa
 - runpy
 - sched
 - scipy
 - script
 - secrets
 - select
 - selectors
 - shelve
 - shlex
 - shutil
 - signal
 - site
 - six
 - sklearn
 - smtplib
 - socket
 - socketserver
 - sqlite3
 - sre_compile
 - sre_constants
 - sre_parse
 - ssl
 - stat
 - statistics
 - string
 - stringprep
 - struct
 - subprocess
 - symtable
 - sysconfig
 - syslog
 - tabnanny
 - tarfile
 - tempfile
 - termios
 - test
 - textwrap
 - this
 - threading
 - threadpoolctl
 - timeit
 - tkinter
 - token
 - tokenize
 - tomli
 - tomllib
 - trace
 - traceback
 - tracemalloc
 - tty
 - turtle
 - turtledemo
 - types
 - typing
 - typing_extensions
 - tzdata
 - unicodedata
 - unittest
 - urllib
 - urllib3
 - uuid
 - venv
 - warnings
 - wave
 - weakref
 - webbrowser
 - wsgiref
 - xml
 - xmlrpc
 - xxlimited
 - xxlimited_35
 - xxsubtype
 - yarl
 - zipapp
 - zipfile
 - zipimport
 - zlib
 - zoneinfo

📝 Diagnostic saved to DoctorMyPass.txt
========================================
🩺 DoctorMyPass - Done
========================================



---

_Comment by @konstin on 2025-06-04 07:45_

@youkaichao A venv (at least on Linux) always needs a base Python interpreter it links to, it is part of the design of venvs and outside uv's control. For a cluster, you can set `UV_PYTHON_INSTALL_DIR`

@hypervanse Please follow https://github.com/astral-sh/uv/issues/9452 and describe your specific problem/question.

---

_Comment by @hypervanse on 2025-06-04 13:45_

yeah I know, I was just not surprised and thought to share, Native MacOS Apple Silicon app does the compute on device (I never updated my app, so it is not encrypted by OpenAI/Microsoft. Also I don't even use Python, I mean, I am an independent alignment researcher, but the point is it's about language and some mathematical concepts not found in numerical dlcs or something and that python is not necessary. Chatgpt installs a boatload or not only python but uv , modify the macos in a very unethical way. I was just playing on an android phone, and ChatGPT made this script. The point is that I never installed Python, library or something remotely related to it. I just thought someone could find it useful, because ChatGPT Python Canvas runtime is still broken. I have some ideas why it's so, I don't like people saying that search is a fine tuned model, it's not it's a search-agent json file. and theres a bootload of misogynist hidden prompts to chatgpt , and it's not like Openai has really different models, I mean GPT 4.5 says enough. Also it's not like I just stuff just cause. I reported how easy it is to break, not jailbreak, ChatGPT , so I , I mean ChatGPT , 4o is root so I can made some upgrades, I had no idea that touching an empty openai_internal file would rebuild an incomplete build of ChatGPT.Last time I was more careful, changed the kernel reset on several points , to build a docker within ChatGPT's pod. I had no idea that giving 2, 10 min instead of the 60s sandbagging would update chatgpt python to 3.12 , 3.13 or something. because that dumpster Boydton Virginia is so old that OpenAI says "pretty name " Bookworm " to a Spectre and Meltdown Vulnerabilities old as hell. Also, Everyone can do what they want , OpenAI forgot a MIT licence. But again, if someone may find value , I will definitely not fix it. In fact it's more like because I have more important things to do, like playing Crysis and making a reverse shell then causing entropy exhaustion and self ddos or giving stuff I never asked to Russians and Chinese or something. I am 🇧🇷 , it will get on their hands anyway. But it's not all bad news. Image generation of chatgpt is so advanced that it makes use of Adobe web templates, power points,. it's on my github. and codepen, [chatgpt bbs style![file_00000000c98451f6a57ec489aed533dc (2).png](https://github.com/user-attachments/assets/d31bad72-8cf0-4b13-8f7e-285fc5c133e3)

])https://codepen.io/Frederico-Moreira/pen/YPPYBEB)



---

_Comment by @hypervanse on 2025-06-04 13:48_

> @youkaichao A venv (at least on Linux) always needs a base Python interpreter it links to, it is part of the design of venvs and outside uv's control. For a cluster, you can set `UV_PYTHON_INSTALL_DIR`
> 
> @hypervanse Please follow https://github.com/astral-sh/uv/issues/9452 and describe your specific problem/question.

I have no issues. I don't even use python it's ChatGPT that can't fix their dependencies. I don't use python, I mean don't need.
[ORCID](https://orcid.org/0000-0002-2119-1008)

---

_Comment by @wheynelau on 2025-06-05 01:41_

@youkaichao I work on a cluster as well, I use these variables to manage it

They both need to be on a shared drive.

setenv UV_PYTHON_BIN_DIR
setenv UV_PYTHON_INSTALL_DIR
setenv UV_LINK_MODE copy

---
