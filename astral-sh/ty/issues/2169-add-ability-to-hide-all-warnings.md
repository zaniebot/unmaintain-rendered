```yaml
number: 2169
title: Add ability to hide all warnings
type: issue
state: open
author: tino
labels:
  - cli
  - configuration
  - needs-info
assignees: []
created_at: 2025-12-22T18:54:10Z
updated_at: 2026-01-15T16:09:48Z
url: https://github.com/astral-sh/ty/issues/2169
synced_at: 2026-01-15T16:50:04Z
```

# Add ability to hide all warnings

---

_@tino_

I just want to focus on errors for now. I guess I can look up all the warning rules and call `--ignore <RULE>` for each but that's a bit tedious.

`| grep -v warning` works for the concise output format, but having a `--errors-only` or something like it would still be very nice!

---

_Label `configuration` added by @AlexWaygood on 2025-12-22 18:56_

---

_Comment by @dhruvmanila on 2025-12-23 05:51_

This seems like a reasonable thing to have (cc @MichaReiser)

ESLint uses the `--quiet` flag to hide warnings (https://eslint.org/docs/latest/use/command-line-interface#--quiet)

Biome uses `--diagnostic-level`:

> `--diagnostic-level=<info|warn|error>` — The level of diagnostics to show. In order, from the lowest to the most important: info, warn, error. Passing `--diagnostic-level=error` will cause Biome to print only diagnostics that contain only errors.

---

_Label `cli` added by @dhruvmanila on 2025-12-23 05:51_

---

_Comment by @MichaReiser on 2025-12-23 08:52_

The proper solution here depends on what exactly the issue author is looking for. 

Adding a CLI flag to hide warning diagnostics is certainly something we can do. However, this won't hide the warnings in the editor. 

The alternative to this would be something like profiles (see https://github.com/astral-sh/ty/issues/2129) that allow users to easily configure ty's strictness. 

@tino can you say a little more about your specific use case?

---

_Label `needs-info` added by @MichaReiser on 2025-12-23 08:52_

---

_Comment by @athoscouto on 2025-12-30 17:04_

@MichaReiser I was about to open a similar issue
My use case is running it during CI
My project currently have 800+ warnings
But only errors will block our CI, and I want other devs in the team to clearly see them when they happen

---

_Comment by @MichaReiser on 2025-12-30 17:36_

>  But only errors will block our CI, and I want other devs in the team to clearly see them when they happen

Thanks. That's helpful. As another alternative design. Cargo has a `warnings` category. It's a bit weird because the category is dynamic. You only know what rules are in the `warnings` set after resolving the entire configuration. But it avoids any extra config flags. But it's a flexible combination of `error-on-warning` and any `--ignore warnings`

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:44_

---

_Comment by @odie5533 on 2026-01-11 04:55_

My use case:
When a developer runs a check locally, it returns 200 warnings and errors all combined.
Only **errors** should block their pre-commit (and CI), so I'd like to be able to filter to just show the errors with full details.

Other flag possibilities:
* --min-severity
* --errors-only

---

_Comment by @tino on 2026-01-15 16:09_

@MichaReiser my use case is the same, to use during CI/pre-commit. Seeing warnings in the editor is fine for me. We can already silence rules that we don't want, wether that's a warning or an error.

Regarding the implementation, we'd start with the default config. So I'd rather have a single flag than having to compile some profile in which I determine what's a warning or not. `--errors-only` sounds reasonable.

---
