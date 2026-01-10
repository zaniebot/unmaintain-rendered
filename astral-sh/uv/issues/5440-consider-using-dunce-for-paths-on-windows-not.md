```yaml
number: 5440
title: "Consider using `dunce` for paths on Windows (not just for display)"
type: issue
state: closed
author: paveldikov
labels:
  - bug
assignees: []
created_at: 2024-07-25T10:56:31Z
updated_at: 2024-07-26T14:39:28Z
url: https://github.com/astral-sh/uv/issues/5440
synced_at: 2026-01-10T04:53:49Z
```

# Consider using `dunce` for paths on Windows (not just for display)

---

_Issue opened by @paveldikov on 2024-07-25 10:56_

_As originally mentioned in https://github.com/astral-sh/uv/issues/1491#issuecomment-2247703908_

Consider the scenario where the Python interpreter is distributed via a network share path. e.g. `\\some-host\some-share\...\python.exe`. Obviously this is not very common but the same sort of path notation can be used for local disk drives, e.g. `\\localhost\c$\` -- which is what I use for this reproducer:

```
$ uv venv --python "//localhost/c$/Program Files/Python310/python.exe" test310
Using Python 3.10.11 interpreter at: \\localhost\c$\Program Files\Python310\python.exe
Creating virtualenv at: test310
Activate with: source test310\Scripts\activate
$ cat test310/pyvenv.cfg
home = \\?\UNC\localhost\c$\Program Files\Python310
implementation = CPython
uv = 0.2.29
version_info = 3.10.11
include-system-site-packages = false
$ test310/Scripts/python.exe
Python 3.10.11 (tags/v3.10.11:7d4cc5a, Apr  5 2023, 00:38:17) [MSC v.1929 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import sqlite3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "\\?\UNC\localhost\c$\Program Files\Python310\lib\sqlite3\__init__.py", line 57, in <module>
    from sqlite3.dbapi2 import *
  File "\\?\UNC\localhost\c$\Program Files\Python310\lib\sqlite3\dbapi2.py", line 27, in <module>
    from _sqlite3 import *
ImportError: DLL load failed while importing _sqlite3: The parameter is incorrect.
```

`dunce`'ing the path by hand in `pyvenv.cfg` fixes it for me:

```
$ cat test310/pyvenv.cfg
home = \\localhost\c$\Program Files\Python310
implementation = CPython
uv = 0.2.29
version_info = 3.10.11
include-system-site-packages = false
$ test310/Scripts/python.exe
Python 3.10.11 (tags/v3.10.11:7d4cc5a, Apr  5 2023, 00:38:17) [MSC v.1929 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import sqlite3
>>> sqlite3
<module 'sqlite3' from '\\\\localhost\\c$\\Program Files\\Python310\\lib\\sqlite3\\__init__.py'>
```

This issue is reproducible with 3.10.11, but not 3.11. (presumably this bad handling of UNC paths got fixed in CPython down the line?)

            

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-25 13:43_

---

_Label `bug` added by @charliermarsh on 2024-07-25 13:43_

---

_Comment by @charliermarsh on 2024-07-25 13:48_

Hmm, the values in `pyvenv` already go through dunce: https://github.com/astral-sh/uv/blob/375a4152fa0d9359ea1f338e8345a5281bf795b2/crates/uv-virtualenv/src/virtualenv.rs#L316

---

_Comment by @charliermarsh on 2024-07-25 13:49_

Can you test a debug build if I push a branch?

---

_Comment by @charliermarsh on 2024-07-25 13:50_

My only guess is that we need to avoid calling the non-safe `canonicalize` on the excecutable in the first place (e.g., change `canonicalize_executable` to use `path.simple_canonicalize()` instead of `fs_err::canonicalize(path)`.

---

_Comment by @paveldikov on 2024-07-25 14:01_

> Can you test a debug build if I push a branch?

Happy to test.

> My only guess is that we need to avoid calling the non-safe canonicalize on the excecutable in the first place (e.g., change canonicalize_executable to use path.simple_canonicalize() instead of fs_err::canonicalize(path).

Yeah, and I wonder if this will actually make the code _simpler_? (assuming that this is going to be a near-total revert of #1086. If so, win-win!)


---

_Comment by @charliermarsh on 2024-07-25 15:00_

Can we try https://github.com/astral-sh/uv/pull/5446 to start, see if the venv is any different?

---

_Comment by @paveldikov on 2024-07-25 15:30_

No change.

```
$ cargo run -- venv --python "//localhost/c$/Program Files/Python310/python.exe" test310
Using Python 3.10.11 interpreter at: \\localhost\c$\Program Files\Python310\python.exe
Creating virtualenv at: test310
Activate with: source test310\Scripts\activate
$ cat test310/pyvenv.cfg
home = \\?\UNC\localhost\c$\Program Files\Python310
implementation = CPython
uv = 0.2.29
version_info = 3.10.11
include-system-site-packages = false
```

---

_Comment by @charliermarsh on 2024-07-25 15:47_

Can you try with the update I just pushed? Trying to understand where the prefix is coming from, confused.

---

_Comment by @paveldikov on 2024-07-25 16:15_

```
$ cargo run -- venv --python "//localhost/c$/Program Files/Python310/python.exe" test310
     Running `target\debug\uv.exe venv --python '//localhost/c$/Program Files/Python310/python.exe' test310`
Using Python 3.10.11 interpreter at: \\localhost\c$\Program Files\Python310\python.exe
Creating virtualenv at: test310
base_python: "\\\\?\\UNC\\localhost\\c$\\Program Files\\Python310\\python.exe"
sys_executable: "\\\\localhost\\c$\\Program Files\\Python310\\python.exe"
sys_base_executable: Some("\\\\localhost\\c$\\Program Files\\Python310\\python.exe")
Activate with: source test310\Scripts\activate
```

pyvenv.cfg unchanged
```
$ cat test310/pyvenv.cfg
home = \\?\UNC\localhost\c$\Program Files\Python310
implementation = CPython
uv = 0.2.29
version_info = 3.10.11
include-system-site-packages = false
```

---

_Comment by @charliermarsh on 2024-07-25 16:20_

My guess is that this path is actually _not_ safe to strip though I'm not intimately familiar with the rules:

```rust
fn is_safe_to_strip_unc(path: &Path) -> bool {
    let mut components = path.components();
    match components.next() {
        Some(Component::Prefix(p)) => match p.kind() {
            Prefix::VerbatimDisk(..) => {},
            _ => return false, // Other kinds of UNC paths
        },
        _ => return false, // relative or empty
    }

    for component in components {
        match component {
            Component::RootDir => {},
            Component::Normal(file_name) => {
                // it doesn't allocate in most cases,
                // and checks are interested only in the ASCII subset, so lossy is fine
                if !is_valid_filename(file_name) || is_reserved(file_name) {
                    return false;
                }
            }
            _ => return false, // UNC paths take things like ".." literally
        };
    }

    if windows_char_len(path.as_os_str()) > 260 { // However, if the path is going to be used as a directory it's 248
        return false;
    }
    true
}
```

---

_Comment by @charliermarsh on 2024-07-25 16:23_

Will look deeper when I can, but need to prioritize a few other things first today. Sorry!

---

_Comment by @paveldikov on 2024-07-25 16:30_

No worries!

Judging from the log lines it must be safe to strip in at least some contexts:
```
sys_executable: "\\\\localhost\\c$\\Program Files\\Python310\\python.exe"
sys_base_executable: Some("\\\\localhost\\c$\\Program Files\\Python310\\python.exe")
```


---

_Comment by @charliermarsh on 2024-07-25 16:33_

I think that's the original, unmodified path we get from `sys.executable` in Python. Then we pass it to `uv_fs::canonicalize_executable(interpreter.sys_executable())` to get `base_python`... which adds the prefix, which dunce then tries to strip if it can (but seemingly can't).

---

_Comment by @paveldikov on 2024-07-25 17:35_

Naive question: do we really need to canonicalise on Windows? (we aren't following symlinks as we are on Linux)

Comment in code says:

> We canonicalize the path to ensure that it's real and consistent, though we don't expect any symlinks on Windows.

We generally know that it's _real_ beforehand -- though perhaps not 'real' in the POSIX sense. See the `Using Python .... interpreter at:` printout (although this happens in a very different place in the control flow)

Also perhaps it's something to do with the dollar sign? I am in the predicament of trying to replicate this issue in my home environment and I don't really happen to have network shares laying around :-/ so I thought I'd fake this by using this notation on my C: drive...

---

_Comment by @charliermarsh on 2024-07-25 17:41_

Hmm. We _probably_ don't.

---

_Comment by @paveldikov on 2024-07-25 17:50_

P.S. it's not the dollar sign. Turns out I can get an arbitrary `\\` path using a shared folder. No special characters involved, but still gets UNC'd.
```
home = \\?\UNC\DESKTOP\Users\Pavel\share\Python310
```

---

_Comment by @charliermarsh on 2024-07-25 17:52_

Yeah. I assume dunce is correct that it's not unequivocally safe to strip that (maybe leads to an ambiguity) but again mercifully I'm not an expert in Windows paths. We can experiment with not canonicalizing paths on Windows at all.

---

_Comment by @charliermarsh on 2024-07-25 20:56_

I pushed a new version to that same branch / PR that avoids canonicalizing on Windows, if you want to try it.

---

_Comment by @paveldikov on 2024-07-25 23:25_

Third time's the charm! 👍 
```
home = \\localhost\c$\Program Files\Python310
```

Looking at the `dunce` code you quoted -- this all tracks. It seems to only consider as 'safe' paths starting with a drive letter e.g. `C:`, else gives up completely:
```
    match components.next() {
        Some(Component::Prefix(p)) => match p.kind() {
            Prefix::VerbatimDisk(..) => {},
            _ => return false, // Other kinds of UNC paths
        },
        _ => return false, // relative or empty
    }
```

IMHO this is overly conservative, but I am likewise not a Windows expert -- just someone that has to live with its quirks! Also found this conversation @ [someone's `dunce` bug report](https://gitlab.com/kornelski/dunce/-/issues/2). I guess the problem here is that once (mis-)canonicalisation adds the prefix, it is impossible to safely to strip it back away.

So... a `dunce` issue that is effectively WONTFIX?

---

_Closed by @charliermarsh on 2024-07-26 12:57_

---

_Comment by @paveldikov on 2024-07-26 14:39_

Thanks a million for the quick resolution 🙌 

---
