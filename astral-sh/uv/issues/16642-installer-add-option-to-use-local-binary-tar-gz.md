```yaml
number: 16642
title: "Installer: add option to use local binary .tar.gz"
type: issue
state: open
author: mariomadproductions
labels:
  - enhancement
  - question
assignees: []
created_at: 2025-11-08T13:56:42Z
updated_at: 2025-11-09T12:46:35Z
url: https://github.com/astral-sh/uv/issues/16642
synced_at: 2026-01-12T16:02:35Z
```

# Installer: add option to use local binary .tar.gz

---

_@mariomadproductions_

### Summary

Rather than requiring it be retrieved via HTTP

https://docs.astral.sh/uv/reference/installer/

### Example

_No response_

---

_Label `enhancement` added by @mariomadproductions on 2025-11-08 13:56_

---

_Comment by @zanieb on 2025-11-08 15:08_

Please share more concretely what you're looking for, we already support installing local targets.

---

_Label `question` added by @zanieb on 2025-11-08 15:08_

---

_Comment by @zanieb on 2025-11-08 15:10_

e.g., https://docs.astral.sh/uv/concepts/projects/dependencies/#path or in `uv pip install`

---

_Comment by @mariomadproductions on 2025-11-08 18:18_

> Please share more concretely what you're looking for, we already support installing local targets.

Ah sorry, I mean the installer script for uv itself: https://docs.astral.sh/uv/reference/installer/

---

_Comment by @RafaelJohn9 on 2025-11-09 12:09_

Hey @mariomadproductions , I think by  `add option to use local binary .tar.gz` you mean the uv bin file ?

if that's the case I'm sure you can achieve that by : 

1. **Go to** [https://github.com/astral-sh/uv/releases/latest](https://github.com/astral-sh/uv/releases/latest)  
2. **Download** the appropriate `.tar.gz` asset for your platform (e.g., `uv-<version>-x86_64-unknown-linux-gnu.tar.gz`)  
3. **Extract and install** it manually:

```bash
tar -xzf uv-*.tar.gz
sudo mv uv/uv /usr/local/bin/  # or to ~/.local/bin if preferred
```
4. **Ensure the install location is in your `PATH`**, then verify:

```bash
uv --version
```

---

_Comment by @mariomadproductions on 2025-11-09 12:29_

> Hey [@mariomadproductions](https://github.com/mariomadproductions) , I think by `add option to use local binary .tar.gz` you mean the uv bin file ?
> 
> if that's the case I'm sure you can achieve that by :
> 
> 1. **Go to** https://github.com/astral-sh/uv/releases/latest
> 2. **Download** the appropriate `.tar.gz` asset for your platform (e.g., `uv-<version>-x86_64-unknown-linux-gnu.tar.gz`)
> 3. **Extract and install** it manually:
> 
> tar -xzf uv-*.tar.gz
> sudo mv uv/uv /usr/local/bin/  # or to ~/.local/bin if preferred
> 4. **Ensure the install location is in your `PATH`**, then verify:
> 
> uv --version

Yeah that's probably enough, based on comments in the installer script, but I'd prefer to just use the installer as that's the documented install method.

---

_Comment by @RafaelJohn9 on 2025-11-09 12:40_


@mariomadproductions ,  so you are looking for a local binary installer script  for uv ?

---

_Comment by @mariomadproductions on 2025-11-09 12:46_

> [@mariomadproductions](https://github.com/mariomadproductions) , so you are looking for a local binary installer script for uv ?

Basically I just another flag for the existing script, that lets you choose local files, rather than having to host them on HTTP and point it to that URL.

---
