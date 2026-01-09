---
number: 21203
title: Allow alias/alternative for noqa comments
type: issue
state: closed
author: kasium
labels: []
assignees: []
created_at: 2025-11-02T11:05:08Z
updated_at: 2025-11-02T15:52:18Z
url: https://github.com/astral-sh/ruff/issues/21203
synced_at: 2026-01-07T13:12:16-06:00
---

# Allow alias/alternative for noqa comments

---

_Issue opened by @kasium on 2025-11-02 11:05_

In my repository, I would like to start using ruff, but I face the following issue
* ruff uses noqa comments for line local disables
* I have a local flake8 plugin which adds certain checks
* I use yesqa to find unnecessary noqa comments

This combination is not possible to use, as yesqa will remove noqa comments from ruff. As the maintainer of yesqa/flake8 claim a "kind of ownership" of the noqa comment and won't implement something, it would be great if ruff could allow an alias for noqa.

---

_Comment by @Daverball on 2025-11-02 14:34_

@kasium I can relate to your problem, I've mostly transitioned to ruff at this point, but have to still rely on a small number of flake8 plugins to fill in some gaps.

While I agree that it would be nice to keep something like `yesqa`/`flake8-noqa` functional in this hybrid setup, it's also true that if the number of codes you still need flake8 for is low, then the need for `yesqa` or `flake8-noqa` should also be low enough, that it's a reasonable trade-off to just not have it for those few external rules. All of the other unused noqa comments will be covered by ruff. I don't think this issue is large enough, that it warrants ruff having to work around the unwillingness of flake8 plugin authors to implement the equivalent of the [`external`](https://docs.astral.sh/ruff/settings/#lint_external) setting to `yesqa`/`flake8-noqa`.

If you do decide that you still need it for the remaining flake8 rules you can't move to ruff, you can always fork `yesqa` or `flake8-noqa` and implement that feature yourself. In fact if you're fine with using `flake8-noqa` instead of `yesqa` (you'd lose out on the automatic fix), then @Avasam already has done most of the work for you: https://github.com/plinss/flake8-noqa/pull/30

But I imagine it would be very easy to add the equivalent feature to a fork of `yesqa` as well.

---

_Comment by @kasium on 2025-11-02 15:52_

Thanks a lot for the long and comprehensive reply. I completely see your points and don't want to make my corner-case bigger than it is 😊

---

_Closed by @kasium on 2025-11-02 15:52_

---
