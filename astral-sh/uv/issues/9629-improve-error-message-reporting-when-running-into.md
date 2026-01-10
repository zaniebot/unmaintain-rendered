---
number: 9629
title: Improve error message reporting when running into a certificate issue
type: issue
state: closed
author: kdheepak
labels:
  - good first issue
  - error messages
assignees: []
created_at: 2024-12-04T05:48:38Z
updated_at: 2025-01-23T04:45:22Z
url: https://github.com/astral-sh/uv/issues/9629
synced_at: 2026-01-10T01:24:43Z
---

# Improve error message reporting when running into a certificate issue

---

_Issue opened by @kdheepak on 2024-12-04 05:48_

A colleague of mine ran into this issue when installing a package with `uv`.

![Image](https://github.com/user-attachments/assets/ab68379e-6137-4394-b5a9-048496226381)

And this was part of a larger install script to install a number of conda and python dependencies. And they concluded that this was an issue with the dependencies. 

I made the same assumption as well, and since I had only ran this version on MacOS, I first thought "oh `uv` is resolving dependencies differently on macos and windows". And then when I tested it on my Windows VM, I didn't experience an issue which made me look at the error message more carefully.

Looking at the error message now, it is obvious it is a certificate issue. It says "Failed to download" and "UnknownIssuer". But it was easy to miss for two people (just two data points, I know)

I think the last line where "help: ..." is printed seems to imply it is because of a dependency version issue? And that might have been the source of confusion.

I'm wondering if there are incremental improvements possible here? Maybe `uv` can skip the help when running into a certificate issue? Or print a different help message that points the user to the`uv` docs on how to resolve SSL issues?

Feel free to close this issue if you think it's not worth any changes. After reading the `uv` docs and if I were to run `uv sync` from the command line myself, it would be super clear that this was a certificate issue to me. I'm coming at this from the perspective of making an automated install for a user that doesn't have to think about "conda" or "uv" or any of the dependencies involved to setup a project and would like the help messages to be more precise for a user that is using `uv` for the very first time.

---

_Comment by @zanieb on 2024-12-04 17:28_

Thanks for the report!

@charliermarsh we may not want to show this message for download failures?

We should also probably special case the certificate errors.

---

_Label `error messages` added by @zanieb on 2024-12-04 17:28_

---

_Label `good first issue` added by @zanieb on 2024-12-04 17:28_

---

_Comment by @charliermarsh on 2025-01-22 21:21_

I think we now hint at using `--native-tls` at least?

---

_Closed by @zanieb on 2025-01-23 04:45_

---
