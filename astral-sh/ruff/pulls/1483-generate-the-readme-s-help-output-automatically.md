```yaml
number: 1483
title: "Generate the README's --help output automatically via cargo +nightly dev generate-all"
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: auto-generate-cli-help-in-readme
created_at: 2022-12-30T18:14:32Z
updated_at: 2022-12-31T03:52:47Z
url: https://github.com/astral-sh/ruff/pull/1483
synced_at: 2026-01-12T15:55:06Z
```

# Generate the README's --help output automatically via cargo +nightly dev generate-all

---

_@squiddy_

Closes: #1474

---

_Renamed from "Auto generate cli help in readme" to "Generate the README's --help output automatically via cargo +nightly dev generate-all" by @squiddy on 2022-12-30 18:28_

---

_@charliermarsh reviewed on 2022-12-30 18:30_

---

_Review comment by @charliermarsh on `README.md`:303 on 2022-12-30 18:30_

It looks like these are appearing in the rendered Markdown.

---

_@charliermarsh reviewed on 2022-12-30 18:30_

---

_Review comment by @charliermarsh on `README.md`:303 on 2022-12-30 18:30_

![Screen Shot 2022-12-30 at 1 30 31 PM](https://user-images.githubusercontent.com/1309177/210101734-3e81ef90-2ed4-4f50-9821-1882ae50a4af.png)

https://github.com/charliermarsh/ruff/blob/f3ebb168ef6d9ec5f1de59e0ea78a1fbf13c35f0/README.md

---

_Review comment by @squiddy on `README.md`:303 on 2022-12-30 18:31_

:man_facepalming: Let me fix that.

---

_@squiddy reviewed on 2022-12-30 18:31_

---

_@charliermarsh reviewed on 2022-12-30 18:31_

---

_Review comment by @charliermarsh on `README.md`:303 on 2022-12-30 18:31_

No prob, I wasn't sure what would happen 😂 

---

_@charliermarsh reviewed on 2022-12-30 18:38_

---

_Review comment by @charliermarsh on `README.md`:316 on 2022-12-30 18:38_

Any idea where all the newlines are coming from?

---

_Review comment by @squiddy on `README.md`:316 on 2022-12-30 18:53_

Good point, I'll go looking.

---

_@squiddy reviewed on 2022-12-30 18:53_

---

_@squiddy reviewed on 2022-12-30 20:03_

---

_Review comment by @squiddy on `README.md`:303 on 2022-12-30 20:03_

Fixed it.

---

_Comment by @charliermarsh on 2022-12-30 20:04_

LGTM!

---

_@squiddy reviewed on 2022-12-30 20:04_

---

_Review comment by @squiddy on `README.md`:316 on 2022-12-30 20:04_

Just got back from my deep-dive into clap.

In the end, I just had to go from `render_long_help` to `render_help`. :sweat_smile:

There is some trailing whitespace after the `FILES` lines, but I don't think that's an issue.

---

_@charliermarsh reviewed on 2022-12-30 20:06_

---

_Review comment by @charliermarsh on `README.md`:316 on 2022-12-30 20:06_

All good. Thanks again :)

---

_Merged by @charliermarsh on 2022-12-30 20:06_

---

_Closed by @charliermarsh on 2022-12-30 20:06_

---

_Branch deleted on 2022-12-30 20:06_

---

_Review comment by @squiddy on `README.md`:316 on 2022-12-30 21:23_

@charliermarsh I've noticed that the trailing whitespace seems to cause issues. In https://github.com/charliermarsh/ruff/commit/74903f23d69172474116ec3e05d182cdf9179415 it got removed, which fails CI again. Shall we adjust the cmd to strip trailing whitespace from the generated output?

---

_@squiddy reviewed on 2022-12-30 21:23_

---

_@charliermarsh reviewed on 2022-12-30 21:50_

---

_Review comment by @charliermarsh on `README.md`:316 on 2022-12-30 21:50_

Hmm yeah, I guess so. I wonder how it got removed? Is it machine-specific? Or did my editor do it automatically, or something?

---

_@charliermarsh reviewed on 2022-12-30 22:43_

---

_Review comment by @charliermarsh on `README.md`:316 on 2022-12-30 22:43_

Are you working on this? I could do it later if needed.

---

_@charliermarsh reviewed on 2022-12-31 03:52_

---

_Review comment by @charliermarsh on `README.md`:316 on 2022-12-31 03:52_

I got it :)

---
