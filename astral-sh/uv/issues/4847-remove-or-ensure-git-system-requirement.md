```yaml
number: 4847
title: Remove (or ensure) git system requirement
type: issue
state: open
author: wyattscarpenter
labels:
  - wish
assignees: []
created_at: 2024-07-06T17:07:17Z
updated_at: 2026-01-16T13:47:44Z
url: https://github.com/astral-sh/uv/issues/4847
synced_at: 2026-01-16T13:57:13Z
```

# Remove (or ensure) git system requirement

---

_@wyattscarpenter_

As far as I can tell, Git is required to be installed on the host system in order for pip git sources to work. This is bad, from a software engineering perspective, because a program should not require other programs in order to work. It is also bad because when you install `uv` using `pip`, you do not get `git` as well (which would be another way of avoiding the dependency failure).

This behavior is understandable, because pip also works like this, but unless using the git executable as pip would is crucial for compatibility reasons, I think uv should just support the git operations itself (and also, I guess, the Mercurial, Subversion, and Bazaar operations, detailed on https://pip.pypa.io/en/stable/topics/vcs-support/). As I understand it, the required operations are basically just downloading and unpacking the repo, so it shouldn't be that hard™.

## Example

```sh
$ pip install -U uv #this doesn't install git or anything.
# (I've omitted the output of this command for brevity.)

$ uv --version
uv 0.2.21 (ebfe6d8fc 2024-07-03)

$ echo "mypy\nblack @ git+https://github.com/psf/black" >requirements.txt

$ uv venv && uv pip install -r requirements.txt
Using Python 3.12.3 interpreter at: [redacted]
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate
Updating https://github.com/psf/black (HEAD)                                                                            error: Failed to download and build: `black @ git+https://github.com/psf/black`
  Caused by: Git operation failed
  Caused by: could not execute process `git init` (never executed)
  Caused by: program not found

$ uv venv && uv pip install -r requirements.txt #switching to a machine where git is installed and on the path, for comparison, and it works fine
Using Python 3.10.12 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
 Updated https://github.com/psf/black (7e2afc9)                                                                                                                                                                                              Resolved 9 packages in 29.34s
   Built black @ git+https://github.com/psf/black@7e2afc9bfdc4ec4bc3297aaa16a62d575249a5e0                                                                                                                                                   Downloaded 1 package in 7.92s
Installed 9 packages in 1m 40s
 + black==24.4.3.dev23+g7e2afc9 (from git+https://github.com/psf/black@7e2afc9bfdc4ec4bc3297aaa16a62d575249a5e0)
 + click==8.1.7
 + mypy==1.10.1
 + mypy-extensions==1.0.0
 + packaging==24.1
 + pathspec==0.12.1
 + platformdirs==4.2.2
 + tomli==2.0.1
 + typing-extensions==4.12.2
```



---

_Comment by @FishAlchemist on 2024-07-06 17:45_

I was under the impression that this PR(https://github.com/astral-sh/uv/pull/3833) remove embedded Git support.
You should be able to understand the Git issue from the description of that PR.


---

_Comment by @charliermarsh on 2024-07-06 19:00_

We may eventually move to [gitoxide](https://github.com/Byron/gitoxide) if it grows to fit our needs, but otherwise this is sort of a non-goal right now.

---

_Label `wish` added by @charliermarsh on 2024-07-06 19:00_

---

_Comment by @zanieb on 2024-07-06 19:18_

Our git operations are definitely non-trivial too as we need to support resolving all sorts of references, various authentication schemes, and optimizations like sparse checkouts. Once a suitable library is available we could consider support again but the system `git` client gives the most complete set of functionality.

---

_Renamed from "Remove git system requirement" to "Remove (or ensure) git system requirement" by @wyattscarpenter on 2024-07-08 09:57_

---

_Comment by @Henkhogan on 2024-10-10 17:41_

I am confused about the README of uv saying

> uv's Git implementation is based on [Cargo](https://github.com/rust-lang/cargo).


and then cargo's README

> Optional system libraries:
> 
> The build will automatically use vendored versions of the following libraries. However, if they are provided by the system and can be found with pkg-config, then the system libraries will be used instead:
> 
> [libcurl](https://curl.se/libcurl/) — Used for network transfers.
> **[libgit2](https://libgit2.org/) — Used for fetching git dependencies.**
> [libssh2](https://www.libssh2.org/) — Used for SSH access to git repositories.
> [libz](https://zlib.net/) (aka zlib) — Used for data compression.
> It is recommended to use the vendored versions as they are the versions that are tested to work with Cargo.

Thus I expected there would be no need to have git installed

---

_Comment by @zanieb on 2024-10-10 17:45_

We don't use libgit2 anymore, it doesn't support enough functionality. Originally, we copied much of Cargo's implementation though — Cargo can fallback to git if you ask it to.

---

_Comment by @andreasgriffin on 2026-01-16 13:47_

Just as @wyattscarpenter   I also would like to see (basic) git included in `uv`, especially because `poetry` does include git, and I ran into this problem in my [attempt](https://github.com/andreasgriffin/bitcoin-safe/pull/403) to switch to `uv`.

---
