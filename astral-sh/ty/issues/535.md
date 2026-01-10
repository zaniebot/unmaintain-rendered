```yaml
number: 535
title: Auto-import action
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-05-28T11:49:48Z
updated_at: 2025-11-14T12:58:01Z
url: https://github.com/astral-sh/ty/issues/535
synced_at: 2026-01-10T02:06:24Z
```

# Auto-import action

---

_Issue opened by @MichaReiser on 2025-05-28 11:49_

Provide an action to automatically import a symbol if it isn't defined in the current module but exported by another module.

---

_Added to milestone `GA` by @MichaReiser on 2025-05-28 11:49_

---

_Label `server` added by @MichaReiser on 2025-05-28 11:49_

---

_Comment by @BurntSushi on 2025-05-29 17:14_

@MichaReiser Can you give an example of this? In particular, the way I'm interpreting this seems to imply that completions should be offering suggestions that _aren't_ yet imported.

---

_Comment by @MichaReiser on 2025-05-29 17:24_

Thinking more about it, maybe that's more a refactor than something that should be part of completions (because that could be noisy). 

The idea was that if you write `TypeInferenc` that it would auto complete `TypeInference` and automatically add the necessary import. 

I think a better start would be to provide a: Add import code action for when we see an unknown symbol which is exported by another module. E.g, you have `TypeInference` but it isn't imported. ty should then show an action to import it. I'll change the issue

---

_Renamed from "Auto-import completion" to "Auto-import action" by @MichaReiser on 2025-05-29 17:24_

---

_Comment by @BurntSushi on 2025-05-29 17:26_

Yeah I think that's probably a better starting point.

Do we already have a reverse index somewhere that maps names to modules available to import in the current environment?

---

_Comment by @BurntSushi on 2025-05-29 17:27_

This seems somewhat related to a "find references to this symbol" feature. Although they aren't quite the same.

---

_Comment by @MichaReiser on 2025-05-29 17:33_

> Do we already have a reverse index somewhere that maps names to modules available to import in the current environment?

Not yet, no. We also don't have an index of all modules 

---

_Comment by @dhruvmanila on 2025-05-30 08:54_

> I think a better start would be to provide a: Add import code action for when we see an unknown symbol which is exported by another module. E.g, you have `TypeInference` but it isn't imported. ty should then show an action to import it. I'll change the issue

Agreed. Code action seems like a good start for this. Another thing is that I think it should just be a quickfix code action and not source level code action i.e., it should be specific to a single unknown symbol and not for all unknown symbols in the file.

If we want to break it down even further, we could limit the initial implementation to just have modules and not include any symbols within those modules. For example, `sys` could provide a code action to auto-import `import sys` but `version_in|` (using `from sys import version_info`) would not work for now. Or, this could be the next step in this.

---

_Comment by @ibrokemypie on 2025-06-06 00:45_

Chiming in to say that auto-import as an auto-completion item like in other LSPs (pyright, ts_ls) is a very important feature to me.

---

_Comment by @MichaReiser on 2025-06-09 07:19_

https://github.com/astral-sh/ty/issues/614 might be a good first step

---

_Removed from milestone `GA` by @carljm on 2025-06-11 00:48_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:48_

---

_Comment by @seamuswn on 2025-07-23 11:17_

Most LSPs I've seen that provide this provide both a code completion and a code actions implementation, with the option to enable/disable each one independently of the other. Pycharm docs have a [pretty good example](https://www.jetbrains.com/help/pycharm/creating-and-optimizing-imports.html#import-packages) of how code actions could work under 'Creating imports on the fly﻿'. Only callout is the support for adding imports either 'locally' (import is added at the same indentation on the line just above the statement) or at the module level.


The standard configurations options I see for LSPs that support this are:
1. Import syntax configuration (choosing between `import sys.version_info` and `from sys import version` statements being generated). Avoids having to pop up / suggest both syntax types in the context menus, since most projects will favor one or the other).
2. Configurable aliasing (e.g. providing a configuration value of `"alias": {"numpy": "np"}` and having that generate `import numpy as np` instead of `import numpy`) - more of a 'nice to have'.

---

_Assigned to @BurntSushi by @carljm on 2025-08-15 15:04_

---

_Comment by @sirfz on 2025-10-10 17:43_

For reference, when using basedpyright in neovim, this is the behavior:

* For example I input `dict[str, Any`
* `Any` isn't currently imported in my file but the autocomplete suggestions would display it and selecting it would automatically add `from typing import Any`.

However, it does not offer code actions for it which would be a great addition.

Sometimes it would offer the same import from different modules which can be tricky for ranking but usually std lib import should prevail.

---

_Comment by @BurntSushi on 2025-10-10 17:51_

@sirfz The ranking is a work in progress, but `dict[str, Any<CURSOR>` should work today:

https://github.com/user-attachments/assets/369cb82f-8677-4067-baf4-713aa7ce5d12

Note that you do need to [explicitly opt into `autoImport`](https://github.com/astral-sh/ty/blob/2f3190d0961d1067a097bfb2c00c3952311055c1/docs/reference/editor-settings.md#autoimport).

---

_Comment by @sirfz on 2025-10-10 17:55_

@BurntSushi I just opted in and got it working, that's wonderful thank you!

---

_Comment by @MichaReiser on 2025-11-14 09:31_

I'm leaning towards closing this issue and tracking the auto import improvements in smaller tasks. I'll create a new issue for a: Add import code action. But I'll leave this decision up to you @BurntSushi 

---

_Comment by @BurntSushi on 2025-11-14 12:57_

Aye, I've opened an issue specifically for an "add missing import" action: https://github.com/astral-sh/ty/issues/1552

---

_Closed by @BurntSushi on 2025-11-14 12:58_

---
