---
number: 9592
title: "Function request: Dependency installation in virtual environment using soft connection"
type: issue
state: closed
author: silent-rain
labels: []
assignees: []
created_at: 2024-12-03T02:16:11Z
updated_at: 2024-12-03T03:00:35Z
url: https://github.com/astral-sh/uv/issues/9592
synced_at: 2026-01-10T01:24:43Z
---

# Function request: Dependency installation in virtual environment using soft connection

---

_Issue opened by @silent-rain on 2024-12-03 02:16_

## Current Status

Although only one copy of the downloaded dependency library is retained globally, a separate dependency is installed in each virtual environment of the project, which takes up a lot of space.

## The problems we are facing

For example, in the "comfyui" drawing, the dependency library volume is expanding day by day, and it still consumes 13GB of memory. In the case of setting up multiple environments, the total volume will be even larger.
So, since there is already a copy of the dependency library globally, can soft links be used in a virtual environment? This can greatly reduce the occupation of space.
For example, in the frontend, "pnpm" also uses a soft connection, so not only does it execute extremely quickly in the "pnpm install" command, but also the "node_modules" volume is extremely small.
So, can we consider it?

---

_Comment by @charliermarsh on 2024-12-03 02:19_

I believe we already have the behavior you're looking for. By default, we use copy-on-write links on macOS and hardlinks on Linux and Windows.

---

_Comment by @silent-rain on 2024-12-03 02:30_

Yes, I just found what I was looking for

Very good



> [tool.uv]
link-mode = "symlink"

---

_Closed by @silent-rain on 2024-12-03 02:31_

---

_Comment by @charliermarsh on 2024-12-03 02:31_

I recommend against using `symlink` specifically. I think the hardlink mode should be better in just about every way.

---

_Comment by @silent-rain on 2024-12-03 02:35_

I have set up multiple "comfyui" environments, each of which takes up a lot of space, so I have to find ways to alleviate this situation

---

_Comment by @charliermarsh on 2024-12-03 02:40_

Hardlinks have the same effect though. The underlying files are the same, without copying.

---

_Comment by @silent-rain on 2024-12-03 03:00_

Thank you very much for your guidance. 
I think I have discovered where the problem lies.


---
