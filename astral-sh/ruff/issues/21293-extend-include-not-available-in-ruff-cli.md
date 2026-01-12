```yaml
number: 21293
title: "--extend-include not available in ruff CLI ?"
type: issue
state: closed
author: moshekaplan
labels:
  - question
assignees: []
created_at: 2025-11-06T04:30:49Z
updated_at: 2025-11-06T20:01:09Z
url: https://github.com/astral-sh/ruff/issues/21293
synced_at: 2026-01-12T15:54:57Z
```

# --extend-include not available in ruff CLI ?

---

_@moshekaplan_

I am running ruff 0.14.3 on Windows 11 x64.
```
> ruff --version
ruff 0.14.3
```

According to the [ruff documentation](https://docs.astral.sh/ruff/settings/#extend-include), there's an option `extend-include`  to add additional files. However, it does not appear to be available via the CLI:

```
> ruff check --extend-include "test.py"
error: unexpected argument '--extend-include' found

  tip: a similar argument exists: '--extend-exclude'

Usage: ruff check --extend-exclude <FILE_PATTERN> [FILES]...

For more information, try '--help'
```

If this is a user error, can you please provide guidance?

If `--extend-include` is not yet supported for the CLI, would it be possible to add `--extend-include` to the CLI?

---

_Label `question` added by @MichaReiser on 2025-11-06 13:21_

---

_Comment by @MichaReiser on 2025-11-06 13:27_

Hi @moshekaplan 

The linked documentation is for Ruff's configuration file. You can see the CLI documentation by running `ruff check -h`.

To include more files from the CLI, you can use `ruff check test.py` because paths provided on the CLI are always checked, except when using `--force-exclude`

---

_Comment by @moshekaplan on 2025-11-06 14:21_

@MichaReiser : Got it. And to maintain the default behavior of checking `.`, it would seem like I'd need to run it as: `ruff check . second_file` .

Thanks for resolving this.

---

_Comment by @MichaReiser on 2025-11-06 20:01_

Yes, you'd have to add `.` too

---

_Closed by @MichaReiser on 2025-11-06 20:01_

---
