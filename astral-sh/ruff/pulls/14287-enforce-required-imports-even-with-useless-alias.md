```yaml
number: 14287
title: enforce required imports even with useless alias
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: isort-alias
created_at: 2024-11-11T22:29:56Z
updated_at: 2024-11-17T14:28:07Z
url: https://github.com/astral-sh/ruff/pull/14287
synced_at: 2026-01-10T20:50:57Z
```

# enforce required imports even with useless alias

---

_Pull request opened by @dylwil3 on 2024-11-11 22:29_

This PR handles a panic that occurs when applying unsafe fixes if a user inserts a required import (I002) that has a "useless alias" in it, like `import numpy as numpy`, and also selects PLC0414 (useless-import-alias)

In this case, the fixes alternate between adding the required import statement, then removing the alias, until the recursion limit is reached. See linked issue for an example.

# Tests to confirm panic resolved:

```console
$ echo 1 | cargo run --quiet -p ruff -- check --no-cache --isolated --select I002,PLC0414 --config 'lint.isort.required-imports = ["from module import this as this"]' - --unsafe-fixes --fix
from module import this as this
1
Found 1 error (1 fixed, 0 remaining).
```

```console
$ echo 1 | cargo run --quiet -p ruff -- check --no-cache --isolated --select I002,PLC0414 --config 'lint.isort.required-imports = ["import numpy as numpy"]' - --unsafe-fixes --fix
import numpy as numpy
1
Found 1 error (1 fixed, 0 remaining).
```

Closes #14283


---

_Comment by @dylwil3 on 2024-11-11 22:30_

Not so happy about the big diff in such a simple rule for an edge case... I welcome alternative suggestions!

---

_Comment by @github-actions[bot] on 2024-11-11 22:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-11-12 08:28_

Could you expand the PR summary with details about what was causing the infinite loop or panic? It would help me review this PR and possibly suggest alternatives.

---

_Comment by @dylwil3 on 2024-11-12 12:23_

Done! It also occurred to me that it might be better to keep the diagnostic but elide the fix for useless-import-alias. That would serve the purpose of warning the user that something was amiss, and they could override the warning by ignoring the rule.

Edit: Made this change. I did not change the "Fix availability" or documentation because it seems like this is a very rare (nonexistent?) exception, but let me know what you think.

---

_@charliermarsh reviewed on 2024-11-12 21:01_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:100 on 2024-11-12 21:01_

Why this part?

---

_@dylwil3 reviewed on 2024-11-12 22:13_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:100 on 2024-11-12 22:13_

I don't know... This seems to have been there since the original implementation: https://github.com/harupy/ruff/blob/d765fbe5ddca1fa7b735256ebb303799a2e7aa95/src/pylint/plugins.rs#L16

I don't think it's possible to have an alias with a dot in it... maybe it's supposed to be an early return? Is checking for a dot faster than comparing two strings? Certainly can't be that much of a speed up... Anyway, happy to remove it.

---

_@charliermarsh reviewed on 2024-11-13 03:41_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:100 on 2024-11-13 03:41_

Thanks. Do you mind removing it (here and in the linked location) in a separate PR?

---

_Comment by @charliermarsh on 2024-11-13 03:43_

Do you mind adding a dedicated test for this? Sorry!

---

_Label `bug` added by @MichaReiser on 2024-11-13 08:52_

---

_Label `fixes` added by @MichaReiser on 2024-11-13 08:52_

---

_@MichaReiser reviewed on 2024-11-13 08:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:61 on 2024-11-13 08:57_

I don't think our testing framework will accept that an always fixable fix isn't always fixable, which makes sense. 

---

_@MichaReiser reviewed on 2024-11-13 08:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:100 on 2024-11-13 08:58_

Would it make sense to implement helper methods on the `isort` settings to test if it is a required import to avoid duplicating the logic?

---

_@dylwil3 reviewed on 2024-11-14 05:45_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:100 on 2024-11-14 05:45_

Ok! Refactored to use new helper methods on `isort` settings, and I will follow up after this PR is merged to get rid of the check for "."

---

_Comment by @dylwil3 on 2024-11-14 05:45_

> Do you mind adding a dedicated test for this? Sorry!

Added!

---

_@dylwil3 reviewed on 2024-11-14 05:52_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:61 on 2024-11-14 05:52_

Of course you're right! Changed to "Sometimes" with special message/fix title emitted in case of conflict.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:95 on 2024-11-14 06:47_

Nit: To reduce the amount of repeated code

```suggestion
		let mut diagnostic = Diagnostic::new(
          UselessImportAlias {
              required_import_conflict: true,
          },
          alias.range(),
      );
        
    if !checker
        .settings
        .isort
        .requires_module_import(alias.name.to_string(), Some(asname.to_string()))
    {
        
        diagnostic.set_fix(Fix::unsafe_edit(Edit::range_replacement(
		        asname.to_string(),
		        alias.range(),
		    )));
    }

    checker.diagnostics.push(diagnostic);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:105 on 2024-11-14 06:47_

Is it possible to move this as well into `requires_member_import`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:137 on 2024-11-14 06:48_

Nit: Same as above, let's invert the check so that we don't need to duplicate the diagnostic creation

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/isort/settings.rs`:79 on 2024-11-14 06:49_

Can we use these new methods in the `isort` implementation as well? It would help to ensure that both implementations stay in sync

---

_@MichaReiser approved on 2024-11-14 06:49_

---

_@dylwil3 reviewed on 2024-11-14 17:05_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:95 on 2024-11-14 17:05_

After creating the diagnostic, the `UselessImportAlias` object is no longer accessible so we can't change the `required_import_conflict` field to `false`. I could create a mutable instance of the violation, but I'd still have to make the diagnostic twice unless I'm missing something (which I probably am!)

---

_@dylwil3 reviewed on 2024-11-14 17:06_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:105 on 2024-11-14 17:06_

I'm not sure I understand the suggestion because `required_member_import` might check for something like `import numpy` which is not aliased at all (and shouldn't cause an early return). This block seems to belong to the logic of "are you aliased and useless" rather than "are you required", right?

---

_@MichaReiser reviewed on 2024-11-14 17:07_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:95 on 2024-11-14 17:07_

You could assign `requried_import_conflict` to a variable and initialize the property with it?

---

_@dylwil3 reviewed on 2024-11-14 17:08_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/isort/settings.rs`:79 on 2024-11-14 17:08_

In the `isort` implementation we are doing the reverse thing: iterating through the required imports and unpacking them to see if they are found (and adding them if not), rather than packaging up some strings into a named import and then checking if the isort settings contain that. So I'm not sure how to use these specific methods in that case. (apologies again if I'm misunderstanding something here!)

---

_@MichaReiser reviewed on 2024-11-14 17:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:105 on 2024-11-14 17:08_

Makes sense. I assumed that a similar check exists in the isort code and I wanted to deduplicate it. Thanks for explaining. 

---

_@dylwil3 reviewed on 2024-11-14 17:19_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/useless_import_alias.rs`:95 on 2024-11-14 17:19_

🤦 duh thanks 😄 

---

_Merged by @dylwil3 on 2024-11-14 21:39_

---

_Closed by @dylwil3 on 2024-11-14 21:39_

---

_Branch deleted on 2024-11-14 21:39_

---

_Branch restored on 2024-11-17 14:28_

---
