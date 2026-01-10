---
number: 2062
title: Unresolved imports in projects on network drives
type: issue
state: closed
author: arvidmor
labels:
  - bug
  - great-writeup
  - windows
  - imports
assignees: []
created_at: 2025-12-18T11:41:26Z
updated_at: 2025-12-24T15:36:19Z
url: https://github.com/astral-sh/ty/issues/2062
synced_at: 2026-01-10T01:52:52Z
---

# Unresolved imports in projects on network drives

---

_Issue opened by @arvidmor on 2025-12-18 11:41_

### Summary

At work, our workstations connect to internal servers for storing our daily work. We have several of these servers, which are all set up slightly differently, though I don't know the details of this (sorry). At least one of them uses a NetApp solution of some kind. The issue reproduces on all of them.

I cannot get ty to find any installed dependencies when the project is located on any of these network drives. 
My client is running Windows 11 Enterprise 23H2.

### MRE
Setup a new project on a network drive and import a 3rd party package
```powershell
$ uv init example
Initialized project `example` at `H:\example`
$ cd example && .venv/Scripts/activate.ps1
$ uv add openpyxl ty # or any other 3rd party package
$ echo "import openpyxl" > main.py
```
Run ty
```powershell
$ ty check .
error[unresolved-import]: Cannot resolve imported module `openpyxl`
 --> main.py:1:8
  |
1 | import openpyxl
  |        ^^^^^^^^
  |
info: Searched in the following paths during module resolution:
info:   1. H:\example (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. \\?\UNC\servername.at.corp.se\home$\username\example\.venv\Lib\site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default
```
### Additional information
Running `ls` on the site-packages path returns nothing. If the path doesn't exist it would normally print an error (I'm using powershell here).

```powershell
$ ls \\?\UNC\servername.at.corp.se\home$\username\example\.venv\Lib\site-packages
$
```
But removing the "?\UNC\" works: 
```powershell
$ ls \\servername.at.corp.se\home$\username\example\.venv\Lib\site-packages

    Directory: \\servername.at.corp.se\home$\username\example\.venv\Lib\site-packages

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----          2025-12-18    11:44                __pycache__
d----          2025-12-18    11:44                et_xmlfile
d----          2025-12-18    11:41                et_xmlfile-2.0.0.dist-info
d----          2025-12-18    11:44                openpyxl
d----          2025-12-18    11:41                openpyxl-3.1.5.dist-info
d----          2025-12-18    11:48                ty
d----          2025-12-18    11:48                ty-0.0.3.dist-info
-a---          2025-12-18    11:41             18 _virtualenv.pth
-a---          2025-12-18    11:41           4342 _virtualenv.py

```

### Version

0.0.3

---

_Comment by @MichaReiser on 2025-12-18 12:19_

Hmm, I wonder where we get this path from. Can you run `ty check -v` and share the logs with us?

---

_Label `needs-info` added by @MichaReiser on 2025-12-18 12:19_

---

_Label `imports` added by @MichaReiser on 2025-12-18 12:19_

---

_Comment by @arvidmor on 2025-12-18 12:30_

> Hmm, I wonder where we get this path from. Can you run `ty check -v` and share the logs with us?

Sure, here's the output of the command. If by 'logs' you're referring to something else, please point me in the right direction :)
```powershell
(example) PS H:\example> ty check -v
INFO Defaulting to python-platform `win32`
INFO Python version: Python 3.13, platform: win32
INFO Indexed 1 file(s) in 0.033s
error[unresolved-import]: Cannot resolve imported module `openpyxl`
 --> main.py:1:8
  |
1 | import openpyxl
  |        ^^^^^^^^
  |
info: Searched in the following paths during module resolution:
info:   1. H:\example (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. \\?\UNC\servername.at.corp.se\home$\username\example\.venv\Lib\site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default
```
H: just points to my home directory on the server, hence the "home$\username\".

---

_Label `windows` added by @AlexWaygood on 2025-12-18 12:32_

---

_Comment by @MichaReiser on 2025-12-18 12:35_

Hmm, sorry. I should have asked you to run `ty check -vv` to get more detailed logs.

---

_Comment by @arvidmor on 2025-12-18 12:39_

Alright, here they are!
```powershell
(example) PS H:\example> ty check -vv
2025-12-18 13:35:18.8501786 DEBUG Version: 0.0.3 (fadfe0966 2025-12-17)
2025-12-18 13:35:18.8538037 DEBUG Architecture: x86_64, OS: windows, case-sensitive: unknown
2025-12-18 13:35:18.8538768 DEBUG Searching for a project in 'H:\example'
2025-12-18 13:35:18.8655172 DEBUG Resolving requires-python constraint: `>=3.13`
2025-12-18 13:35:18.8657147 DEBUG Resolved requires-python constraint to: 3.13
2025-12-18 13:35:18.8717641 DEBUG Project without `tool.ty` section: 'H:\example'
2025-12-18 13:35:18.8719901 DEBUG Searching for a user-level configuration at `C:\Users\username\AppData\Roaming\ty\ty.toml`
2025-12-18 13:35:18.8733746 INFO Defaulting to python-platform `win32`
2025-12-18 13:35:18.8735183 DEBUG Resolving `VIRTUAL_ENV` environment variable: H:\example\.venv
2025-12-18 13:35:18.8949458 DEBUG Attempting to parse virtual environment metadata at '\\?\UNC\servername.at.corp.se\home$\username\example\.venv\pyvenv.cfg'
2025-12-18 13:35:18.9279275 DEBUG Searching for site-packages directory in sys.prefix \\?\UNC\servername.at.corp.se\home$\username\example\.venv
2025-12-18 13:35:18.9434707 DEBUG Resolved site-packages directories for this virtual environment are: ["\\\\?\\UNC\\servername.at.corp.se\\home$\\username\\example\\.venv\\Lib\\site-packages"]
2025-12-18 13:35:18.944126 DEBUG Searching for real stdlib directory in sys.prefix C:\Users\username\AppData\Local\Programs\Python\Python313
2025-12-18 13:35:18.944381 DEBUG Resolved real stdlib path for this virtual environment is: C:\Users\username\AppData\Local\Programs\Python\Python313\Lib
2025-12-18 13:35:18.9504509 DEBUG Including `.` in `environment.root`
2025-12-18 13:35:18.9534431 DEBUG Adding first-party search path `H:\example`
2025-12-18 13:35:18.96296 DEBUG Using vendored stdlib
2025-12-18 13:35:18.9659672 DEBUG Adding site-packages search path `\\?\UNC\servername.at.corp.se\home$\username\example\.venv\Lib\site-packages`
2025-12-18 13:35:18.9789951 INFO Python version: Python 3.13, platform: win32
2025-12-18 13:35:18.979152 DEBUG Adding new file root '\\?\UNC\servername.at.corp.se\home$\username\example\.venv\Lib\site-packages' of kind LibrarySearchPath
2025-12-18 13:35:18.9800747 DEBUG Adding new file root 'H:\example' of kind Project
2025-12-18 13:35:18.9803392 DEBUG Starting main loop
2025-12-18 13:35:18.9804181 DEBUG Waiting for next main loop message.
2025-12-18 13:35:18.980524 DEBUG Checking all files in project 'example'
2025-12-18 13:35:19.0939531 DEBUG Skipping directory 'H:\example\.git' because it is excluded by a default or `src.exclude` pattern
2025-12-18 13:35:19.0959763 INFO Indexed 1 file(s) in 0.115s
2025-12-18 13:35:19.1025544 DEBUG Checking file 'H:\example\main.py'
2025-12-18 13:35:19.1141865 DEBUG Resolving dynamic module resolution paths
2025-12-18 13:35:19.1424371 DEBUG Module `openpyxl` not found in search paths
2025-12-18 13:35:19.1438959 DEBUG Module `openpyxl` not found while looking in parent dirs (neither stub nor real module file)
2025-12-18 13:35:19.1442785 DEBUG Checking all files took 0.048s
error[unresolved-import]: Cannot resolve imported module `openpyxl`
 --> main.py:1:8
  |
1 | import openpyxl
  |        ^^^^^^^^
  |
info: Searched in the following paths during module resolution:
info:   1. H:\example (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. \\?\UNC\servername.at.corp.se\home$\username\example\.venv\Lib\site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
2025-12-18 13:35:19.1453447 DEBUG Exiting main loop
```

---

_Label `needs-info` removed by @MichaReiser on 2025-12-19 12:10_

---

_Comment by @MichaReiser on 2025-12-19 12:12_

I suspect that ty gets all confused about `H:\example\.venv` expanding to `\\?\UNC\servername.at.corp.se\home$\username\example\.venv\pyvenv.cfg`. I would have to look back at why we canonicalize virtual environments in the first place. If not, that might be the easiest fix. It's also curious that we can't remove the UNC prefix. 

But this will require me to set up a Windows network drive.

Just for completness, would you mind sharing what's in `H:\example\.venv\Lib\site-packages` (just a directory listing with `dir`)

---

_Comment by @striker4150 on 2025-12-21 23:28_

Hello, I'm having the exact same issue:
```powershell
(copy-rated) PS Z:\Scripts\copy_rated> ty check
error[unresolved-import]: Cannot resolve imported module `filelock`
 --> copy_rated.py:7:6
  |
5 | from itertools import tee
6 |
7 | from filelock import FileLock
  |      ^^^^^^^^
  |
info: Searched in the following paths during module resolution:
info:   1. Z:\Scripts\copy_rated (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. \\?\UNC\SERVERNAME\E\Scripts\copy_rated\.venv\Lib\site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

...

Found 4 diagnostics
```
(Extra unresolved imports removed from output)

---

_Comment by @JTP3XP on 2025-12-22 20:59_

@MichaReiser I'm running into this as well. I see the OP did not respond to your request for a listing of the contents of their site-packages folder, but I can confirm the issue is not that the packages are missing from that folder. 

I also tried running `ty check --python` and specifying the .venv's python executable, to make sure it is not an issue with .venv discovery, and confirmed it is not.

For example:
```
> uv run ty check --python \\?\UNC\share.domain.local\share\Profiles\JPatton\Repos\this_repo\.venv\Scripts\python.exe
error[unresolved-import]: Cannot resolve imported module `pandas`
 --> main.py:4:6
  |
2 | import os
3 |
4 | from pandas import DataFrame, concat, read_sql
  |      ^^^^^^
...
info: Searched in the following paths during module resolution:
info:   1. F:\Profiles\JPatton\Repos\this_repo (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. \\?\UNC\share.domain.local\share\Profiles\JPatton\Repos\this_repo\.venv\Lib\site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default
```

Here is a dir command:
```
> dir -LiteralPath \\?\UNC\share.domain.local\share\Profiles\JPatton\Repos\this_repo\.venv\Lib\site-packages


    Directory: \\?\UNC\share.domain.local\share\Profiles\JPatton\Repos\this_repo\.venv\Lib\site-packages


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        12/22/2025   2:59 PM                certifi
d-----        12/22/2025   2:59 PM                certifi-2025.11.12.dist-info
d-----        12/22/2025   2:59 PM                charset_normalizer
d-----        12/22/2025   2:59 PM                charset_normalizer-3.4.4.dist-info
d-----        12/22/2025   2:59 PM                dateutil
d-----        12/22/2025   2:59 PM                dotenv
d-----        12/22/2025   2:59 PM                greenlet
d-----        12/22/2025   2:59 PM                greenlet-3.3.0.dist-info
d-----        12/22/2025   2:59 PM                idna
d-----        12/22/2025   2:59 PM                idna-3.11.dist-info
d-----        12/22/2025   2:59 PM                inflection
d-----        12/22/2025   2:59 PM                inflection-0.5.1.dist-info
d-----        12/22/2025   3:00 PM                numpy
d-----        12/22/2025   3:00 PM                numpy-2.3.5.dist-info
d-----        12/22/2025   3:00 PM                numpy.libs
d-----        12/22/2025   3:00 PM                pandas
d-----        12/22/2025   3:00 PM                pandas-2.3.3.dist-info
d-----        12/22/2025   3:00 PM                pandas.libs
d-----        12/22/2025   2:59 PM                python_dateutil-2.9.0.post0.dist-info
d-----        12/22/2025   2:59 PM                python_dotenv-1.2.1.dist-info
d-----        12/22/2025   2:59 PM                pytz
d-----        12/22/2025   2:59 PM                pytz-2025.2.dist-info
d-----        12/22/2025   2:59 PM                requests
d-----        12/22/2025   2:59 PM                requests-2.32.5.dist-info
d-----        12/22/2025   2:59 PM                ruff
d-----        12/22/2025   2:59 PM                ruff-0.14.10.dist-info
d-----        12/22/2025   2:59 PM                six-1.17.0.dist-info
d-----        12/22/2025   2:59 PM                sqlalchemy
d-----        12/22/2025   2:59 PM                sqlalchemy-2.0.45.dist-info
d-----        12/22/2025   3:01 PM                ty
d-----        12/22/2025   3:01 PM                ty-0.0.5.dist-info
d-----        12/22/2025   2:59 PM                typing_extensions-4.15.0.dist-info
d-----        12/22/2025   2:59 PM                tzdata
d-----        12/22/2025   2:59 PM                tzdata-2025.3.dist-info
d-----        12/22/2025   2:59 PM                urllib3
d-----        12/22/2025   2:59 PM                urllib3-2.6.2.dist-info
d-----        12/22/2025   2:59 PM                __pycache__
-a----        12/22/2025   2:59 PM          11437 inflection.py
-a----        12/22/2025   2:59 PM          34703 six.py
-a----        12/22/2025   2:59 PM         160429 typing_extensions.py
-a----        12/22/2025   2:58 PM             18 _virtualenv.pth
-a----        12/22/2025   2:58 PM           4342 _virtualenv.py
```

This is the same output as `dir F:\Profiles\JPatton\Repos\this_repo\.venv\Lib\site-packages`, with powershell needing the `-LiteralPath` argument because of the `?` in the UNC path here.

Let me know if there is anything else that would be helpful to see.

---

_Comment by @MichaReiser on 2025-12-23 08:49_

Thanks for the extra detail. I plan to debug this once I finished my open PRs adding support for `--add-ignore` (and maybe `--fix`) and once I've access to my Windows machine

---

_Label `bug` added by @MichaReiser on 2025-12-23 08:49_

---

_Comment by @MichaReiser on 2025-12-24 14:03_

Thanks for the amazing repro @arvidmor. It made it very easy to reproduce the issue. Now let's debug what goes wrong.

---

_Label `great-writeup` added by @MichaReiser on 2025-12-24 14:03_

---

_Comment by @arvidmor on 2025-12-24 14:08_

Sorry for the delayed response. I didn't think about the fact that I created this the day before Christmas vacation... I don't have access to the environment until early January but I'll be sure to check back in first thing when i get back. 

Do let me know if there's any more info i can provide. And thanks a lot for the quick replies :-)

---

_Comment by @MichaReiser on 2025-12-24 14:14_

No worries. Your reproduction is perfect and allows to verify the fix on my end. 

Commenting out this line makes the imports resolve:

https://github.com/astral-sh/ruff/blob/b4c2825afdd8c1010c3a5859521629d2d1e0a6df/crates/ty_module_resolver/src/resolve.rs#L1359-L1364

Looks like something goes wrong in our (annoyingly complicated) logic to ensure the module name in the import statement and the file name on disk match exactly.

---

_Closed by @MichaReiser on 2025-12-24 15:36_

---
