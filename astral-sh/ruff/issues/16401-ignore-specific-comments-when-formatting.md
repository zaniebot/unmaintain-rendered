```yaml
number: 16401
title: Ignore specific comments when formatting
type: issue
state: open
author: toewen
labels:
  - formatter
assignees: []
created_at: 2025-02-26T15:53:17Z
updated_at: 2025-08-16T17:11:14Z
url: https://github.com/astral-sh/ruff/issues/16401
synced_at: 2026-01-12T15:54:55Z
```

# Ignore specific comments when formatting

---

_@toewen_

### Summary

I have python scripts I execute on a slurm cluster. 

To give some configurations to the cluster the script starts with comments in a specific style. For example

```python
#!/bin/python
#SBATCH -n 5
#SBATCH -N 1
```

When formatting this file the Shebang is left as is but the other comments get treated as regular comments and a space is inserted between `#` and `SBATCH`. Unfortunately this space prevents the slurm cluster from recognizing the comments.

My current solution is

```python
#!/bin/python
# fmt: off
#SBATCH -n 5
#SBATCH -N 1
# fmt: on
```

I would like an option to ignore these comments when formatting a file.

---

_Label `formatter` added by @MichaReiser on 2025-02-26 16:14_

---

_Comment by @MichaReiser on 2025-02-26 16:16_

There was a similar issue in the past but I can't find it right now :( 

I could see an option to specify comment patterns that should never be formatted.

---

_Comment by @ali-ramadhan on 2025-08-16 04:50_

This would also be great to preserve vertical folding (or region) comments like `#+++` or `# region`.

@MichaReiser Perhaps you were thinking of https://github.com/astral-sh/ruff/issues/9805 ?

---

_Comment by @Avasam on 2025-08-16 17:11_

- My issue https://github.com/astral-sh/ruff/issues/9805 as Ali mentioned seems related.
- There's https://github.com/astral-sh/ruff/issues/11941 that's also very similar
- There's also https://github.com/astral-sh/ruff/issues/11789, but that's about formatting the comments, not ignoring them.

---
