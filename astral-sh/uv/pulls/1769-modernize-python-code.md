```yaml
number: 1769
title: Modernize Python code
type: pull_request
state: closed
author: gaborbernat
labels: []
assignees: []
base: main
head: modernize
created_at: 2024-02-20T17:23:37Z
updated_at: 2024-02-20T19:51:18Z
url: https://github.com/astral-sh/uv/pull/1769
synced_at: 2026-01-12T16:04:43Z
```

# Modernize Python code

---

_@gaborbernat_

Use f-strings, walrus operator and raise SystemExit instead of sys.exit.

Signed-off-by: Bernát Gábor <bgabor8@bloomberg.net>

---

_@AlexWaygood reviewed on 2024-02-20 17:31_

---

_Review comment by @AlexWaygood on `python/uv/__main__.py`:39 on 2024-02-20 17:31_

Hmm, I kinda prefer the `if`/`else` here -- I feel like it makes it more explicit that it's a binary choice: for Windows we do one thing; for all other platforms, we do a different thing

---

_@gaborbernat reviewed on 2024-02-20 17:34_

---

_Review comment by @gaborbernat on `python/uv/__main__.py`:39 on 2024-02-20 17:34_

But raise SystemExit makes it redundant, 🤔 so I prefer this 😆 the raise makes it explicit that the execution of that branch ends there.

---

_@AlexWaygood reviewed on 2024-02-20 17:39_

---

_Review comment by @AlexWaygood on `python/uv/__main__.py`:39 on 2024-02-20 17:39_

I agree with you that calling `sys.exit` is redundant, since all it does internally is raise `SystemExit`, so I too prefer raising `SystemExit` directly as a matter of style. But I'd still personally find it more readable, in this specific instance, to do this:

```py
    if sys.platform == "win32":
        import subprocess

        completed_process = subprocess.run(cmd, env=env)
        raise SystemExit(completed_process.returncode)
    else:
        os.execvpe(uv, cmd, env=env)
```

even though, as you say, the `else` is, strictly speaking, redundant.

Make a bikesheddy PR, expect bikesheddy review comments ;)

---

_Comment by @charliermarsh on 2024-02-20 17:42_

I think we're _technically_ 3.7-compatible right now, so we may have to decide whether that's correct or not.

---

_Comment by @gaborbernat on 2024-02-20 17:50_

> I think we're _technically_ 3.7-compatible right now, so we may have to decide whether that's correct or not.

https://github.com/astral-sh/uv/blob/main/pyproject.toml#L10 seems to strongly disagree 😆 any modern installer would refuse installing the package in 3.7 because of that.

---

_Review comment by @gaborbernat on `python/uv/__main__.py`:39 on 2024-02-20 17:51_

I prefer my style, but if there's one more maintainer vote on the previous form can revert 👍 

---

_@gaborbernat reviewed on 2024-02-20 17:51_

---

_Comment by @zanieb on 2024-02-20 18:06_

We should be as backwards compatible as possible here. I do not think we should make these cosmetic changes.

---

_Comment by @gaborbernat on 2024-02-20 18:14_

Backwards compatible to what? Python 1.0? As pointed out above, the pyproject.toml metadata already makes you incompatible with anything before 3.8 for any installer that's not 5+ years old…

---

_Comment by @zanieb on 2024-02-20 19:08_

We do not want to encourage use of Python 3.7, but we _are_ compatible except for that line preventing installation with it. I think we should consider explicitly breaking runtime compatibility separately.

Separate from the compatibility comments, I'm unwilling to make stylistic changes to such critical code unless accompanied by other necessary changes — the risk of breaking things for our users is not worth it.

I hope this doesn't come across as harsh, this just isn't the time for us to be reviewing these kinds of changes. We're focused on fixing bugs and improving correctness to unblock new users.

---

_Closed by @zanieb on 2024-02-20 19:08_

---

_Comment by @gaborbernat on 2024-02-20 19:15_

![](https://media2.giphy.com/media/ETzdvyeIuYYvrV4d7V/giphy.gif?cid=36b14facvls3s8rnoqk19abb3nj64h19b009yj1qz23pzoxx&ep=v1_gifs_search&rid=giphy.gif&ct=g)

Fine, :-) I'll go away then.

---

_Comment by @zanieb on 2024-02-20 19:38_

We do appreciate you!

---

_Comment by @notatallshaw on 2024-02-20 19:50_

FYI just two days ago Pip dropped functional and runtime support for Python 3.7: https://github.com/pypa/pip/pull/11944

The motivating factor there was vendoring other Python packages. Something uv doesn't have to worry about I guess.

---
