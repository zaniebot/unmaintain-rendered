```yaml
number: 7353
title: "CLI: Toggle default output format based on the number of violations"
type: issue
state: open
author: zanieb
labels:
  - cli
assignees: []
created_at: 2023-09-13T16:25:57Z
updated_at: 2024-04-02T07:47:45Z
url: https://github.com/astral-sh/ruff/issues/7353
synced_at: 2026-01-10T11:09:49Z
```

# CLI: Toggle default output format based on the number of violations

---

_Issue opened by @zanieb on 2023-09-13 16:25_

In #7349 and #7352 the default output of `ruff check` includes much more content. However, this comes with a performance degradation for large numbers of violations as computing the display of source and fixes is expensive. A possible solution is to toggle the default output format between "full" and "concise" (#7350) depending on the number of detected violations. If we see more than 30 violations (for example), we should switch to the concise output. If `--format full` or `--format concise` is provided explicitly (or in the configuration file) we should _always_ respect the given format regardless of the number of violations.

---

_Label `cli` added by @zanieb on 2023-09-13 16:26_

---

_Comment by @MichaReiser on 2024-03-30 08:01_

>  If --format full or --format concise is provided explicitly (or in the configuration file) we should always respect the given format regardless of the number of violations.

I'm undecided on this. I think the use case here is that sometimes it's desired to get the full output, even if computing the output takes a long or prints a lot of text (e.g., when piping in CI). However, I'm concerned that the *default* `full` (no configuration) behavior is different from the explicit `output-format: full. 

The main motivation for truncating fixes (we could, for example, print the first `n` violations in full, then print a message that Ruff has switched to concise) is to protect users from being overwhelmed and having a "slow"  experience. I think I would want that protection even when I explicitly say that I want the `full` output format. 

One less implicit solution would be to define a new `full-always` or similar output format. 

---

_Comment by @zanieb on 2024-03-30 14:33_

>  I think I would want that protection even when I explicitly say that I want the full output format.

I don't really follow this, why? If you explicitly request the full format it seems weird that we would not respect that.

> However, I'm concerned that the default full (no configuration) behavior is different from the explicit `output-format: full.`

We would encode the default behavior with a separate key like "auto" or "default" instead of overloading "full" to have two separate behaviors.

---

_Comment by @MichaReiser on 2024-04-02 07:47_

> I don't really follow this, why? If you explicitly request the full format it seems weird that we would not respect that.

Because I understood that having the "automatic" output format (that switches between full and concise) and the "full" output format use the same configuration name. But that's not the case according to:

> We would encode the default behavior with a separate key like "auto" or "default" instead of overloading "full" to have two separate behaviors.

So I'm okay with printing all diagnostics when explicitly requesting `full` (and not `auto`)

---
