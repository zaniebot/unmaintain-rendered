```yaml
number: 3826
title: Support for Python 3.6+
type: issue
state: closed
author: garthk
labels:
  - core
assignees: []
created_at: 2023-03-31T02:37:07Z
updated_at: 2023-11-06T13:56:36Z
url: https://github.com/astral-sh/ruff/issues/3826
synced_at: 2026-01-12T15:54:44Z
```

# Support for Python 3.6+

---

_@garthk_

G'day! Ruff 0.0.2060, given this config…

```toml
[tool.ruff]
target-version = "py36"
```

 … objects:

```plain
$ ruff check .
error: TOML parse error at line 2, column 18
   |
 2 | target-version = "py36"
   |                  ^^^^^^
unknown variant `py36`, expected one of `py37`, `py38`, `py39`, `py310`, `py311`
```

Like much of the community I'm pushing for everyone to upgrade to versions of Python supported by the Python team. Like some of the community, though, I'm _also_ sometimes stuck running my code in an environment running an older version of Python supported by the OS distribution. I'd much appreciate Ruff's help maintaining such code.

If you'll indulge me slightly missing the point of the most obvious Python sketch…

<img src="https://user-images.githubusercontent.com/15906/229007362-391d1d21-fb3c-4deb-a669-a9d163936e74.png" width="640" height="360" alt="Image macro showing the Four Yorkshiremen sketch with Tim Brooke-Taylor, John Cleese, Graham Chapman and Marty Feldman, overlaid with text suggesting using a modern version of Python is a luxury.">


---

_Comment by @charliermarsh on 2023-03-31 03:44_

Lol thank you for the meme :)

---

_Comment by @charliermarsh on 2023-03-31 03:49_

I would need to go through and do a proper accounting of the things we might break if you were to run with `--target-version py37` over code that needs to be Python 3.6 compatible.

My initial reaction is that if your code is intended to run in Python 3.6 and all later versions, Ruff _should_ be able to parse it, and there are no immediate autofix behaviors that come to mind as problematic for your code.

(If you were writing code that had to be Python 3.6 compatible, but didn't need to be Python 3.7 compatible, that'd be different, since 3.7 introduced backwards-incompatible syntax changes.)

---

_Comment by @charliermarsh on 2023-03-31 03:53_

That is to say: it might be totally fine to run with `--target-version py37` for code that's intended to be Python 3.6+ compatible, but I'm not 100% certain right now. (If you do so, it'd be safest not to enable the Pyupgrade rules which are the most prominent user of the target version.)

---

_Label `core` added by @charliermarsh on 2023-03-31 03:53_

---

_Renamed from "unknown variant py36" to "Support for Python 3.6+" by @charliermarsh on 2023-03-31 03:53_

---

_Comment by @garthk on 2023-03-31 10:21_

That'll do for now, sure; I'll be careful with any upgrade rule inclusions. Thanks!

---

_Comment by @charliermarsh on 2023-11-06 13:56_

Gonna close for now as we don't plan to explicitly support Python 3.6.

---

_Closed by @charliermarsh on 2023-11-06 13:56_

---
