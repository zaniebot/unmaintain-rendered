---
number: 6968
title: uv interprets url as local file
type: issue
state: closed
author: bqback
labels:
  - good first issue
  - help wanted
  - error messages
assignees: []
created_at: 2024-09-03T12:48:54Z
updated_at: 2025-03-21T05:30:47Z
url: https://github.com/astral-sh/uv/issues/6968
synced_at: 2026-01-10T01:24:08Z
---

# uv interprets url as local file

---

_Issue opened by @bqback on 2024-09-03 12:48_

I used the following command:

`uv pip install "git+git@server.com:path/to/lib.git#egg=lib"`

and I get an error that hints at it being interpreted as a local file for some reason? Verbose output:

```
DEBUG uv 0.2.32
DEBUG Searching for Python interpreter in system path or `py` launcher
DEBUG Found `cpython-3.12.4-windows-x86_64-none` at `C:\work\.venv\Scripts\python.exe` (active virtual environment)   
DEBUG Using Python 3.12.4 environment at .venv\Scripts\python.exe
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: file:///C:/work/git+git@server.com:path/to/lib.git#egg=lib
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `\\?\%LocalAppData%\uv\cache\built-wheels-v3\path\6c86e883763f6cc5`
error: Distribution not found at: file:///work/git+git@server.com:path/to/lib.git#egg=lib
```

Platform is Windows 10, uv version is 0.2.32.

I've also tried doing this without quotes around the url, and running the installation with `-e` explicitly gives me an error message that the url is interpreted as a local file:

```
DEBUG uv 0.2.32
error: Unsupported editable requirement in `requirements_dev.txt`
  Caused by: Editable must refer to a local directory, not an archive: file:///C:/work/file:///work/git+git@server.com:path/to/lib.git#egg=lib`
```

---

_Comment by @charliermarsh on 2024-09-03 12:49_

I think you need a protocol there? Like `git+https://git@server.com:path/to/lib.git#egg=lib"` or `git+ssh://...`.

---

_Label `question` added by @charliermarsh on 2024-09-03 12:49_

---

_Label `error messages` added by @charliermarsh on 2024-09-03 12:50_

---

_Comment by @bqback on 2024-09-03 12:56_

https://pip.pypa.io/en/stable/topics/vcs-support/ lists git+git as a supported schema, albeit one that's not recommended. 
I've also had a hard time finding a public git+git url to try to rule out issues with formatting and the repository not being available (although I'm not sure why the repository being available would lead to it being interpreted as a local file).

I've tried specifying it as 
`uv pip install -e git+git://git@server.com:path/to/lib.git#egg=lib`
but it just wouldn't parse at all with the message
```
error: Failed to parse: `git+git://git@server.com:path/to/lib.git#egg=lib`
Caused by: Expected one of `@`, `(`, `<`, `=`, `>`, `~`, `!`, `;`, found `+`
git+git://git
   ^
```

Running `pip install -r requirements.txt` directly fails with the error
```
ERROR: git+git@server.com:path/to/lib.git#egg=lib is not a valid editable requirement. It should either be a path to a local project or a VCS URL (beginning with bzr+http, bzr+https, bzr+ssh, bzr+sftp, bzr+ftp, bzr+lp, bzr+file, git+http, git+https, git+ssh, git+git, git+file, hg+file, hg+http, hg+https, hg+ssh, hg+static-http, svn+ssh, svn+http, svn+https, svn+svn, svn+file).
```

---

_Comment by @charliermarsh on 2024-09-03 12:59_

Ah right, we knowingly don't support `git+git`, just like we don't support `git+file`.


---

_Label `question` removed by @charliermarsh on 2024-09-03 12:59_

---

_Comment by @charliermarsh on 2024-09-03 12:59_

We should improve the error message but it's expected to fail.

---

_Comment by @bqback on 2024-09-03 13:00_

Understandable! Should I reformat the link to `git+https://`/try installing this one package via pip directly?

---

_Comment by @charliermarsh on 2024-09-03 13:00_

It should work just fine with uv if you use either `git+ssh` or `git+https`!

---

_Label `help wanted` added by @charliermarsh on 2024-09-03 14:13_

---

_Label `good first issue` added by @charliermarsh on 2024-09-03 14:13_

---

_Comment by @jakubkunert on 2024-09-03 21:00_

I can take this issue and try to update error message accordingly :)

---

_Comment by @charliermarsh on 2024-09-04 20:38_

Great!

---

_Comment by @charliermarsh on 2024-09-08 18:32_

@jakubkunert -- Just let me know if you're not able or don't plan to get to it (or need some pointers!), so we can prioritize accordingly.

---

_Comment by @jakubkunert on 2024-09-08 21:54_

@charliermarsh thank you! Will try to solve it tomorrow.

I've already checked it yesterday and on current version I have different error message than posted above.
Will spend with this some more time tomorrow.
But if you have any quick poiters it will be wonderful.

---

_Comment by @charliermarsh on 2024-09-16 02:25_

So:

```
$ uv pip install -e git+git://git@server.com:path/to/lib.git#egg=lib
error: Failed to parse: `git+git://git@server.com:path/to/lib.git#egg=lib`
  Caused by: invalid port number
git+git://git@server.com:path/to/lib.git#egg=lib
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

That looks reasonable to me. It's a URL with an invalid port.

On the other hand:

```
$ cargo run pip install -e git+git://git@server.com/path/to/lib.git#egg=lib
error: Editable must refer to a local directory, not a Git URL: `git+git://git@server.com/path/to/lib.git#egg=lib`
```

That looks right too.

Then:

```
$ cargo run pip install git+git://git@server.com/path/to/lib.git#egg=lib
error: Git operation failed
  Caused by: failed to clone into: /Users/crmarsh/Library/Caches/uv/git-v0/db/44ab598169957ad2
  Caused by: process didn't exit successfully: `git fetch --force --update-head-ok 'git://git@server.com/path/to/lib.git' '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
--- stderr
fatal: unable to look up git@server.com (port 9418) (nodename nor servname provided, or not known)
```

So these all look correct to me on main.


---

_Closed by @charliermarsh on 2024-09-16 02:26_

---
