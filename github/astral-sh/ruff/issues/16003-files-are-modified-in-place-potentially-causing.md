---
number: 16003
title: files are modified in place potentially causing issues with hardlinks
type: issue
state: closed
author: jjhelmus
labels: []
assignees: []
created_at: 2025-02-06T21:50:26Z
updated_at: 2025-02-06T23:14:24Z
url: https://github.com/astral-sh/ruff/issues/16003
synced_at: 2026-01-07T13:12:16-06:00
---

# files are modified in place potentially causing issues with hardlinks

---

_Issue opened by @jjhelmus on 2025-02-06 21:50_

### Description

Ruff seems to modify files in place. When the file being modified is a hardlink all other instances will also be modified. Technically the contents of the inode is modified and the hardlinks refer to this inode. This can cause issues when hardlinks are used as a caching mechanism. 

As an example:
``` 
❯ ruff --version
ruff 0.3.5
❯ cat one.py
def hello( ):
        print(   "Hello World!")
❯ ln one.py two.py  # create a hardlink
❯ cp one.py three.py  # create a copy
❯ ruff format one.py --isolated
1 file reformatted
❯ diff one.py two.py  # the hardlink was also modified
❯ diff one.py three.py  # but the copy is not
1,2c1,2
< def hello():
<     print("Hello World!")
---
> def hello( ):
>         print(   "Hello World!")
```

An alternative approach is to unlink the file, breaking the hardlink, before writing the modified content.

The specific use where this causes issue was when I ran ruff on a conda environment. This resulted in modification of the associated files in the package cache (conda hardlinks files into environments when possible) thus corrupting it.

A similar issue existed in`pip` prior to the 18.0 release, see the discussion in pypa/pip#5405 and conda/conda#6861.

---

_Comment by @zanieb on 2025-02-06 22:04_

Sorry, but why are you formatting the conda environment?

It seems correct for Ruff to respect hardlinks. It sounds like you're expecting copy-on-write semantics which should be implemented at the file system layer.

---

_Comment by @zanieb on 2025-02-06 22:07_

I think this is a bit different than `pip`, which is installing new files into an environment and accordingly should not modify existing files in place.

---

_Comment by @jjhelmus on 2025-02-06 22:12_

> Sorry, but why are you formatting the conda environment?

It was an accident, the environment was in the working directory and the glob was broad :smile:.

> It seems correct for Ruff to respect hardlinks. It sounds like you're expecting copy-on-write semantics which should be implemented at the file system layer.

I can respect this as expect behavior, it just confused me for a bit. I'd have no complaints if you want to close this as a won't fix or similar.

---

_Comment by @jjhelmus on 2025-02-06 22:18_

Thinking about this more, modification in place is absolutely the right behavior. Manually formatting the file with most any editor would do the same thing. Sorry for the noise.

---

_Closed by @jjhelmus on 2025-02-06 22:18_

---

_Comment by @zanieb on 2025-02-06 23:14_

> It was an accident

Ah okay haha that makes more sense.

> Thinking about this more, modification in place is absolutely the right behavior. Manually formatting the file with most any editor would do the same thing. Sorry for the noise.

Thanks! And no problem.

---
