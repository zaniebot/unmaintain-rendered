---
number: 8384
title: Show configuration toml files in tabs within the docs
type: issue
state: closed
author: doolio
labels:
  - documentation
  - good first issue
  - help wanted
assignees: []
created_at: 2023-10-31T17:57:07Z
updated_at: 2023-11-05T17:10:31Z
url: https://github.com/astral-sh/ruff/issues/8384
synced_at: 2026-01-07T13:12:15-06:00
---

# Show configuration toml files in tabs within the docs

---

_Issue opened by @doolio on 2023-10-31 17:57_

To better understand what I mean see [here](https://hatch.pypa.io/latest/config/environment/overview/#environment-configuration) and below in the hatch docs which also uses Material for Mkdocs.

![image](https://github.com/astral-sh/ruff/assets/13859009/573e8730-c72c-4b50-8214-7c5e184f55f9)

It doesn't seem the case in the hatch docs that if you select one such tab that all the tabs that follow automatically select the same tab. I don't know how it is done but that is the case in the hugo docs for example [here](https://gohugo.io/getting-started/configuration/#configuration-directory) and below and it is a really nice feature.

![image](https://github.com/astral-sh/ruff/assets/13859009/0b8b0f8c-0914-44c1-b2ed-1854923f5b49)

I think this would be a great improvement to the `ruff` docs in terms of readability and understanding for users.



---

_Comment by @doolio on 2023-10-31 18:01_

The same style could be used in other areas within the docs too for example the installation instructions as [here](https://rye-up.com/) in the rye docs - again another project using Material for MkDocs.


---

_Comment by @charliermarsh on 2023-10-31 18:19_

This would be great, especially for showing `pyproject.toml` vs. `ruff.toml`.

---

_Label `documentation` added by @charliermarsh on 2023-10-31 18:32_

---

_Comment by @charliermarsh on 2023-10-31 18:35_

mkdocs: https://squidfunk.github.io/mkdocs-material/reference/content-tabs/

---

_Comment by @doolio on 2023-10-31 18:39_

> Content tabs are linked based on their label, not offset. This means that all tabs with the same label will be activated when a user clicks a content tab regardless of order inside a container. 

And this from the page you linked suggests maybe what I described implemented in the hugo docs is possible with Material for MkDocs.

---

_Label `help wanted` added by @MichaReiser on 2023-10-31 21:32_

---

_Comment by @dhruvmanila on 2023-11-01 05:25_

For reference, FastAPI does a lot of this in it's documentation. For example,
* Code: https://github.com/tiangolo/fastapi/blob/master/docs/en/docs/python-types.md#generic-types
* Rendered: https://fastapi.tiangolo.com/python-types/#generic-types

---

_Comment by @charliermarsh on 2023-11-03 03:53_

Would love for someone to take this on, great contributor project that doesn't require Rust :)

---

_Label `good first issue` added by @charliermarsh on 2023-11-03 03:53_

---

_Comment by @trag1c on 2023-11-03 08:42_

I'd like to work on this, please assign me :)

---

_Assigned to @trag1c by @MichaReiser on 2023-11-03 08:48_

---

_Comment by @doolio on 2023-11-03 09:14_

I would be happy to help too. I'm a bit reluctant to take it all on owing to the pace this project is moving and my time. I wouldn't want to disappoint people. Maybe, if it is a lot of work we could split it between myself and trag1c?

---

_Comment by @MichaReiser on 2023-11-03 15:07_

@trag1c could you comment here / reach out to @doolio if you see ways of collaborating on this issue? 

@doolio in what kind of wark are you interested?

---

_Comment by @trag1c on 2023-11-03 15:09_

Hey, to be fair I think I'm almost done if I didn't miss anything, I just need to update the part that generates the Settings section.

---

_Comment by @charliermarsh on 2023-11-03 15:42_

Thanks @trag1c, that's awesome (and to @doolio for offering to help)!

---

_Comment by @doolio on 2023-11-03 15:42_

I'm only learning to program (starting with python) so I think I could only meaningfully contribute right now in terms of reporting issues  and improving the docs. 

---

_Referenced in [astral-sh/ruff#8480](../../astral-sh/ruff/pulls/8480.md) on 2023-11-03 21:08_

---

_Closed by @charliermarsh on 2023-11-05 17:10_

---
