```yaml
number: 869
title: "`file_to_module` will misbehave on \"real modules\""
type: issue
state: closed
author: Gankra
labels:
  - bug
  - imports
assignees: []
created_at: 2025-07-22T12:49:04Z
updated_at: 2025-09-25T01:15:36Z
url: https://github.com/astral-sh/ty/issues/869
synced_at: 2026-01-12T15:54:24Z
```

# `file_to_module` will misbehave on "real modules"

---

_@Gankra_

In https://github.com/astral-sh/ruff/pull/19471 I am introducing the notion of a "real module" which is found by running resolution with stubs disallowed to provide more useful goto-definition results. A consequence of this is that it violates an invariant that file_to_module enforces: files and modules should be a bijection. Specifically if you pass a "real module" to file_to_module it will notice it's not the stub module and return None.

This... *might* cause problems? I haven't seen any issue yet. @MichaReiser suggested also allowing it to return Some(module) if it's found with `resolve_real_module`.

---

_Label `bug` added by @Gankra on 2025-07-22 12:49_

---

_Label `imports` added by @Gankra on 2025-07-22 12:49_

---

_Comment by @MichaReiser on 2025-07-22 12:56_

> This... might cause problems? I haven't seen any issue yet. @MichaReiser suggested also allowing it to return Some(module) if it's found with resolve_real_module.

We might be "lucky" in editors because we use a different ty instance for non-project files. This might change in the future. Or I'm wrong :)

Related to https://github.com/astral-sh/ty/issues/839

---

_Comment by @carljm on 2025-07-22 16:00_

I think `file_to_module` returning `None` in this case is the right and desired behavior, and not something we need to fix? Or at least, that would be my starting assumption until we see a concrete reason it's a problem.

The contract is that `file_to_module` will give you back the module name under which this file is importable, and for a real-module shadowed by a stub, that file is not importable under any name (using the default import rules that prioritize stubs), so `None` is the right result.

If we do run into the need for a `file_to_module` that works for this case, it seems like the right answer would be a separate `file_to_real_module` that implements the same bijection, but for the import configuration that ignores stubs.

---

_Comment by @MichaReiser on 2025-07-22 16:13_

> The contract is that file_to_module will give you back the module name under which this file is importable, and for a real-module shadowed by a stub, that file is not importable under any name (using the default import rules that prioritize stubs), so None is the right result.

The issue with this is that we use `file_to_module` when resolving relative imports. That means, relative imports won't work for any file where `file_to_module` returns `None`. Which is unfortunate in the LSP because you can't follow relative imports and they're all typed as `Unknown`. 



---

_Comment by @carljm on 2025-07-22 16:50_

What behavior do we expect from following relative imports from a "real module" that isn't part of the project? Should it follow the import as part of the project (i.e. go back to stubs) or should it follow the import to another "real module"? The answer to this would make a big difference as to how we want to adjust things here. What do other LSPs do in this scenario?

---

_Comment by @carljm on 2025-07-22 16:58_

If we want it to follow the import "locally" to another "real module", then I suspect the best option will just be to implement fallback best-effort resolution of relative imports that doesn't require correct configuration of the import path -- this will help both this case and the more general case of misconfigured LSP users.

But that behavior might be inconsistent, because I expect absolute imports from "real modules" will resolve back to stubs, not to real modules.

---

_Comment by @MichaReiser on 2025-07-22 17:43_

> What behavior do we expect from following relative imports from a "real module" that isn't part of the project? Should it follow the import as part of the project (i.e. go back to stubs) or should it follow the import to another "real module"? The answer to this would make a big difference as to how we want to adjust things here. What do other LSPs do in this scenario?

Both go to definition and go to declaration would work the same as when you click on the import in a first party file. go to definition goes to the real module, go to declaration goes to the stub

---

_Comment by @carljm on 2025-07-22 17:52_

I assume that implies that type inference should go to the stub?

---

_Comment by @MichaReiser on 2025-07-22 17:57_

> I assume that implies that type inference should go to the stub?

Yes

---

_Closed by @Gankra on 2025-09-25 01:15_

---
