```yaml
number: 1192
title: Check for outdated auto-generated files in CI
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: ci-check-for-outdated-files
created_at: 2022-12-11T07:54:18Z
updated_at: 2022-12-13T19:24:29Z
url: https://github.com/astral-sh/ruff/pull/1192
synced_at: 2026-01-12T15:55:05Z
```

# Check for outdated auto-generated files in CI

---

_@squiddy_

I've noticed that sometimes those generated files become out of sync with the code. I've adjusted the CI workflow to put notice messages on those files if it detects changes.

Haven't worked with github workflows before, so there might be some nicer way. *If* this is even interesting, otherwise don't hestitate to close. :)

![image](https://user-images.githubusercontent.com/50333/206892338-6962e7d3-8272-45dd-8ec6-4ac65cf36843.png)


---

_Comment by @charliermarsh on 2022-12-11 14:19_

Thank you, I desperately want this!

---

_Comment by @charliermarsh on 2022-12-11 14:19_

(Does the CI task fail if the diff surfaces differences?)

---

_Comment by @squiddy on 2022-12-11 14:22_

It doesn't yet, I wasn't sure how much you would like to escalate this, hence the warning. I'll change it accordingly.

---

_Comment by @charliermarsh on 2022-12-11 14:25_

I guess ideally it’d diff every file, then fail the task. But failing as soon as it hits a “bad” diff is also fine.

---

_Comment by @charliermarsh on 2022-12-11 14:25_

(If it doesn’t fail the task I’ll probably miss it at least once :))

---

_Comment by @squiddy on 2022-12-11 15:14_

@charliermarsh It now fails the job when there are changes. I added a temporary 2nd commit to show that.

---

_Comment by @charliermarsh on 2022-12-11 15:18_

Awesome, thank you.

---

_Merged by @charliermarsh on 2022-12-11 15:18_

---

_Closed by @charliermarsh on 2022-12-11 15:18_

---

_Comment by @charliermarsh on 2022-12-11 15:19_

It's worth saying aloud that all of your PRs have been so good and hugely appreciated.

---

_Comment by @squiddy on 2022-12-11 15:25_

Thank you. Happy to contribute and learn. It's really awesome to get your quick feedback, that's very encouraging.

You merged the `REMOVE ME` commit though, so now the main pipeline will fail. :innocent: 

---

_Comment by @charliermarsh on 2022-12-11 15:34_

Way to go, me :)

---

_Comment by @JonathanPlasse on 2022-12-11 18:01_

@charliermarsh, would you be interested in adding a pre-commit hook that checks if the auto-generated files are up to date?

---

_Branch deleted on 2022-12-13 19:24_

---
