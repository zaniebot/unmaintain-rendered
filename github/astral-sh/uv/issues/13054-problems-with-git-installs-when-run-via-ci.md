---
number: 13054
title: Problems with git installs when run via CI
type: issue
state: closed
author: drcongo
labels:
  - question
assignees: []
created_at: 2025-04-22T13:18:42Z
updated_at: 2025-07-25T13:03:32Z
url: https://github.com/astral-sh/uv/issues/13054
synced_at: 2026-01-07T13:12:18-06:00
---

# Problems with git installs when run via CI

---

_Issue opened by @drcongo on 2025-04-22 13:18_

### Question

Hello. Sorry this is such a vague question, it would definitely have been better placed in `discussions` rather than `issues` but we don't seem to have discussions on this repo. Anyway, I have a project with a git post-receive hook on a remote server run via CI, which runs `uv sync --no-dev` and everything is fine until my `uv.lock` contains a git install, which then errors out the whole install process with this...

```
remote: Resolved 120 packages in 5ms        
remote:    Updating https://github.com/someorg/somedep.git (ee67c93fa95ffb7379be1ad4730d6c7504baf581)        
remote:   × Failed to download and build `somedep @        
remote:   │ git+https://github.com/someorg/somedep.git@ee67c93fa95ffb7379be1ad4730d6c7504baf581`
remote:   ├─▶ Git operation failed        
remote:   ╰─▶ process didn't exit successfully: `/usr/bin/git rev-parse` (exit status:        
remote:       128)        
remote:       --- stderr        
remote:       fatal: not a git repository: '.'        
remote: 
remote:   help: `somedep` was included because `myproject` (v0.1.0) depends on        
remote:         `somedep`    
```

The weirdest thing about this though is that if I ssh into the server, and then run `sh /path/to/my/post-receive` as the same user that CI runs as, everything works fine.

### Platform

Ubuntu 24.04

### Version

Any (but on 0.6.9 in this case)

---

_Label `question` added by @drcongo on 2025-04-22 13:18_

---

_Comment by @konstin on 2025-04-22 16:08_

Does clearing the cache with `uv cache clean` help?

---

_Comment by @emdneto on 2025-07-24 22:49_

Facing a similar issue in opentelemetry-python https://github.com/open-telemetry/opentelemetry-python/actions/runs/16509062023/job/46686986557?pr=4692 

---

_Comment by @zanieb on 2025-07-24 22:54_

@emdneto that looks pretty different?

```
├─▶ failed to find branch, tag, or commit
  │   `0f166e3136033f3230a692f82381fb66f39d417f`
  ╰─▶ process didn't exit successfully: `/usr/bin/git rev-parse
      '0f166e3136033f3[23](https://github.com/open-telemetry/opentelemetry-python/actions/runs/16509062023/job/46686986557?pr=4692#step:5:24)0a692f82381fb66f39d417f^0'` (exit status: 128)
      --- stdout
      0f166e3136033f3230a692f82381fb66f39d417f^0

      --- stderr
      fatal: ambiguous argument '0f166e3136033f3230a692f82381fb66f39d417f^0':
      unknown revision or path not in the working tree.
      Use '--' to separate paths from revisions, like this:
      'git <command> [<revision>...] -- [<file>...]'
```

I think you should open a new issue.

---

_Comment by @emdneto on 2025-07-25 12:54_

@zanieb opened https://github.com/astral-sh/uv/issues/14894 

---

_Comment by @zanieb on 2025-07-25 13:03_

(We never got a reply here so I'm going to close this out)

---

_Closed by @zanieb on 2025-07-25 13:03_

---

_Referenced in [astral-sh/uv#16102](../../astral-sh/uv/issues/16102.md) on 2025-10-02 11:23_

---
