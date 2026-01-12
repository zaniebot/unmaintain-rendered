```yaml
number: 3361
title: Rule E261 is missing even though version 0.0.244 is supposed to have introduced it
type: issue
state: closed
author: rleconte
labels: []
assignees: []
created_at: 2023-03-06T16:25:32Z
updated_at: 2023-03-06T16:45:02Z
url: https://github.com/astral-sh/ruff/issues/3361
synced_at: 2026-01-12T15:54:43Z
```

# Rule E261 is missing even though version 0.0.244 is supposed to have introduced it

---

_@rleconte_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
[Release notes for v0.0.244](https://github.com/charliermarsh/ruff/releases/tag/v0.0.244) mention that several rules have been added, such as E261.

But when running `ruff rule E261` I get the following output:
```
❯ ruff rule E261           
error: invalid value 'E261' for '<RULE>': unknown rule code

For more information, try '--help'.
```
I tried with both `ruff 0.0.254` and `ruff 0.0.244`

And indeed, this rule is not checked when running ruff, with the following file (`E261.py`):
``` E261.py
print("test") # Test comment with only one whitespace before it
```
```
❯ ruff check --select E --isolated E261.py
```

What am I missing here?

Thanks in advance.

Romain

---

_Comment by @charliermarsh on 2023-03-06 16:26_

Hey sorry! I mentioned this in #3355 so I'll just copy over that response for now:

> Ah yeah, sorry about that. This is just a confusion in messaging -- we've implemented those rules, but they're currently behind a feature flag, so they're not accessible from the production build.

> I want all of the remaining pycodestyle rules to go out in a single coordinated release, since it's arguably a 'breaking' change for users (it'll enable a bunch of previously non-existent rules for any users that enable the `E` category).

> There are about 10 more rules to go, hopefully in the next week or two but don't hold me to it :)


---

_Closed by @charliermarsh on 2023-03-06 16:26_

---

_Comment by @rleconte on 2023-03-06 16:44_

I was about to say that I noticed they were missing from the [docs here](https://beta.ruff.rs/docs/rules/#pycodestyle-e-w), so it is intended behavior that they are not enabled yet :slightly_smiling_face:  
(And sorry for missing the already filed issue :see_no_evil: )

Which section of the changelog should I watch to notice when they are activated? The `Rules` section?

Thanks for the super quick reply!

---

_Comment by @charliermarsh on 2023-03-06 16:45_

Yeah the `Rules` section. I'll probably be pretty loud about it when they're enabled since it'll be a big change for some setups :)

---
