```yaml
number: 897
title: "`ty` badge"
type: pull_request
state: merged
author: jorenham
labels:
  - documentation
assignees: []
merged: true
base: main
head: github-badge
created_at: 2025-07-26T12:42:54Z
updated_at: 2025-07-28T19:41:34Z
url: https://github.com/astral-sh/ty/pull/897
synced_at: 2026-01-12T15:54:27Z
```

# `ty` badge

---

_@jorenham_

It's a bit of a chicken/egg story, but it won't work until after its merged. But if you replace the json url with one to a little gist of mine, you can get a sneak peak:

[![ty](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/jorenham/9041990267c10a80730c98bd8b1beef0/raw/c8b626d25c4cc498727cf43803be290e1182c122/v0.json)](https://github.com/astral-sh/ty)

I kept it as close as I could to [ruff's badge](https://github.com/astral-sh/ruff#show-your-support). I slightly modified the [ty svg](https://github.com/astral-sh/ty/blob/main/docs/assets/logo-letter.svg) (removed the redundant `viewbox` and `fill` attributes, and did some modest path rounding) to shave off a couple of precious bytes and to make the json somewhat less unreadable.

Anyway, I have no strong personal opinions about it, so don't hestitate to let me know what you think about it.

---

Closes #877


---

_Comment by @jorenham on 2025-07-26 12:55_

Oh yea, I wasn't sure if it should link to https://github.com/astral-sh/ty (as it now does) or to https://ty.dev. Ruff links to github, but https://ty.dev is quite a bit shorter and seems to (currently?) redirect to github either way. Linking it to ty.dev could perhaps also be handy for analytics or something, but then again, I'm not sure what what your planning to do with it. So, should I change it to ty.dev or keep it pointing github?

---

_Review comment by @MichaReiser on `README.md`:79 on 2025-07-28 07:21_

I know we have it in ruff but I'm leaning towards omitting this section here. Mainly because I don't have an expectations that users show their support this way. I think the badge at the top is enough. 

---

_@MichaReiser approved on 2025-07-28 07:22_

Thank you. This looks great! Using GitHub seems fine to me. 

---

_Label `documentation` added by @MichaReiser on 2025-07-28 07:22_

---

_@jorenham reviewed on 2025-07-28 14:50_

---

_Review comment by @jorenham on `README.md`:79 on 2025-07-28 14:50_

I think you'd be surprised at the number of people that can't wait to start using ty. But I can also see how it could be cleaner to indeed keep it minimal here (too).

---

_@MichaReiser approved on 2025-07-28 19:35_

Thank you. This looks great

---

_Merged by @MichaReiser on 2025-07-28 19:35_

---

_Closed by @MichaReiser on 2025-07-28 19:35_

---

_Branch deleted on 2025-07-28 19:41_

---
