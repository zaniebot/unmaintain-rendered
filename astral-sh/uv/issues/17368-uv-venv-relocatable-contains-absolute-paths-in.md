```yaml
number: 17368
title: uv venv --relocatable contains absolute paths in csh activate script
type: issue
state: open
author: alastair
labels:
  - bug
  - breaking
assignees: []
created_at: 2026-01-08T21:58:59Z
updated_at: 2026-01-09T18:20:30Z
url: https://github.com/astral-sh/uv/issues/17368
synced_at: 2026-01-12T16:02:49Z
```

# uv venv --relocatable contains absolute paths in csh activate script

---

_@alastair_

### Summary

when running `uv venv --relocatable` I expect to see no absolute paths to the location of the venv inside itself, however I see that the csh script still contains `VIRTUAL_ENV` pointing to the path.

I don't use csh, just thought it was curious and that maybe it was a bug.

```
alastair@apmini:~/venv-test$ uv venv
Using CPython 3.12.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
alastair@apmini:~/venv-test$ ack alastair .venv/bin
.venv/bin/activate.bat
29:@for %%i in ("/Users/alastair/venv-test/.venv") do @set "VIRTUAL_ENV=%%~fi"

.venv/bin/activate.fish
82:set -gx VIRTUAL_ENV '/Users/alastair/venv-test/.venv'

.venv/bin/activate
81:VIRTUAL_ENV='/Users/alastair/venv-test/.venv'

.venv/bin/activate.nu
71:    let virtual_env = '/Users/alastair/venv-test/.venv'

.venv/bin/activate.csh
34:setenv VIRTUAL_ENV '/Users/alastair/venv-test/.venv'
alastair@apmini:~/venv-test$ rm -rf .venv/
alastair@apmini:~/venv-test$ uv venv --relocatable
Using CPython 3.12.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
alastair@apmini:~/venv-test$ ack alastair .venv/bin
.venv/bin/activate.csh
34:setenv VIRTUAL_ENV '/Users/alastair/venv-test/.venv'
```

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.9.21 (Homebrew 2025-12-30)

### Python version

Python 3.12.0

---

_Label `bug` added by @alastair on 2026-01-08 21:59_

---

_Comment by @zanieb on 2026-01-08 22:06_

Related https://github.com/astral-sh/uv/issues/16973

---

_Comment by @zanieb on 2026-01-08 22:06_

(I'm not sure it's possible to fix this in csh?)

---

_Comment by @EliteTK on 2026-01-09 17:57_

I had a look at this yesterday on tcsh (has more features than plain csh, but do we actually support real csh?) and I genuinely couldn't find any way to do this.

We could "support" relocatable csh by checking if the user passed an argument when sourcing which itself would be the path to the script. And if not, we could tell them to do so.

So to activate a relocatable venv from csh you would do `source path/to/venv/activate.csh path/to/venv`...

Probably easier to just not create the script in that circumstance instead though...

---

_Comment by @zanieb on 2026-01-09 18:14_

Can we detect if we have tcsh instead of csh and just hard-fail when we don't have tcsh under relocatable mode?

---

_Comment by @EliteTK on 2026-01-09 18:19_

We can detect tcsh[^1]. But I meant that there is no automated way to do this on tcsh and also probably csh. I didn't bother trying to find a copy of the original csh, but since tcsh is more or less just an extended version of csh, it seems unlikely that it has some way to do this anyway.

[^1]: It sets a `tcsh` variable with its version.

---

_Comment by @zanieb on 2026-01-09 18:20_

Oh I thought you were saying it _was_ possible in tcsh.

I guess we can just omit this file when `--relocatable` is used as a breaking change in 0.10

---

_Label `breaking` added by @zanieb on 2026-01-09 18:20_

---

_Added to milestone `v0.10.0` by @zanieb on 2026-01-09 18:20_

---
