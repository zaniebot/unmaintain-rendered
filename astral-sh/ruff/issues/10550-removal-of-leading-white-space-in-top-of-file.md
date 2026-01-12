```yaml
number: 10550
title: removal of leading white-space in top-of-file comment 
type: issue
state: closed
author: janwilmans
labels:
  - docstring
  - formatter
assignees: []
created_at: 2024-03-24T18:30:27Z
updated_at: 2024-03-25T07:44:21Z
url: https://github.com/astral-sh/ruff/issues/10550
synced_at: 2026-01-12T15:54:50Z
```

# removal of leading white-space in top-of-file comment 

---

_@janwilmans_

using ruff vscode plugin,  default configuration but with:

```
[tool.ruff]
line-length = 160
indent-width = 4
```

Before code format:
```
#!/usr/bin/env python3
"""Returns with exitcode 1 if /....., otherwise 0 (success)
    description bla bla description
    description bla bla description
"""
```

after code format:

```
#!/usr/bin/env python3
"""Returns with exitcode 1 if /....., otherwise 0 (success)
description bla bla description
description bla bla description
"""
```

All leading whitespace in comments up top seems to be removed, however, in raw-strings that are assigned to a variable, this does not happen.



---

_Label `docstring` added by @AlexWaygood on 2024-03-24 19:51_

---

_Label `formatter` added by @AlexWaygood on 2024-03-24 19:51_

---

_Comment by @charliermarsh on 2024-03-24 19:57_

I believe this is by design. The formatter normalizes docstrings, and a string expression at the top of a file is a docstring.

---

_Comment by @janwilmans on 2024-03-24 23:40_

The docstring can change the behaviour of the program, such as with docopt, so I think there should at least be a way to disable this ?

---

_Comment by @zanieb on 2024-03-25 00:54_

This looks like a duplicate of #10396 

You can disable this with `# fmt: skip` as discussed there.

---

_Comment by @janwilmans on 2024-03-25 07:34_

You are correct, this is a duplicate, I did not find it because I did not look for 'docstring' as a keyword, silly me, thank you for pointing that out.

However, I think it is not a reasonable solution to annotate all python files with tool-specific annotations, I see there is an issue [#4276](https://github.com/psf/black/issues/4276) for this on the Black repo. They are also of the opinion that modifying user data is allowed by a code formatter. 

I'm not sure I follow this logic, any change that have a runtime effect is a bug to me. I will go back to autopep8, feel free to close this issue.


---

_Closed by @MichaReiser on 2024-03-25 07:44_

---
