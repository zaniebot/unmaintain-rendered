```yaml
number: 17896
title: Remove R from redirects, as it is not a valid rule
type: pull_request
state: closed
author: VanOvermeire
labels: []
assignees: []
base: main
head: r-valid-at-runtime
created_at: 2025-05-06T19:13:46Z
updated_at: 2025-06-20T06:41:43Z
url: https://github.com/astral-sh/ruff/pull/17896
synced_at: 2026-01-10T18:39:08Z
```

# Remove R from redirects, as it is not a valid rule

---

_Pull request opened by @VanOvermeire on 2025-05-06 19:13_

## Summary

This PR should fix the following bug: https://github.com/astral-sh/ruff/issues/17890

## Test Plan

Tested against a local repository. main branch `ruff check --select R` works, with this redirect removed, `ruff check --select R` throws `error: invalid value 'R' for '--select <RULE_CODE>'`

All existing tests still pass. 

Not sure if it is worthwhile/necessary to add a unit test for this fix?

---

_@ntBre reviewed on 2025-05-06 19:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rule_redirects.rs`:24 on 2025-05-06 19:31_

Thanks for jumping on this!

Oh wow, I wonder if we need to remove this whole section of `R*`-`RET*` pairs. Based on these dates, we could possibly delete quite a large chunk here. @MichaReiser do we need to wait for a minor release for this?

---

_@MichaReiser reviewed on 2025-05-07 07:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rule_redirects.rs`:24 on 2025-05-07 07:37_

I don't think it's enough to just remove the redirect from `R` to `RET`. We should also remove the other `R` redirects to `R`.

Yes, I think we have to wait for a minor and we should start raising deprecation warnings first. Are there other remappings that have the same issue?


Do we already raise a deprecation warning if someone uses `R`? 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rule_redirects.rs`:24 on 2025-05-07 20:21_

> Yes, I think we have to wait for a minor and we should start raising deprecation warnings first. Are there other remappings that have the same issue?

Yes, it looks like "U"/"UP" has the same issue ("U" is not present in our JSON schema but selecting "U" enables the UP rules). That's also true for "IC"/"ICN" so I suspect it's true for all of these, but I haven't verified them all. This is how I'm checking:

```shell
$ ruff check --select IC --show-settings | sed -n '/linter.rules.enabled/,/^]/p'
linter.rules.enabled = [
        unconventional-import-alias (ICN001),
        banned-import-alias (ICN002),
        banned-import-from (ICN003),
]
```

and then cross-referencing with SchemaStore [here](https://github.com/SchemaStore/schemastore/blob/master/src/schemas/json/ruff.json).

> Do we already raise a deprecation warning if someone uses `R`?

I don't see any warnings from this:

```shell
$ ruff check --select R
All checks passed!
```

So maybe that's the first PR we need to get this started?

---

_@ntBre reviewed on 2025-05-07 20:21_

---

_@MichaReiser reviewed on 2025-05-12 08:15_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rule_redirects.rs`:24 on 2025-05-12 08:15_

Yes, starting to emit warnings would be the first step if we still want to change this behavior. 

---

_Comment by @MichaReiser on 2025-06-20 06:41_

Closing due to inactivity. If someone's interested to pick this up, feel free to submit a new PR and reference this PR in the summary. Thank you @VanOvermeire for your work here

---

_Closed by @MichaReiser on 2025-06-20 06:41_

---
