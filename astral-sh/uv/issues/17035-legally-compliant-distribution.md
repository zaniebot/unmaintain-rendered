```yaml
number: 17035
title: Legally compliant distribution ?
type: issue
state: open
author: zvr
labels:
  - question
assignees: []
created_at: 2025-12-08T17:31:36Z
updated_at: 2025-12-12T14:10:50Z
url: https://github.com/astral-sh/uv/issues/17035
synced_at: 2026-01-10T03:11:35Z
```

# Legally compliant distribution ?

---

_Issue opened by @zvr on 2025-12-08 17:31_

### Question

I've seen many ways to install `uv` in a container layer, so that it's further re-distributed:

- `pip install uv`
- `curl -LsSf https://astral.sh/uv/install.sh | sh`
- `COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/`
- etc. etc.

Unfortunately, all these result in having the `uv` executable, but none of the artifacts that are legally required to be able to redistribute it.

`uv` itself is licensed under `Apache-2.0 OR MIT` (to use an SPDX expression) and both of these licenses have the obligation to also include the license text alongside the executable. The MIT license also introduces the obligation to include the copyright notice.

Furthermore, the executable contains dozens of other modules (crates), each of them under its own license (usually the same `Apache-2.0 OR MIT`, but I have not checked them all). For each one of these, obligations also exist for redistribution.
For example, taking one of the dependencies in random, the `spdx` crate under the `MIT` license requires to somehow include its copyright notice (`"Copyright (c) 2019 Embark Studios"`) when redistributing it.

Is there a "distribution" of `uv` that includes all this information, so that it makes re-distribution easier?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @zvr on 2025-12-08 17:31_

---

_Comment by @charliermarsh on 2025-12-11 15:23_

Do you have examples of other tools that satisfy what you're looking for?


---

_Comment by @zvr on 2025-12-12 14:10_

Well, all Debian and Ubuntu packages traditionally provide this information, by placing files (especially one named "`copyright`") under `/usr/share/doc`. 

Chromium releases include an HTML file with all license texts and attributions. Whenever an Electron app is created, a "LICENSES.chromium.html" file is added, which is a copy of these.

I am sure that, whatever phone you are using, there will be a menu entry (under Settings/About/...) that will show all the required legal texts. Android apps usually follow [guidance](https://developers.google.com/android/guides/opensource) from Google on this.

Actually, I now see that `pip install uv` _does_ store the uv license texts (Apache and MIT) in `site-packages/uv-*.dist-info/licenses`, so it only needs to add another file there (or a collection of files).

If you want, I can definitely help a little —although I am merely a user of `uv` and have not (yet) looked at its code or its release process. The interesting part is building some kind of automation, so the file is generated automagically. There is no need to reinvent everything; tools like [this](https://crates.io/crates/licenses) already exist.

---
