---
number: 7075
title: "error: Failed to prepare distributions, Caused by: Failed to fetch wheel: sentence-transformers==2.2.2"
type: issue
state: closed
author: B-M-S-West
labels:
  - question
assignees: []
created_at: 2024-09-05T10:34:40Z
updated_at: 2024-10-21T21:43:07Z
url: https://github.com/astral-sh/uv/issues/7075
synced_at: 2026-01-07T13:12:17-06:00
---

# error: Failed to prepare distributions, Caused by: Failed to fetch wheel: sentence-transformers==2.2.2

---

_Issue opened by @B-M-S-West on 2024-09-05 10:34_

I'm currently setting up a new project using uv following using
uv init
uv version = 0.4.5
python version = 3.11
windows 11

When trying to add graphrag
uv add graphrag

I get the following error:
$ uv add graphrag
Resolved 242 packages in 785ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: sentence-transformers==2.2.2
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit code: 1

Some further information printed out:
        ********************************************************************************
        Usage of dash-separated 'description-file' will not be supported in future
        versions. Please use the underscore name 'description_file' instead.

        By 2024-Sep-26, you need to update your project and remove deprecated calls
        or your builds will no longer be supported.

        See https://setuptools.pypa.io/en/latest/userguide/declarative_config.html for details.
        ********************************************************************************

!!
  opt = self.warn_dash_deprecation(opt, section)
error: could not create 'build\bdist.win-amd64\wheel\.\sentence_transformers\cross_encoder\evaluation\CEBinaryAccuracyEvaluator.py': No such file or directory
---

Any ideas on the issue and how I can resolve it?

---

_Comment by @charliermarsh on 2024-09-05 13:19_

Are you able to `pip install sentence-transformers==2.2.2 --use-pep517`? Or does that also show an error?

---

_Label `question` added by @charliermarsh on 2024-09-05 13:19_

---

_Comment by @B-M-S-West on 2024-09-05 14:49_

I was able to use pip install to get it working and it would appear in the uv.lock
Since then everytime I use uv add for another depenency the same error will come up as before, as it tries to add sentence-transformers to the dependency of the pyproject.toml?

---

_Comment by @jimmy3142 on 2024-09-25 22:04_

I'm also having a similar issue with many other dependencies. Anything else you can suggest here @charliermarsh?

---

_Comment by @charliermarsh on 2024-09-25 22:12_

Can you elaborate on the issue you're seeing? The commands you're running, the error you're seeing, how I might reproduce it, etc.

---

_Comment by @jimmy3142 on 2024-10-18 02:15_

I solved my issue. Sorry for the delayed reply. I didn't want to hijack the thread, as it was a slightly different issue.

---

_Closed by @zanieb on 2024-10-21 21:43_

---
