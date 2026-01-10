```yaml
number: 11883
title: Add sizes option to the tree command
type: issue
state: closed
author: pablofueros
labels:
  - enhancement
assignees: []
created_at: 2025-03-01T12:33:02Z
updated_at: 2025-12-03T15:25:35Z
url: https://github.com/astral-sh/uv/issues/11883
synced_at: 2026-01-10T03:23:53Z
```

# Add sizes option to the tree command

---

_Issue opened by @pablofueros on 2025-03-01 12:33_

### Summary

This week I had the idea of creating a cli tool to check the size of the packages used in a python project. After checking available tools to facilitate the work, I noticed that uv tree could be used to get the dependency tree and then look up the sizes of the packages on the venv. As it was a "simple" wrapper around uv, I thought about it and decided to ask for the feature here.


### Example

Note that sizes are not completely accurate:

```bash
~/test/ > uv tree --sizes
81MB    |   test v0.1.0  
39MB    |   ├── numpy v2.2.3
42MB    |   └── pandas v2.2.3 (group: dev)
(**)    |       ├── numpy v2.2.3
0.5MB   |       ├── python-dateutil v2.9.0.post0
8KB     |       │   └── six v1.17.0
1.5MB   |       ├── pytz v2025.1
1.6MB   |       └── tzdata v2025.1
(**) Package size already displayed
```


### Benefits

* In order to share a distribution (e.g. with pyinstaller), this helps us to get an idea of how big the executable will be.
* Check if the dependency you are using really fits the project. There may be a lighter package that does what you need.

---

_Label `enhancement` added by @pablofueros on 2025-03-01 12:33_

---

_Renamed from "Add size option tp the tree command" to "Add size option to the tree command" by @pablofueros on 2025-03-01 12:39_

---

_Renamed from "Add size option to the tree command" to "Add sizes option to the tree command" by @pablofueros on 2025-03-01 12:39_

---

_Comment by @charliermarsh on 2025-03-05 15:29_

This seems reasonable, but we'd only be able to show this for installed packages (and `uv tree` generally shows the whole dependency tree, including packages that aren't installed).

---

_Comment by @zanieb on 2025-03-05 18:36_

(Unless we start tracking sizes in the lockfile?)

---

_Comment by @charliermarsh on 2025-03-05 18:50_

Oh I think we maybe do have them for registry packages? At least the wheel size. So we could just use that, that's much better.

---

_Comment by @pablofueros on 2025-03-05 20:50_

Hello! First thanks for taking the time to answer my concern.

Second, I dont know how the tree command works exactly, but, as @zanieb pointed out, adding some metadata to the lock file each time a package is added to the project could be the way to go. I think that the wheel size may not be as representative as the actual files saved on the venv, I would need to do some testing...

Greetings!

---

_Comment by @charliermarsh on 2025-03-05 20:58_

The problem with the size reported by the registry is that it's the size of the compressed wheel, not the size of the package on-disk.

---

_Comment by @pablofueros on 2025-03-06 10:20_

Sorry, I dont know exactly what is the registry. After some research, I saw that the wheel size isn´t the same as the package on-disk as you said. Could the changes on the venv be tracked and registered in the lock file each time a package is added / removed? I saw that the tree command can run without the venv, but add or remove do actually create the venv. Each time they are called, changes on the venv could be registered, couldn't they?

---

_Comment by @charliermarsh on 2025-12-03 05:49_

(We now have `--show-sizes` which I think is sufficient.)

---

_Closed by @charliermarsh on 2025-12-03 05:49_

---

_Comment by @pablofueros on 2025-12-03 11:58_

Thanks for the update!

I've been trying it out. Note that if there are repeated packages, it could be great not to repeat the sizes (look my idea on the 1st comment). This could also be great to make a sumup of the sizes to get the total size of the project (which was my actual concern)

---

_Comment by @zanieb on 2025-12-03 12:56_

It sounds like you want a JSON output which would make deduplicating and calculating the total size easier. I'm not sure it's worth complicating the display.

---

_Comment by @pablofueros on 2025-12-03 15:25_

Yeah the display isn't the most important here, that was just an idea. But the total size (without the duplicates ofc) would be intersting!

---
