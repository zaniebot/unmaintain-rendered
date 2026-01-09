---
number: 14442
title: "D100 with `--add-noqa` breaks shebang lines"
type: issue
state: open
author: pjspereira
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-11-18T20:01:17Z
updated_at: 2025-04-24T13:31:39Z
url: https://github.com/astral-sh/ruff/issues/14442
synced_at: 2026-01-07T13:12:16-06:00
---

# D100 with `--add-noqa` breaks shebang lines

---

_Issue opened by @pjspereira on 2024-11-18 20:01_

The `—add-noqa` breaks shebang lines with the D100 rule on files with no module documentation. Can the shebang line be ignored and the D100 error be assigned to the second line, if it exists?

Example output from `ruff check --add-noqa --select D100 --isolated .`:

```python
#!/usr/bin/env python  # noqa: D100

import sys

if __name__==‘__main__’:
    print(sys.argv)
```

If this script is marked executable, the noqa comment will be passed as a filename to python, which will crash the interpreter, since the file does not exist.

Keywords: shebang D100
Ruff version: 0.6.4


---

_Comment by @MichaReiser on 2024-11-18 21:15_

That sounds reasonable. We need to change the diagnostic location to after the first line

---

_Label `bug` added by @MichaReiser on 2024-11-18 21:15_

---

_Label `help wanted` added by @MichaReiser on 2024-11-18 21:15_

---

_Comment by @InSyncWithFoo on 2024-11-20 06:10_

[`CPY001`](https://docs.astral.sh/ruff/rules/missing-copyright-notice/), [`INP001`](https://docs.astral.sh/ruff/rules/implicit-namespace-package/) and other file-level rules seem to suffer from the same problem.

As a FYI, changing the location would be a breaking change for RyeCharm, which currently [uses the value of the range](https://github.com/InSyncWithFoo/ryecharm/blob/1f64d40167968db7beb4491de31b2333fdce82a3/src/main/kotlin/insyncwithfoo/ryecharm/ruff/linting/RuffAnnotator.kt#L59-L60) to determine whether it should show [an editor-level notice banner](https://insyncwithfoo.github.io/ryecharm/configurations/ruff/#show-editor-banner-for-file-level-diagnostics). Updating this logic is simple enough, though.

---

_Comment by @MichaReiser on 2024-11-20 06:59_

Yeah, it's certainly not ideal and thanks for the heads up for the RyeCharm integration. 

We could consider detecting this in the fix loop but my worry is that it won't be possible to determine if the edit should be applied before or after the shebang line (e.g. what if an edit changes the shebang line). But maybe we can? By only applying said logic if its an insertion at offset 0 and a shebang line is present?

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-10 13:44_

---

_Renamed from "D100 with —add-noqa breaks shebang lines" to "D100 with -`-add-noqa` breaks shebang lines" by @charliermarsh on 2025-01-10 14:22_

---

_Renamed from "D100 with -`-add-noqa` breaks shebang lines" to "D100 with `--add-noqa` breaks shebang lines" by @charliermarsh on 2025-01-10 14:22_

---

_Comment by @MusicalNinjaDad on 2025-04-24 07:14_

Does that mean that all file-level rules should:
- expect their file-level `# noqa:` to be directly below the shebang, or would the convention be to add a blank line in-between?
- apply their `--fix`es to the correct place?

I imagine this would be best done in a central location - e.g. where the noqa directives / fixes are parsed? using something like the EXE rules do: `if let Some(shebang) = ShebangDirective::try_extract(comment)`



---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-04-24 07:15_

---

_Comment by @MichaReiser on 2025-04-24 07:23_

I'm not quiet sure what the right fix is. There's no clear good solution. The problem is that our noqa system expects that the suppression comments are on the same line as the diagnostic's range. 

We could explore if the following works:

* In `add_noqa`: If we detect that we're on the first line and the first line is a shebang, add the suppression right after the shebang line
* When finding suppression comments and the diagnostic range starts at `0:0` and there is a shebang, respect suppressions on the next line.

---

_Comment by @MusicalNinjaDad on 2025-04-24 13:23_

I'd be happy to take a look at this, if it's not urgent and you're ok with that. Although I don't want to step on @AlexWaygood 's toes...

---

_Comment by @MichaReiser on 2025-04-24 13:29_

Feel free to take a look. I don't think @AlexWaygood is working on this. Just a heads up. We're currently very busy with the type checker and reviews could be somewhat delayed because of that.

---

_Comment by @AlexWaygood on 2025-04-24 13:31_

I think this is one of the issues I assigned myself to at the beginning of the year because it was listed as a possible starter task for @ntBre (I didn't want anybody else taking it in case it was something he was interested in working on 😄). Both @ntBre and I are now somewhat busy with other tasks, so anybody who wants to look at this should feel free to do so! I unassigned myself from this issue earlier today.

---
