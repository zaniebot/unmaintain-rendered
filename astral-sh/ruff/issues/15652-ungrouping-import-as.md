---
number: 15652
title: "Ungrouping `import` ... `as` ..."
type: issue
state: closed
author: CmpCtrl
labels:
  - question
  - isort
assignees: []
created_at: 2025-01-21T16:27:29Z
updated_at: 2025-01-29T12:41:03Z
url: https://github.com/astral-sh/ruff/issues/15652
synced_at: 2026-01-10T01:22:56Z
---

# Ungrouping `import` ... `as` ...

---

_Issue opened by @CmpCtrl on 2025-01-21 16:27_

Ruff recently started ungrouping imports from a module when using `import` ... `as` ..., Am i doing something wrong?

I'm using Ruff v2025.2.0 in vscode.

Silly example:
``` python
from numpy import ( # noqa: F401
    array as np_array,
    ndarray as np_ndarray,
    float64 as np_float64,
    int32 as np_int32,
    int64,
    uint32,
    uint64,
)
```

formats to:
``` python
from numpy import (  # noqa: F401
    array as np_array,
)
from numpy import (
    float64 as np_float64,
)
from numpy import (
    int32 as np_int32,
)
from numpy import (
    int64,
    uint32,
    uint64,
)
from numpy import (
    ndarray as np_ndarray,
)
```

---

_Label `question` added by @MichaReiser on 2025-01-21 17:17_

---

_Comment by @MichaReiser on 2025-01-21 17:17_

hi

I don't think anything changed recently in isort that changed the behavior but have you tried setting [`combine-as-imports`](https://docs.astral.sh/ruff/settings/#lint_isort_combine-as-imports) to `true`?

---

_Label `isort` added by @dhruvmanila on 2025-01-22 05:23_

---

_Comment by @CmpCtrl on 2025-01-22 14:52_

That setting didn't seem to have any influence. I'm confused how this works, i had assumed that formatting imports happened with format (either right-click format or format on save) but that doesn't seem to be the case?

So i was using a vscode setting of:

``` json
     "[python]": {
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "charliermarsh.ruff",
        "editor.codeActionsOnSave": {
            "source.organizeImports": "explicit"
        },
    },
```

If i do `shift-alt-o` to organize inputs, ruff separates the `import as` but i also tried isort and that works as expected.

---

_Comment by @MichaReiser on 2025-01-22 14:55_

Yes, `format` doesn't organize your imports. You have to use the explicit organize imports command. 

> That setting didn't seem to have any influence. I

Interesting, setting `combine-as-imports` to `true` does keep the imports together when I tried, see


https://play.ruff.rs/f5e639be-75b4-4346-9678-fbc010068b30

---

_Comment by @dhruvmanila on 2025-01-23 09:22_

Do you see this only in an editor context or is the behavior same from the command-line? If it's different, can you try using `source.organizeImports.ruff` instead? The one that you've is a broader code action which means if there are any other extensions that supports organizing imports in a Python file, they will be invoked as well.

---

_Comment by @CmpCtrl on 2025-01-23 14:58_

How do i run ruff from the command line in VScode correctly? I have the ruff extension installed.
If i run `ruff check --fix ruff_issue.py` i get the `ruff : The term 'ruff' is not recognized as the name of a cmdlet, function, script file, or operable program.`.

if i run the full path (which i found from the ruff output terminal) i get `All checks passed!` and it doesn't change anything (i think it should sort the order).
`c:\Users\xxx\.vscode\extensions\charliermarsh.ruff-2025.2.0-win32-x64\bundled\libs\bin\ruff.exe check --fix ruff_issue.py`

Switching the vscode setting to the following makes no change, it then ungroups the import as.
``` json
    "[python]": {
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "charliermarsh.ruff",
        "editor.codeActionsOnSave": {
            "source.organizeImports.ruff": "explicit"
        },
    },
```

Clearly i am doing something wrong, and haven't had time to learn how to use this correctly, so please dont be shy about pointing out whatever basic obvious thing i'm missing. Thanks for the help.

---

_Comment by @MichaReiser on 2025-01-25 11:09_

> Clearly i am doing something wrong, and haven't had time to learn how to use this correctly, so please dont be shy about pointing out whatever basic obvious thing i'm missing. Thanks for the help.

I don't think there isn't anything obvious. At least there's nothing obvious to me. 

Do you have other extensions installed? Maybe isort? Could you try disabling those extensions to see if the issue still reproduces?

---

_Comment by @dhruvmanila on 2025-01-29 09:36_

@CmpCtrl Is the issue that Ruff organizes the imports from first code snippet to the second code snippet as seen in the issue description? If so, then that's the default formatting. If you'd like to keep them combined, then [`combine-as-imports`](https://docs.astral.sh/ruff/settings/#lint_isort_combine-as-imports) setting can be used as mentioned in https://github.com/astral-sh/ruff/issues/15652#issuecomment-2607465479 (refer to the playground link).

---

_Comment by @CmpCtrl on 2025-01-29 12:41_

Thanks yea, so i made a new `pyproject.toml` with that setting this morning and it now works. I was confused since i had tried that when it was suggested before and it made no change to the behavior. I must have had a typo somewhere. I'm also confused about why the behavior seemed to change recently, i must have had the isort extension active in the past. Thanks for the tips.

---

_Closed by @CmpCtrl on 2025-01-29 12:41_

---
