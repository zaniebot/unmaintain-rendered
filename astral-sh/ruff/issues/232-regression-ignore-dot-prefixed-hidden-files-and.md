```yaml
number: 232
title: "Regression: Ignore dot-prefixed hidden files and folders by default"
type: issue
state: closed
author: amotl
labels: []
assignees: []
created_at: 2022-09-20T14:32:08Z
updated_at: 2022-09-20T15:28:29Z
url: https://github.com/astral-sh/ruff/issues/232
synced_at: 2026-01-12T15:54:40Z
```

# Regression: Ignore dot-prefixed hidden files and folders by default

---

_@amotl_

Dear Charlie,

after upgrading from 0.0.37 to 0.0.40, `ruff` started including dot-prefixed folders like `.tox`, `.venv`, `.venv36` in my working tree of [mqttwarn](https://github.com/jpmens/mqttwarn) into the scan, significantly increasing the overall execution time from ~0.3s to ~5.5s. This is probably coming from the adjustments at #209 and/or #213?

I think that #174 will be a sweet feature, which will indirectly mitigate the problem, at least for my scenario. However, I still believe that dot-prefixed files and folders should be excluded by default, no matter what. Saying that, I don't know the exact behavior of other similar tools, and whether they provide a separate option to _include_ scanning dot-prefixed files and folders on purpose, or not - I merely wanted to humbly share my immediate reaction about this with you.

With kind regards,
Andreas.

P.S.: After upgrading to ruff 0.0.42, the overall execution time went down to ~2.1s again, but the behavior to include scanning `.tox` and `.venv*` folders by default, is still present.

---

_Renamed from "Regression: Ignore dot-prefixed files and folders by default" to "Regression: Ignore dot-prefixed hidden files and folders by default" by @amotl on 2022-09-20 14:41_

---

_Comment by @Jackenmen on 2022-09-20 14:46_

This comes from my request in #176 where you can see the reasoning behind it. There have been a bunch of PRs made to the exclusion logic recently so it's been somewhat broken for a few releases. But this change was technically made in #188.

I think that improving the default exclusions to be `.venv*` rather than `.venv` would be welcome (I personally use `.venvs` which is uncommon but I've seen people use `.venv3x` somewhat commonly whenever they need to check something with additional Python version) though unprecedented by other tools (to my knowledge). My opinion on hidden files should probably be clear from #176 so not going to repeat my reasoning from there :) 

---

_Comment by @amotl on 2022-09-20 15:17_

Dear Jakub,

thank you for taking the time to respond. Apparently, I completely missed your post at #176, apologies. Now everything starts making sense. On top of that, I see that the new `DEFAULT_EXCLUDE` list was implemented with #188, and that it includes `.tox`, `.venv`, and `build` already. Sweet!

https://github.com/charliermarsh/ruff/blob/09b926fd59115afc8f47c191b86ad7c47c79889e/src/settings.rs#L54-L76

Now, I am wondering even more why it does not work correctly on my machine. The corresponding `ruff` configuration settings are [mqttwarn/pyproject.toml#L15-L20](https://github.com/jpmens/mqttwarn/blob/302adf08762b8f14ba36c2f54b9d8c95f4db03c3/pyproject.toml#L15-L20) and the output when running `time ruff .` on a warm cache is https://gist.github.com/amotl/2c8420cf6ae4970a523fab61696b7fc7, which clearly shows the program is scanning all the hidden folders it should have excluded through its `DEFAULT_EXCLUDE` list. I am sure I am doing something wrong here?

With kind regards,
Andreas.


---

_Comment by @charliermarsh on 2022-09-20 15:19_

Can you try changing `exclude` to `extend-exclude` in your `pyproject.toml`?

---

_Comment by @charliermarsh on 2022-09-20 15:20_

(Using `exclude` will override that default list; using `extend-exclude` will augment it.)

---

_Comment by @amotl on 2022-09-20 15:21_

Oh sorry. Using `extend-exclude` works great. Thank you very much, and apologies for missing that.

---

_Closed by @amotl on 2022-09-20 15:21_

---

_Comment by @charliermarsh on 2022-09-20 15:23_

Phew! Is it down to ~0.3s again?

---

_Comment by @amotl on 2022-09-20 15:28_

Ha ;]. Down to 0.210s / 0.195s / 0.183s again, on three consecutive invocations. With #233, or when manually excluding `.venv36`, it can go down to 0.095s.

---
