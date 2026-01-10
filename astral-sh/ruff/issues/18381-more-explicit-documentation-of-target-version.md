```yaml
number: 18381
title: "More Explicit Documentation of `target-version` behavior"
type: issue
state: closed
author: asglover
labels:
  - question
assignees: []
created_at: 2025-05-30T02:12:51Z
updated_at: 2025-05-30T03:06:53Z
url: https://github.com/astral-sh/ruff/issues/18381
synced_at: 2026-01-10T11:09:58Z
```

# More Explicit Documentation of `target-version` behavior

---

_Issue opened by @asglover on 2025-05-30 02:12_

@ntBre 

Following up on my comment on this [issue](https://github.com/astral-sh/ruff/issues/10290#issuecomment-2920992425): 

The current documentation for `target-version` read like [this](https://docs.astral.sh/ruff/settings/#target-version): 

> The minimum Python version to target, e.g., when considering automatic code upgrades, like rewriting type annotations. Ruff will not propose changes using features that are not available in the given version.

Would you object to clarifying that `target-version` does not mean that ruff will lint to make sure that code will run on the minimum target version syntax error free? If you want, linking to the issue? It could read something like. 

> The minimum Python version to target, e.g., when considering automatic code upgrades, like rewriting type annotations. Ruff will not propose changes using features that are not available in the given version. [Currently](https://github.com/astral-sh/ruff/issues/2501), Ruff does not identify syntax issues based on the minimum supported target-version. i.e. using `match:` with `target-version=="py38"` will not trigger an error, but ruff will never generate code using `match` if  `target-version=="py38"`. 

Maybe I didn't do a close enough reading and this is a skill issue, I just thought that the behavior of target-version was different until close inspection. 

```
[tool.ruff]
# Always generate Python 3.7-compatible code.
target-version = "py37"
```

^ This is what got me 

because I can do something silly : 

```python
def func(a):
    match a: 
        case 'yo':
            import numpy # unused import 
            pass

```

and with `target-version=py38` 

`ruff check --fix `

will generate this code 

```python 
def func(a):
    match a: 
        case 'yo':
            pass

```

so I think maybe the language in the documentation should be clarified slightly that it won't generate changes which explicitly introduce unsupported syntax, but it will fix other errors in code with unsupported syntax. 

When the `DOWN` features land, I think they should be turned on by default when someone specifies `target-version`, as that's what I thought the feature did in the first place. 

What are your thoughts? 

---

_Comment by @ntBre on 2025-05-30 02:40_

Ah okay, thanks for opening the issue! For syntax errors specifically, we've fairly [recently](https://github.com/astral-sh/ruff/issues/6591) expanded our coverage for version-related syntax errors. If you enable preview mode, you'll get a diagnostic about `match` not being supported before 3.10: https://play.ruff.rs/16a17a46-5e43-4f2d-b147-9ce97d96b7cf

Is that the kind of thing you're looking for? I think we're planning to stabilize the new syntax errors soon, possibly in the next minor release, but they're still behind the `preview` flag/option for now.

---

_Label `question` added by @ntBre on 2025-05-30 02:40_

---

_Comment by @asglover on 2025-05-30 03:06_

Okay, I guess if these changes are landing soon, and that's coming with changes to the documentation, then there's little point in changing the documentation just to change it back. 

And just warning on the issues is a great start which makes --fix'es possible in the future.  

Thanks for the info, good luck on the feature! 

---

_Closed by @asglover on 2025-05-30 03:06_

---
