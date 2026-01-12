```yaml
number: 17039
title: "[docs] Fix file permission issues when running Ruff through Docker"
type: pull_request
state: open
author: nickjj
labels: []
assignees: []
base: main
head: nickjj/update-docker-docs
created_at: 2025-03-28T15:51:46Z
updated_at: 2025-03-28T16:05:01Z
url: https://github.com/astral-sh/ruff/pull/17039
synced_at: 2026-01-12T15:56:00Z
```

# [docs] Fix file permission issues when running Ruff through Docker

---

_@nickjj_

## Summary

The current documentation at https://docs.astral.sh/ruff/installation/ suggests running:

``` sh
docker run -v .:/io --rm ghcr.io/astral-sh/ruff:0.3.0 check
```

There are 2 issues here:

- If Ruff modifies a file, the file will be written to the Docker host as `root:root` causing all sorts of permission issues, such as not being able to save the file anymore in your normal code editor
    - If using Docker Desktop, there are mechanisms to make it not be root but on native Linux (including WSL 2) it will be root
- The docs reference `0.3.0` which is a bit old
    - I used the most recent release but maybe a long term plan in another PR (above my paygrade) would be to auto-generate this when cutting a new release so it's always using the latest release?

This PR addresses both issues:

- For the first one, I added `-u "$(id -u):$(id -g)"` which will use the current user's UID / GID to ensure files get written back to the host as the same user who ran the Docker command
    - I have been using this locally for days now, it works on both native Linux without Docker Desktop as well as Docker Desktop on macOS, I did not directly test other machines but I imagine it will be ok with Docker Desktop on Windows too as long as you run the command from WSL 2
        - I do not use SELinux or Podman so I'm not sure if it will work there
- For the second one, I found the latest release and referenced it

## Test Plan

I didn't run any tests, I found the documentation file and patched it.

---

_Renamed from "Fix file permission issues when running Ruff through Docker" to "[docs] Fix file permission issues when running Ruff through Docker" by @nickjj on 2025-03-28 15:55_

---

_Comment by @MichaReiser on 2025-03-28 15:56_

The explanation makes sense to me, but I strongly suspect that the command won't work on Windows. 

The writing back should also only be a problem if you use `--fix`. 

But improving this would definitely be nice

---

_Comment by @nickjj on 2025-03-28 16:03_

> but I strongly suspect that the command won't work on Windows.

Yep, it will not work with PowerShell but it will within WSL 2. Do you have any suggestions on how you want to handle it?

Here's a couple of potential paths off the top of my head:

1. Keep it as is
2. Keep it as is but add a 2nd command for Windows users with PowerShell which doesn't have `-u` set
3. Keep it as is but add a comment or paragraph to remove those flags when using PowerShell
4. Remove the flag but add a comment or paragraph for fixing file permissions on native Linux / WSL 2 without Docker Desktop

Options 1 or 2 seem desirable for developer experience so all folks have a copy / paste'able command.

Considerations:

- Who is your target audience? For example, how many folks are using native Linux vs Docker Desktop on macOS vs Docker Desktop on Windows with WSL 2 vs Docker Desktop on Windows with PowerShell
    - Optimizing for the most folks without needing to modify the command to copy / paste seems ideal?

---
