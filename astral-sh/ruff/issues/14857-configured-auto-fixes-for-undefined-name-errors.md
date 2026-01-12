```yaml
number: 14857
title: Configured auto-fixes for undefined-name errors
type: issue
state: closed
author: leep-frog
labels:
  - fixes
assignees: []
created_at: 2024-12-09T02:34:50Z
updated_at: 2024-12-16T07:39:42Z
url: https://github.com/astral-sh/ruff/issues/14857
synced_at: 2026-01-12T15:54:54Z
```

# Configured auto-fixes for undefined-name errors

---

_@leep-frog_

It would be tremendously useful if we could add the following configuration to automatically fix [undefined-name](https://docs.astral.sh/ruff/rules/undefined-name/) errors:

```
[format]

auto-import-names = [
  "pd:import pandas as pd",
  "Dict:from typing import Dict",
]
```

which would fix the following code:

```
def some_func() -> Dict[str, pd.array]:
  return {
    "key": pd.array([]),
  }
```

to become the following automatically

```
import pandas as pd
from typing import Dict

def some_func() -> Dict[str, pd.array]:
  return {
    "key": pd.array([]),
  }
```

---

_Comment by @dhruvmanila on 2024-12-09 05:28_

Thanks for the proposal.

I definitely agree that this is a really useful feature although I'd argue that a language server is more capable of providing such features (as they already do) via auto-completion. If you type `pand...`, the auto-completion menu could include a completion for `pandas` with an edit which will add the import. For example, this is what Pyright gives me:

![Image](https://github.com/user-attachments/assets/0edf163c-0295-472c-a06a-c6b5b33eb473)

The reason I'm saying that language servers are more capable is because they understand your project at a much deeper level. They would understand all the packages that are available in the current virtual environment which is where this suggestion would come from. If this configuration is implemented in Ruff, it would assume all `pd` symbols to be coming from `pandas` module and add an `import pandas as pd` on all files that contain that symbol even if that might not be true.


---

_Comment by @MichaReiser on 2024-12-09 09:04_

We could consider adding it as a manual fix to the existing rules and respect the import-convention configuration to suggest common imports. 

The main challenge with arbitrary imports is that Ruff doesn't understand the project dependencies yet. It limits what kind of suggestions we can provide.

---

_Label `fixes` added by @AlexWaygood on 2024-12-09 12:52_

---

_Comment by @leep-frog on 2024-12-10 00:21_

Thanks for the prompt response.  Your point about language server definitely makes sense and is how I currently go about it.  The main reasons I'd really like to see this in ruff is the following:

1. No user interaction would be required for ruff auto-importing.  If I type (`def func(d: Dict[str, Any]`), for example, (assuming I have an auto-formatter setup in my IDE to trigger on certain typed characters), then these imports (`Dict, Any`) would be automatically added without any selection having to be made and my flow is not interrupted at all.  Just simply type away and assume ruff will take care of these things.
2. Occasionally, my language server (Pylance) takes a really long time to generate recommendations.  I'll sometimes type `pd` and end up waiting longer than it would have taken to just do it myself (albeit this may be more of a complaint with Pylance than a requirement to add this feature in ruff)

Ultimately, I think this would be really great for simple, common imports (`Dict`, `Optional`, common typing annotations etc., `pd`, `np`), but agree that this should not be recommended for more nuanced auto-imports.

That being said, certainly neither of my arguments make this a definitive requirement for ruff, and only a nice-to-have, so will leave it to you to decide whether to pick this up or not.  But thought it is something that thought others might find useful!

---

_Comment by @leep-frog on 2024-12-10 00:22_

I'm also working on a VS Code extension to do this btw.  Should be done within a week I'd say.  Not sure if that affects how you prioritize this, but figured I'd mention it.

---

_Comment by @dhruvmanila on 2024-12-10 04:22_

> We could consider adding it as a manual fix to the existing rules

I believe we already do, at least the part where we auto-import certain symbols if required for an auto-fix. 

> ... and respect the import-convention configuration to suggest common imports.

Can you expand on this? Do you mean that if an auto-fix were to import the `pandas` module then it should check if the convention is to use `import pandas as pd` or not?

> That being said, certainly neither of my arguments make this a definitive requirement for ruff, and only a nice-to-have, so will leave it to you to decide whether to pick this up or not. But thought it is something that thought others might find useful!

I agree and thank you for providing your thoughts on this.

---

_Comment by @leep-frog on 2024-12-16 03:26_

> I agree and thank you for providing your thoughts on this.

No problem!

And I just published [this VS Code extension](https://marketplace.visualstudio.com/items?itemName=groogle.very-import-ant) that does what this issue requested, so now it might be even less of a priority to add this functionality inside of ruff itself.

Given that, I'm fine if you close this, but will defer to you if you're still pondering adding it to ruff even with the new extension.

---

_Comment by @MichaReiser on 2024-12-16 07:39_

Thanks for the update and nice extension!

I'll close this issue for now because there's no immediate action on our side. This does not mean that we don't plan on adding this functionality to Ruff's LSP long-term

---

_Closed by @MichaReiser on 2024-12-16 07:39_

---
