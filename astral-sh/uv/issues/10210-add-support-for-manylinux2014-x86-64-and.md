```yaml
number: 10210
title: Add support for manylinux2014_x86_64 and manylinux2014_aarch64
type: issue
state: closed
author: Pixel-Minions
labels:
  - question
assignees: []
created_at: 2024-12-28T01:58:30Z
updated_at: 2024-12-29T02:17:15Z
url: https://github.com/astral-sh/uv/issues/10210
synced_at: 2026-01-10T04:36:21Z
```

# Add support for manylinux2014_x86_64 and manylinux2014_aarch64

---

_Issue opened by @Pixel-Minions on 2024-12-28 01:58_

Hi,

I am currently working with lambdas in AWS and deploying some layers using UV, but sadly, it doesnt support `manylinux2014_x86_64` and `manylinux2014_aarch64`, it is possible to add them?

Thank you for the amazing project.

Best,

---

_Comment by @charliermarsh on 2024-12-28 14:36_

I believe we do support these targets. What issue are you seeing exactly?

---

_Label `question` added by @charliermarsh on 2024-12-28 14:36_

---

_Comment by @Pixel-Minions on 2024-12-28 18:55_

Hi @charliermarsh, it is not supported, please take a look:

![Image](https://github.com/user-attachments/assets/19be44aa-898d-4042-af31-a99a99f3f9a9)

This is the lambdas documentation: https://docs.aws.amazon.com/lambda/latest/dg/python-package.html

---

_Comment by @shauneccles on 2024-12-29 00:25_

What happens if you drop the --python-platform and allow uv to try and determine the platform to install?

---

_Comment by @charliermarsh on 2024-12-29 00:31_

You want `x86_64-manylinux_2_17`. That's equivalent to `manylinux2014`.

---

_Comment by @charliermarsh on 2024-12-29 00:32_

I guess we can add aliases for those.

---

_Comment by @Pixel-Minions on 2024-12-29 01:03_

> What happens if you drop the --python-platform and allow uv to try and determine the platform to install?

I am building from windows, zipping and sending to aws for the lambda layer.

---

_Comment by @Pixel-Minions on 2024-12-29 01:04_

> You want `x86_64-manylinux_2_17`. That's equivalent to `manylinux2014`.

That is great, thank you for letting me know, and yes maybe a alias would help to make it clear without having to dig deeper.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-29 01:12_

---

_Closed by @charliermarsh on 2024-12-29 01:36_

---

_Comment by @Pixel-Minions on 2024-12-29 02:17_

Fantastic, thank you @charliermarsh 

---
