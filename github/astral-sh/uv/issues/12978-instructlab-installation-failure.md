---
number: 12978
title: InstructLab Installation Failure
type: issue
state: closed
author: rauldsl
labels: []
assignees: []
created_at: 2025-04-19T06:18:16Z
updated_at: 2025-04-29T01:49:24Z
url: https://github.com/astral-sh/uv/issues/12978
synced_at: 2026-01-07T13:12:18-06:00
---

# InstructLab Installation Failure

---

_Issue opened by @rauldsl on 2025-04-19 06:18_

I'm trying to install InstructLab locally on my Mac M1. 

**Reference**
https://docs.instructlab.ai/getting-started/mac_metal/


**When It Happens**

I'm getting the following error message when I run:

$ pip install 'instructlab[mps]'

>> 
ERROR: Cannot install instructlab[mps]==0.15.1, instructlab[mps]==0.16.0, instructlab[mps]==0.16.1, instructlab[mps]==0.17.0, instructlab[mps]==0.17.1 and instructlab[mps]==0.17.2 because these package versions have conflicting dependencies.

The conflict is caused by:
    instructlab[mps] 0.17.2 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"
    instructlab[mps] 0.17.1 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"
    instructlab[mps] 0.17.0 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"
    instructlab[mps] 0.16.1 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"
    instructlab[mps] 0.16.0 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"
    instructlab[mps] 0.15.1 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"


---

_Comment by @NMertsch on 2025-04-20 07:31_

How is this issue related to uv?

---

_Comment by @missu on 2025-04-29 01:32_

I have a similar error. I'm trying to install it on an M2 Mac Mini.

```
INFO: pip is looking at multiple versions of instructlab to determine which version is compatible with other requirements. This could take a while.
Collecting instructlab
  Using cached instructlab-0.17.1-py3-none-any.whl.metadata (31 kB)
  Using cached instructlab-0.17.0-py3-none-any.whl.metadata (31 kB)
  Using cached instructlab-0.16.1-py3-none-any.whl.metadata (30 kB)
  Using cached instructlab-0.16.0-py3-none-any.whl.metadata (30 kB)
  Using cached instructlab-0.15.1-py3-none-any.whl.metadata (30 kB)
ERROR: Cannot install instructlab==0.15.1, instructlab==0.16.0, instructlab==0.16.1, instructlab==0.17.0, instructlab==0.17.1 and instructlab==0.17.2 because these package versions have conflicting dependencies.

The conflict is caused by:
    instructlab 0.17.2 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"
    instructlab 0.17.1 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"
    instructlab 0.17.0 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"
    instructlab 0.16.1 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"
    instructlab 0.16.0 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"
    instructlab 0.15.1 depends on mlx<0.6.0 and >=0.5.1; sys_platform == "darwin" and platform_machine == "arm64"

To fix this you could try to:
1. loosen the range of package versions you've specified
2. remove package versions to allow pip to attempt to solve the dependency conflict

ERROR: ResolutionImpossible: for help visit https://pip.pypa.io/en/latest/topics/dependency-resolution/#dealing-with-dependency-conflicts
```
I am currently running Python3.13.3.
The first time I tried installing instructlab, I had success. Then I ran the `ilab` command. This is when I saw a warning about no longer supporting python3.9. I was running python3.9. So I deleted everything, upgraded to python13.3.3, and tried again. This is when I encountered the error listed above.


---

_Comment by @charliermarsh on 2025-04-29 01:43_

These are `pip` errors.

---

_Closed by @charliermarsh on 2025-04-29 01:43_

---

_Comment by @rauldsl on 2025-04-29 01:49_

Hey @NMertsch you're right, Nothing to deal with UV. It seems like a pip errors. 
And even trying to specify or remove the conflicting Python version isn't working. However, this is something to deal with in the Project InstructLab. Some of the packages the MLX packages don't exist. 

---
