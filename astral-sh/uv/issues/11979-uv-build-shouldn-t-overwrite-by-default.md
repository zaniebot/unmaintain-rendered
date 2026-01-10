---
number: 11979
title: "uv build shouldn't overwrite by default"
type: issue
state: closed
author: pycaw
labels:
  - question
  - needs-decision
assignees: []
created_at: 2025-03-05T15:11:03Z
updated_at: 2025-03-18T22:22:46Z
url: https://github.com/astral-sh/uv/issues/11979
synced_at: 2026-01-10T01:25:13Z
---

# uv build shouldn't overwrite by default

---

_Issue opened by @pycaw on 2025-03-05 15:11_

In my opinion uv build shouldn't overwrite by default. Let's say I have distributed by build output. Then I forget to bump the package version and I overwrite the last build output of mine. I assume just as repositories do not overwrite packages by default (what is more, aren't really even configured to overwrite any time), files under /dist shouldn't be either by `uv build`. Maybe if an additional flag is present.

---

_Comment by @zanieb on 2025-03-05 18:50_

I don't think I agree. Why do you need the previous build in this context? I think this is a case where most people would want it to build and overwrite and we'd need a strong motivation to add friction to the experience by requiring opt-in.

---

_Label `question` added by @zanieb on 2025-03-05 18:50_

---

_Label `needs-decision` added by @zanieb on 2025-03-05 18:50_

---

_Comment by @pycaw on 2025-03-05 20:06_

I can't imagine silently overwriting existing packaged distributions is a clearly better default  than, on the other hand, to stop for a prompt or just straight up refuse to do so. Sometimes it could be mistakes that lead to this behavior and therefore aborting this would be warranted so the user can react before worsening their situation; or, potential data loss (not impossible). Passing a -f/--force/--force-output flag could convey clear intent. At the least a message should be issued in case of overwriting.

---

_Comment by @zanieb on 2025-03-05 20:20_

I don't think you answered my question there.

cc @konstin if you have an opinion

---

_Comment by @pycaw on 2025-03-05 21:48_

> I don't think you answered my question there.
> 
> cc [@konstin](https://github.com/konstin) if you have an opinion

Even if I never need the previous build, I'd always want to know if the new build I produced maybe overwrites one I had already built (and distributed).

Then, I might need the previous build because... I want to do something with an older build, this can be a sizeable list of things.


---

_Comment by @charliermarsh on 2025-03-15 01:01_

I think this is the right behavior, personally. It matches the other tools I can think of: `python -m build`, `cargo build`, etc.

---

_Closed by @charliermarsh on 2025-03-15 01:01_

---

_Comment by @pycaw on 2025-03-17 11:13_

[EDIT: code improved]

I understand there are different preferences, and that this became some kind of default and one must adjust. In any case, if someone wants this behavior, this is how I achieved it (using hatchling backend):

```python
import re
from pathlib import Path

from hatchling.builders.hooks.plugin.interface import BuildHookInterface


class SpecialBuildHook(BuildHookInterface):
    def initialize(self, version, build_data):
        pat = rf".*{re.escape(self.metadata.version)}(\.-py3-.*|\.tar\.gz)"
        if not (
            files := [
                m.group() for m in [re.match(pat, p.name) for p in (Path(self.directory).glob("*"))] if m
            ]
        ):
            return
        for f in files:
            self.app.display_error(f"Found file '{f}' with same version")
        self.app.display_warning(
            "Removing existing files in the build output directory or "
            "set HATCH_BUILD_NO_HOOKS=1 to disable hooks."
        )
        self.app.abort()
```

---

_Comment by @charliermarsh on 2025-03-17 12:33_

Thank you for following up, that's great.

---
