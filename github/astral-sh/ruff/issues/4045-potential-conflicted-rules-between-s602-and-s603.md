---
number: 4045
title: Potential conflicted rules between S602 and S603
type: issue
state: open
author: zeshuaro
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-04-20T11:45:17Z
updated_at: 2025-05-21T20:08:22Z
url: https://github.com/astral-sh/ruff/issues/4045
synced_at: 2026-01-07T13:12:14-06:00
---

# Potential conflicted rules between S602 and S603

---

_Issue opened by @zeshuaro on 2023-04-20 11:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

After bumping ruff from 0.0.261 to 0.0.262, I'm getting conflicted errors from the rules S602 and S603. See below for the minimal code snippet:

```python
from subprocess import PIPE, Popen

proc = Popen(["/foo/bar"], stdout=PIPE, stderr=PIPE, shell=False)
```

I have been setting `shell=False` as per the S602 rule. However, running this snippet with ruff 0.0.262 errors with the S603 rule, which suggests to set `shell=True`. And if I do that, then the code is failing the S602 rule. Would you be able to check and see if this is intentional?

You can find the GitHub Action logs [here](https://github.com/zeshuaro/telegram-pdf-bot/actions/runs/4753738626/jobs/8445683417?pr=1508) with the above error introduced in 0.0.262.

And you can find my ruff config [here](https://github.com/zeshuaro/telegram-pdf-bot/blob/master/pyproject.toml#L58-L112).

---

_Referenced in [zeshuaro/telegram-pdf-bot#1508](../../zeshuaro/telegram-pdf-bot/pulls/1508.md) on 2023-04-20 11:47_

---

_Comment by @evanrittenhouse on 2023-04-20 15:55_

Happy to look into this! I'm not able to find any S602/S603 rule, though - [here](https://beta.ruff.rs/docs/rules/#flake8-bandit-s) are the `S` rule codes. Could it be under a different code? I could also look at it if you could provide the human readable name.

---

_Comment by @dhruvmanila on 2023-04-20 18:47_

They're here :)

https://github.com/charliermarsh/ruff/blob/cb762f4cad62000e8cd5fa26fec35b228e3bc5d8/crates/ruff/src/rules/flake8_bandit/rules/shell_injection.rs#L252-L253

---

_Comment by @evanrittenhouse on 2023-04-20 18:58_

Gotcha - I can submit a fix to make these mutually exclusive if that makes sense? Seems like they only differ on shell truthiness, so I think that's the easiest way to go

@dhruvmanila  

---

_Comment by @dhruvmanila on 2023-04-20 19:18_

I'm confused between these two as they're the exact opposite of each other. If we try to make them mutually exclusive, then which one to detect first?

I think we should actually look into why these two rules exists in the first place and then decide what the fix should be.

Relevant docs:
* B602: https://bandit.readthedocs.io/en/latest/plugins/b602_subprocess_popen_with_shell_equals_true.html
* B603: https://bandit.readthedocs.io/en/latest/plugins/b603_subprocess_without_shell_equals_true.html

---
(A bit late here so calling it a day :))

---

_Comment by @zeshuaro on 2023-04-20 21:39_

I think this Issue on bandit might be related to the behaviour we’re seeing here with ruff?
https://github.com/PyCQA/bandit/issues/333

---

_Comment by @evanrittenhouse on 2023-04-20 21:46_

It think B603 is a lower priority than B602:
>Because this is a lesser issue than that described in subprocess_popen_with_shell_equals_true a LOW severity warning is reported.

If that's the case, in the case where both are enabled we could defer to checking S602 first - if it's triggered, we could short-circuit the S603 check

---

_Comment by @charliermarsh on 2023-04-21 01:57_

Huh, interesting. So I guess we're consistent with Bandit here? And the only workaround within Bandit is to add a `# nosec` or similar, to allow low-risk `subprocess` calls?

I suppose we could consider removing `S603` altogether. Or, we could modify `S603` to only flag calls with dynamic arguments, like variables, and let all-string calls be considered "safe", which seems to be what they suggest in that issue.


---

_Label `question` added by @charliermarsh on 2023-04-21 01:57_

---

_Referenced in [astral-sh/ruff#4054](../../astral-sh/ruff/issues/4054.md) on 2023-04-22 14:00_

---

_Referenced in [astral-sh/ruff#4089](../../astral-sh/ruff/issues/4089.md) on 2023-04-25 15:03_

---

_Comment by @spapanik on 2023-06-29 11:04_

I think that `S603` makes sense if (even one of) the arguments are dynamic, but for static arguments, it doesn't offer much

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:16_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:16_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:16_

---

_Comment by @ofek on 2023-11-12 23:02_

I vote for removal or opt-in since even non-literal statics trigger:

```python
[sys.executable, '-m', 'ruff', 'rule', '--all']
```

---

_Comment by @ThiefMaster on 2023-11-20 16:11_

Yes, this rule (S602) seems completely useless right now. It triggers on a simple `subprocess.check_output(['/usr/bin/foo'])` which has zero non-hardcoded input...

---

_Comment by @NeilGirdhar on 2024-06-17 19:25_

Not sure if this should be a new issue, but I think S602 should also trigger unconditionally for `os.popen` as per [this comment](https://discuss.python.org/t/is-time-to-remove-os-module-spawn-functions/55829/13).

---

_Comment by @dhruvmanila on 2024-06-18 05:04_

@NeilGirdhar I think that's what `S605` does: https://play.ruff.rs/93f469fb-9725-411c-83c0-2c0d04d0ab75

---

_Comment by @NeilGirdhar on 2024-06-18 05:14_

Ah, sorry, for some reason my test missed this.  How do you see the lint errors in the playground?  I can't seem to find them.

---

_Comment by @dhruvmanila on 2024-06-18 05:23_

No worries.

> How do you see the lint errors in the playground? I can't seem to find them.

What do you mean? Do you not see any squiggly lines? You'll need to enable to rules in the settings tab which is on the left sidebar. And, you might also need to enable preview mode if the rule isn't stabilized yet. 

---

_Comment by @NeilGirdhar on 2024-06-18 05:27_

Ah, right, I need to hover, my mistake.  Thanks!

---

_Referenced in [reef-technologies/cookiecutter-rt-django#194](../../reef-technologies/cookiecutter-rt-django/pulls/194.md) on 2024-07-23 08:49_

---

_Referenced in [neuralmagic/nm-actions#32](../../neuralmagic/nm-actions/pulls/32.md) on 2024-09-30 18:35_

---

_Comment by @david-mcdowell-ilw on 2025-05-21 20:08_

 Any plans for this?

---

_Referenced in [BfArM-MVH/grz-tools#355](../../BfArM-MVH/grz-tools/pulls/355.md) on 2025-08-15 12:14_

---
