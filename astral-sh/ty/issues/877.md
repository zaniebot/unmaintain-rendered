```yaml
number: 877
title: "`ty` markdown badge"
type: issue
state: closed
author: jorenham
labels:
  - wish
assignees: []
created_at: 2025-07-23T16:00:58Z
updated_at: 2025-07-28T19:35:09Z
url: https://github.com/astral-sh/ty/issues/877
synced_at: 2026-01-10T02:06:24Z
```

# `ty` markdown badge

---

_Issue opened by @jorenham on 2025-07-23 16:00_

I think it would be kinda cool to have a [![ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json)](https://github.com/astral-sh/ruff), but for `ty`. 

---

_Label `wish` added by @dhruvmanila on 2025-07-24 04:06_

---

_Comment by @MichaReiser on 2025-07-24 08:56_

I never created a markdown badge before. Do you know how to create one? I'll ask internally if not.

---

_Comment by @jorenham on 2025-07-24 11:13_

I suppose you could follow `ruff`'s approach, which uses a simple json (https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json) for this

---

_Comment by @MichaReiser on 2025-07-24 12:46_

Oh nice. We could probably use the svg logo from here https://github.com/astral-sh/ruff/blob/9461d3076f611a81b7aba88cfcee5b85e08abf8c/playground/shared/src/Header.tsx#L99-L128

Do you want to give it a try and open a PR to add the badge to ty's README?

---

_Comment by @jorenham on 2025-07-24 13:03_

> Do you want to give it a try and open a PR to add the badge to ty's README?

Sure :)

---

_Comment by @jorenham on 2025-07-24 18:35_

How about ![ty](https://img.shields.io/badge/ty-checked-261230.svg?logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDgiIGhlaWdodD0iNDgiIHZpZXdCb3g9IjAgMCA0OCA0OCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTQ4IDcuNjhIMjcuODRWMEgzLjg0VjcuNjhIMFYyNS45MkgzLjg0VjQwLjIxMzZDMy44NCA0NC41MTM5IDcuMzI2MDcgNDggMTEuNjI2NCA0OEg0OFYyOS43NkgyNy44NFYyNS45Mkg0MC4yMTM2QzQ0LjUxMzkgMjUuOTIgNDggMjIuNDMzOSA0OCAxOC4xMzM2VjcuNjhaIiBmaWxsPSIjNDZFQkUxIi8+Cjwvc3ZnPgo=), or perhaps something like ![ty](https://img.shields.io/badge/ty-checked-0b0e14.svg?labelColor=30173d&logo=data:image/svg+xml;base64,PHN2ZwogIHdpZHRoPSI0OCIKICBoZWlnaHQ9IjQ4IgogIHZpZXdCb3g9IjAgMCA0OCA0OCIKICBmaWxsPSJub25lIgogIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIKPgogIDxwYXRoCiAgICBkPSJNNDggNy42OEgyNy44NFYwSDMuODRWNy42OEgwVjI1LjkySDMuODRWNDAuMjEzNkMzLjg0IDQ0LjUxMzkgNy4zMjYwNyA0OCAxMS42MjY0IDQ4SDQ4VjI5Ljc2SDI3Ljg0VjI1LjkySDQwLjIxMzZDNDQuNTEzOSAyNS45MiA0OCAyMi40MzM5IDQ4IDE4LjEzMzZWNy42OFoiCiAgICBmaWxsPSIjRDdGRjY0IgogIC8+Cjwvc3ZnPgo=)?

---

_Comment by @MichaReiser on 2025-07-25 06:21_

Nice. I would go with the first style and simply use `logo | ty`. This is what we have for ruff and uv too. That allows us to increase the scope of ty long term ;) Regard

---

_Comment by @jorenham on 2025-07-25 15:01_

![ty](https://img.shields.io/badge/-ty-261230.svg?logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDgiIGhlaWdodD0iNDgiIHZpZXdCb3g9IjAgMCA0OCA0OCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTQ4IDcuNjhIMjcuODRWMEgzLjg0VjcuNjhIMFYyNS45MkgzLjg0VjQwLjIxMzZDMy44NCA0NC41MTM5IDcuMzI2MDcgNDggMTEuNjI2NCA0OEg0OFYyOS43NkgyNy44NFYyNS45Mkg0MC4yMTM2QzQ0LjUxMzkgMjUuOTIgNDggMjIuNDMzOSA0OCAxOC4xMzM2VjcuNjhaIiBmaWxsPSIjNDZFQkUxIi8+Cjwvc3ZnPgo=) ![ty](https://img.shields.io/badge/-ty-0b0e14.svg?labelColor=30173d&logo=data:image/svg+xml;base64,PHN2ZwogIHdpZHRoPSI0OCIKICBoZWlnaHQ9IjQ4IgogIHZpZXdCb3g9IjAgMCA0OCA0OCIKICBmaWxsPSJub25lIgogIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIKPgogIDxwYXRoCiAgICBkPSJNNDggNy42OEgyNy44NFYwSDMuODRWNy42OEgwVjI1LjkySDMuODRWNDAuMjEzNkMzLjg0IDQ0LjUxMzkgNy4zMjYwNyA0OCAxMS42MjY0IDQ4SDQ4VjI5Ljc2SDI3Ljg0VjI1LjkySDQwLjIxMzZDNDQuNTEzOSAyNS45MiA0OCAyMi40MzM5IDQ4IDE4LjEzMzZWNy42OFoiCiAgICBmaWxsPSIjRDdGRjY0IgogIC8+Cjwvc3ZnPgo=)

Hmm, I guess it could be read as "tty" this way 🤔 

But I like the minimal look :)

I'll try and work on PR for the first one later today

---

_Closed by @MichaReiser on 2025-07-28 19:35_

---
