```yaml
number: 13662
title: question about Python version priority
type: issue
state: open
author: dotysan
labels:
  - question
assignees: []
created_at: 2025-05-26T17:37:01Z
updated_at: 2025-05-27T08:34:07Z
url: https://github.com/astral-sh/uv/issues/13662
synced_at: 2026-01-12T16:01:34Z
```

# question about Python version priority

---

_@dotysan_

### Question

So I'm using uv to test a script with different Python versions using this shebang:
`#! /usr/bin/env -S uv run --verbose --script --managed-python --python=3.13`

I bumped it to 3.14 and tested. All went well. So I then set this:
`#! /usr/bin/env -S uv run --verbose --script --managed-python --python=">=3.13"`

However upon next exec, it still used 3.14. Since 3.13 is current/stable and 3.14 is still preview, shouldn't it prefer 3.13?

It appears uv is just using whatever version ran last.

Also, while testing, I set this shebang:
`#! /usr/bin/env -S uv run --verbose --script --managed-python --python=3.12`

But had a newer requires-python in the PEP 723 section.
```
# /// script
# requires-python = ">=3.13"
# dependencies = []
# ///
```

And got no error! Am I misunderstanding something about the priorities here. Setting requires-python seems to have no effect if uv is called with a newer --python version.

### Platform

Linux 6.10.2-x86_64-linode165 x86_64 GNU/Linux

### Version

uv 0.7.8

---

_Label `question` added by @dotysan on 2025-05-26 17:37_

---

_Comment by @dotysan on 2025-05-26 17:43_

Nevermind on my second question about conflicting --python and requires-python.

Looking closer, I do see a warning. Which is prolly sufficient.
```
warning: The requested interpreter resolved to Python 3.12.10, which is incompatible with the script's Python requirement: `>=3.13`
```

---

_Comment by @konstin on 2025-05-26 20:30_

> However upon next exec, it still used 3.14. Since 3.13 is current/stable and 3.14 is still preview, shouldn't it prefer 3.13?
>
> It appears uv is just using whatever version ran last.

From uv's perspective, the 3.14 preview fulfills `>=3.13`, so it prefers to reuse the existing environment over reinstalling everything. It does not pick a 3.14 prerelease unless prompted, as prereleases are opt-in. By switching to 3.14 and creating the 3.14 environment for the script, you did an implicit opt-in to that prerelease series, and uv will continue to use it while compatible. 

---

_Comment by @dotysan on 2025-05-27 00:16_

Thank you for the explanation @konstin.

But here's my situation...

I have a script that should be self-defining. It needs to say "Hi, I'm a script. I need *these* dependencies. I'm compatible with *these* versions. And I want to run on the latest *stable* Python release. Not experimental."

Here's the script in question: https://gist.github.com/dotysan/b5ad0b8296293590fc969236b50dd910

How to tell it to run on the latest stable instead of the oldest compatible Python 3.10 without adding --python to the shebang?

Of course, never default to Python 3.14--until it is considered current/stable.

---

_Comment by @konstin on 2025-05-27 08:34_

There is currently no setting to explicitly select for stable versions.

---
