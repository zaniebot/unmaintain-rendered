```yaml
number: 11944
title: "Add confirmation after running `uv add some_package`"
type: issue
state: open
author: ifsheldon
labels:
  - enhancement
assignees: []
created_at: 2025-03-04T09:42:02Z
updated_at: 2025-03-05T21:11:21Z
url: https://github.com/astral-sh/uv/issues/11944
synced_at: 2026-01-12T16:00:50Z
```

# Add confirmation after running `uv add some_package`

---

_@ifsheldon_

### Summary

Can you please add a confirmation after `uv add some_package` before actually downloading and adding it? Some Python packages (like PyTorch) have a lot of dependencies and large size, so I think it'd be nice to ask for confirmation after `uv add some_package`, presenting what packages will be added and the estimated total disk usage. This confirmation can also avoid typo mistakes.

This behavior is pretty much the same as `conda install` or `mamba install`.

### Example

_No response_

---

_Label `enhancement` added by @ifsheldon on 2025-03-04 09:42_

---

_Comment by @notatallshaw on 2025-03-04 17:22_

The problem with this is that uv, or any tool that installs Python packages, can't guarantee that packages won't be downloaded during resolution. 

If the index doesn't support metadata files or http range requests the package must be downloaded to extract the metadata. 

Conda based tools get around this by having a json file (or multiple json files) that represent all dependencies for all packages. This has its own tradeoffs, as you need to download the entire universe of dependencies (sometimes many hundreds of MBs) to install a small package with no dependencies. 

---

_Comment by @zanieb on 2025-03-04 17:43_

I'm loosely into the idea of adding opt-in confirm behavior to various commands, i.e., I like this in `npx` and it would be useful in  `uvx`

---

_Comment by @karanravindra on 2025-03-04 22:33_

> presenting what packages will be added and the estimated total disk usage

I actually think that's a pretty cool idea.

In terms of a confirmation after using `uv add xyz`, I have found (on Mac and Linux) that simply pressing <CTRL+C> will not only stop the download, but also removes any cache.

ps: pytorch offers a no-gpu install version which is WAYYYY smaller
https://download.pytorch.org/libtorch/cpu/libtorch-macos-arm64-2.6.0.zip

---

_Comment by @ifsheldon on 2025-03-05 08:39_

> The problem with this is that uv, or any tool that installs Python packages, can't guarantee that packages won't be downloaded during resolution.

OK, got it. So I guess there's not a way that definitely works for any packages to get the size and its dependencies of the package without downloading it. This mechanism seems kind of flawed, but anyway it's not uv's fault.

> In terms of a confirmation after using uv add xyz, I have found (on Mac and Linux) that simply pressing <CTRL+C> will not only stop the download, but also removes any cache.

This is neat. So, perhaps one workaround is that `uv add xyz` will actually download the packages _before_ asking confirmation and if the user cancel it in the confirmation, it just cleans the related cache.

As far as I understand, there are two parts:
1. Adding confirmation: This should be super straightforward and easy to add
2. List all dependencies and estimate their total size: This is a bit hard (to my surprise).

---

_Comment by @karanravindra on 2025-03-05 21:11_

> As far as I understand, there are two parts:
> 
> Adding confirmation: This should be super straightforward and easy to add
> List all dependencies and estimate their total size: This is a bit hard (to my surprise).

definitely seems possible though I would also suggest that this feature can be toggle able with and Environment Variable. I imagine there would be a large portion of users who wouldn't use this, for example if they have fast internet.

---
