```yaml
number: 4532
title: Assignment from none
type: pull_request
state: closed
author: ghost
labels: []
assignees: []
base: main
head: assignment-from-none
created_at: 2023-05-19T16:33:57Z
updated_at: 2023-06-05T22:08:50Z
url: https://github.com/astral-sh/ruff/pull/4532
synced_at: 2026-01-12T15:55:15Z
```

# Assignment from none

---

_@ghost_

Here is a draft of implementing PLE1128. I have a skeleton implementation and need assistance finishing it using Visitor or something similar. Looking for feedback, and suggestions on how to finish this off. 

---

_Comment by @charliermarsh on 2023-05-19 16:39_

Thank you for this!

Looking at the rule... I don't think we have the compiler infrastructure necessary to support implementing this rule reliably (yet). We could _maybe_ support it in some limited cases (e.g., calling a function from within the file that defines it), but we'd have a lot of false negatives, since we don't yet resolve references across files.

Could I help you pick out a different Pylint rule in lieu of this one?


---

_Comment by @ghost on 2023-05-19 16:42_

Ah, cross-file support is not something I had considered just yet... what an interesting problem, I will spend some time considering how to do that once I better understand how Ruff works. 

I would absolutely be open to suggestions! I'm from Dish Network and we use ruff across all our Python automation. I'm working with Dish internal teams to set up a process for more Dish devs to contribute to FOSS projects like ruff, so expect some more from us in the near future!

---

_Comment by @charliermarsh on 2023-05-19 16:58_

That's amazing to hear! Thanks so much for getting involved.

A good chunk of the outstanding Pylint rules can't be implemented yet without improving some of our internal infrastructure, but from scanning the list, here are a few that are doable: `W1507`, `W0124`.

---

_Comment by @charliermarsh on 2023-05-19 17:13_

(Edit: `R0206` is already implemented, just not reflected in the issue. Removed it from my suggestion.)

---

_Comment by @tusharsadhwani on 2023-05-19 17:27_

@charliermarsh Is there an open issue/discussion about the said infrastructure? If there are knowledge gaps or decisions that require input, I'd like to help.

---

_Comment by @charliermarsh on 2023-05-20 15:25_

@tusharsadhwani - There isn't, it's not yet a priority, but I'll keep this in mind as we start to think through it.

---

_Comment by @charliermarsh on 2023-05-20 15:25_

(Going to close for now as I don't think we're able to implement this with our current infra.)

---

_Closed by @charliermarsh on 2023-05-20 15:25_

---

_Comment by @ghost on 2023-06-05 20:45_

@charliermarsh I believe I have an idea for cross-file support, I don't quite know where I would begin tho:

What are your thoughts on having a lookup table that stores all declarations from all files in a given directory/workspace, (exact logic needs to be fleshed out). So at runtime, Ruff would first index all python files and store the filename and line number of any declaration, (class, function, etc). When Ruff encounters one of these objects, it would use the lookup table to find the declaration of that object, and proceed from there.

---

_Comment by @tusharsadhwani on 2023-06-05 21:12_

@wiwa5606 as it turns out it's actually not that straightforward.

Even if you had a way to reliably detect every import as a relative import (as opposed to one coming from the built-in modules, 3rd party libraries or a modified sys.path), you still have the problem of having multiple definitions of the thing in the same file, figuring out whether only one of them is reachable, sometimes multiple declarations can be under a `sys.version` check to support multiple python versions, the thing you're looking for could be nested inside another scope (like a class), ...

Building an import graph and finding the declaration for the said name or attribute are both really complex problems.

---

_Comment by @charliermarsh on 2023-06-05 21:17_

I hope to end up building support for all of those things, but it will admittedly take a lot of time and work. The core ideas are likely correct though: learn to resolve imports (including non-first party imports), do a pass over every file to build its symbol table, and lookup in the module symbol table as you analyze bindings.

---

_Comment by @ghost on 2023-06-05 22:05_

Right, 3rd party imports and sys.path manipulation. There's always another thing...

It almost sounds like Ruff needs its own interpreter to do that resolution, I don't know if that will be too slow though. I'm obviously lacking a solid understanding of how Ruff does what it does today, so I don't quite know the specifics. I'm more than willing to pitch in if that's something that is being considered.

---
