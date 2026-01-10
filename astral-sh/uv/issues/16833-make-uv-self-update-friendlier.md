```yaml
number: 16833
title: "Make \"uv self update\" friendlier"
type: issue
state: closed
author: ichoosetoaccept
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-11-24T14:00:30Z
updated_at: 2025-12-04T18:44:08Z
url: https://github.com/astral-sh/uv/issues/16833
synced_at: 2026-01-10T03:11:35Z
```

# Make "uv self update" friendlier

---

_Issue opened by @ichoosetoaccept on 2025-11-24 14:00_

### Summary

I have uv installed via Homebrew.
When I run `uf self update` the output currently is:
`error: uv was installed through an external package manager, and self-update is not available. Please use your package manager to update uv.`

Makes sense--no surprise there.

What I'm proposing is for uv to be friendlier about this. uv could internally check (say, with `which uv`) how it was installed and then tell the user to use `brew update && brew upgrade uv` in my case.

Naturally this would need to be adapted to all ways uv can be installed and to all possible OSes uv is available on.

I'm on macOS 26.1 and I use
```
uv --version
uv 0.9.11 (Homebrew 2025-11-20)
```

This is squarely in "nitpick territory" and most definitely a minor thing but worth considering 🙂.

Cheers!

### Example

Example, for command
`uv self update`

print
```
You installed uv using Homebrew (https://brew.sh). To update uv, please run
brew update && brew upgrade uv.
```

---

_Label `enhancement` added by @ichoosetoaccept on 2025-11-24 14:00_

---

_Comment by @zanieb on 2025-11-24 14:21_

I think some tools, e.g., tailscale(?), will detect the Homebrew case and actually just run the relevant Homebrew command for you.

I'm not opposed but it won't be a high priority.

---

_Label `help wanted` added by @zanieb on 2025-11-24 14:21_

---

_Closed by @zanieb on 2025-12-04 18:44_

---
