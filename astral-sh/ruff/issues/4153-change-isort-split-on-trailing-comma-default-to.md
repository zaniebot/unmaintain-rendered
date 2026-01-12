```yaml
number: 4153
title: "Change `isort.split-on-trailing-comma` default to false"
type: issue
state: closed
author: janosh
labels:
  - question
assignees: []
created_at: 2023-04-29T21:36:24Z
updated_at: 2023-05-21T20:56:43Z
url: https://github.com/astral-sh/ruff/issues/4153
synced_at: 2026-01-12T15:54:44Z
```

# Change `isort.split-on-trailing-comma` default to false

---

_@janosh_

[Currently defaults to true](https://beta.ruff.rs/docs/settings/#split-on-trailing-comma), causing this to stay as is

```py
from a import (
    b,
)
```

I think a better default would be to fold:

```py
from a import b
```

---

_Comment by @charliermarsh on 2023-04-29 23:00_

I think the current default is "correct" because it's consistent with Black's behavior and isort's `profile = "black"`, unless I'm misunderstanding. Black respects these "magic trailing commas" (which has the downside you've outlined above: if you have a trailing comma, Black will never remove it) and so removing them would break Black compatibility by default.

So, I'd rather err on the side of leaving this default as-is. Users are, of course, welcome to change the setting in their own projects :)

---

_Label `question` added by @charliermarsh on 2023-04-29 23:00_

---

_Comment by @janosh on 2023-04-29 23:04_

Good point. Didn't consider `black`. I wish they'd change this default but seems very unlikely.

---

_Closed by @janosh on 2023-04-29 23:04_

---

_Comment by @intgr on 2023-05-20 19:46_

I also found this behavior surprising. I think `blacks`'s behavior is not relevant here, black is fine with either formatting. We should follow `isort`'s behavior instead.

`split-on-trailing-comma = false` matches `isort`'s default behavior (with or without `profile = "black"`).


---

_Comment by @intgr on 2023-05-20 19:50_

> it's consistent with Black's behavior and isort's profile = "black", unless I'm misunderstanding

I think you're misunderstanding isort's behavior :)

---

_Comment by @charliermarsh on 2023-05-20 20:07_

Is this not a bug in isort? The documentation says that `split-on-trailing-comma` defaults to `true` for the Black profile (and collapsing imports like that is definitely inconsistent with Black).


---

_Comment by @intgr on 2023-05-20 22:13_

If that's the case, it sounds like a bug indeed.

Where does it say that? https://pycqa.github.io/isort/docs/configuration/options.html#split-on-trailing-comma only says the behavior mimics black when enabled, not that it's set to true with black profile.

---

_Comment by @charliermarsh on 2023-05-20 22:15_

In the [Profiles](https://pycqa.github.io/isort/docs/configuration/profiles.html) documentation:

<img width="1792" alt="Screen Shot 2023-05-20 at 6 15 10 PM" src="https://github.com/charliermarsh/ruff/assets/1309177/de6ed6c4-3c26-4a6b-8e16-876c363cc909">


---

_Comment by @charliermarsh on 2023-05-20 22:16_

(I agree with you that I'm not seeing this behavior in practice, so not certain what's wrong: the documentation or the implementation.)

---

_Comment by @intgr on 2023-05-20 22:18_

Ah OK, I was looking at the "Black Compatibility" and "Options" sections of their docs.

---

_Comment by @johnthagen on 2023-05-21 18:40_

> (and collapsing imports like that is definitely inconsistent with Black).

One minor note I wanted to bring up, it is inconsistent with how Black _would_ have formatted it, but if isort or Ruff _do_ format it to:

```py
from a import b
```

Black is happy with it, just like if a user would have compressed it to one line manually. We've been running this way with isort + black for years, but it does seem like this might not have been intentional as mentioned in this thread.

I do find this useful so I'm glad to hear there is an opt-out, since unlike other parts of code, it'd be much less likely for a user to _intentionally_ want to lengthen imports as they might some other code since imports are mostly boilerplate. In my experience it's much more likely that a user has their editor automatically remove an unused import but it leaves the trailing comma for another import from that same module. I've observed that over time in large code bases without `isort` collapsing these it can lead to a lot of wasted vertical space.

---

_Comment by @janosh on 2023-05-21 20:56_

> if isort or Ruff _do_ format it to:
> 
> ```python
> from a import b
> ```
> 
> Black is happy with it

Great point. I'd appreciate departing from `isort` profile `black` in this respect since `split-on-trailing-comma=true` is a poor default imo for the same reason @johnthagen mentioned

> it'd be much less likely for a user to intentionally want to lengthen imports

---
