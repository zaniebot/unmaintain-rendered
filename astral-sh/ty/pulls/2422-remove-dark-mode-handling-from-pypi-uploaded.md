```yaml
number: 2422
title: Remove dark-mode handling from PyPI-uploaded README
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/read
created_at: 2026-01-09T19:17:13Z
updated_at: 2026-01-09T20:35:29Z
url: https://github.com/astral-sh/ty/pull/2422
synced_at: 2026-01-10T02:34:11Z
```

# Remove dark-mode handling from PyPI-uploaded README

---

_Pull request opened by @charliermarsh on 2026-01-09 19:17_

## Summary

This matches the approach we use in Ruff.

Closes https://github.com/astral-sh/ty/issues/2322.


---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-09 19:19_

---

_Review requested from @zanieb by @charliermarsh on 2026-01-09 19:19_

---

_Comment by @zanieb on 2026-01-09 19:23_

Wait what? I thought I tested this by just using a light-mode image and GitHub automatically switches the text rendering in dark mode.

If I go to PyPI, I can see the text fine?

<img width="1364" height="770" alt="Screenshot 2026-01-09 at 1 22 55 PM" src="https://github.com/user-attachments/assets/3f8242a8-9476-41f7-9a7c-7c9b41142e3a" />


---

_Comment by @charliermarsh on 2026-01-09 19:25_

You can't see it if you enable dark mode:

<img width="3584" height="2088" alt="Screenshot 2026-01-09 at 2 24 57 PM" src="https://github.com/user-attachments/assets/2265fb11-a240-4cda-9ecf-65f2fc9277fe" />


---

_Comment by @charliermarsh on 2026-01-09 19:25_

Because our media query will switch the text to white, but the rest of the PyPI page doesn't adapt.

---

_Comment by @zanieb on 2026-01-09 19:25_

Note I intentionally removed the complicated handling from Ruff because it wasn't necessary, so please wait to merge this.

---

_Comment by @zanieb on 2026-01-09 19:26_

There is no media query though? It's just `<img alt="Shows a bar chart with benchmark results." width="500px" src="./docs/assets/ty-benchmark-cli.svg">`?

You're referring to the embedded query?

---

_Comment by @zanieb on 2026-01-09 19:27_

You're saying this occurs if you visit PyPI with dark mode enabled?

---

_Comment by @charliermarsh on 2026-01-09 19:27_

> You're saying this occurs if you visit PyPI with dark mode enabled?

Yes.

> You're referring to the embedded query?

Yeah, there's CSS _within_ `ty-benchmark-cli.svg`. I removed it in this PR.

---

_Comment by @zanieb on 2026-01-09 19:27_

Thanks for clarifying. I missed https://github.com/astral-sh/ty/pull/2422#issuecomment-3730282665 

---

_Comment by @zanieb on 2026-01-09 19:29_

I think I'd prefer to retain the adaptive image and add a static light image and just replace the single URL on publish? Then we don't need a srcset and don't need to replicate that in the replacement code?

---

_Comment by @charliermarsh on 2026-01-09 19:33_

That's fine with me, I'll do a pass on it.

---

_Comment by @zanieb on 2026-01-09 19:35_

Thank you!

---

_@zanieb approved on 2026-01-09 19:35_

---

_Merged by @charliermarsh on 2026-01-09 20:35_

---

_Closed by @charliermarsh on 2026-01-09 20:35_

---

_Branch deleted on 2026-01-09 20:35_

---
