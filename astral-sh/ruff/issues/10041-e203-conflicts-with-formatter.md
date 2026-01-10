```yaml
number: 10041
title: "E203: conflicts with formatter"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
assignees: []
created_at: 2024-02-19T07:35:42Z
updated_at: 2024-02-26T17:22:37Z
url: https://github.com/astral-sh/ruff/issues/10041
synced_at: 2026-01-10T11:09:52Z
```

# E203: conflicts with formatter

---

_Issue opened by @MichaReiser on 2024-02-19 07:35_


Hi, I think there is another case, where it is not working and that is when we have multiple indices, like in pandas: 

My example looks like this: 

`dataframe.loc[index + 1 :, "columnname"] `

`Ruff format` always adds the space and `ruff . --fix` always removes it.

_Originally posted by @Blumenkind111 in https://github.com/astral-sh/ruff/issues/8752#issuecomment-1948430614_

Playground https://play.ruff.rs/de63f867-2176-4c27-b653-d0c3eb49754d
            

---

_Renamed from "E203: conflict with formatter" to "E203: conflicts with formatter" by @MichaReiser on 2024-02-19 07:35_

---

_Label `bug` added by @MichaReiser on 2024-02-19 07:36_

---

_Label `help wanted` added by @MichaReiser on 2024-02-19 07:36_

---

_Comment by @Paciupa on 2024-02-21 00:22_

I have the same issue:

```python
countries = ["Spain", "France", "UK", "Norway", "Iceland"]
print(f"{countries[:3]}")
print(f"{countries[1:4]}")

middle = len(countries) // 2
print(f"{countries[middle  :  middle + 3]}") # `ruff format --preview` add spaces here, byt `ruff --fix` removes spaces.

print(f"{countries[-3:]}")
```
It started only since version `0.2.2`.
Without `--preview` ruff doesn't format that line.

---

_Comment by @dhruvmanila on 2024-02-21 05:20_

> It started only since version `0.2.2`.

This is because f-string formatting was added in preview in that release. This isn't the issue here but just clarifying that the problem was present before that version as well. You can try it out by keeping the expression outside the f-string:

```python
countries[middle  :  middle + 3]
```

---

_Label `help wanted` removed by @MichaReiser on 2024-02-23 08:55_

---

_Comment by @MichaReiser on 2024-02-23 09:30_

It might be difficult to fix `dataframe.loc[index + 1 :, "columnname"] ` without an AST or more lookahead (which is slow). The rule would need to know that what it encounters here is a tuple where this is fine. 

I wonder if it's worth the struggle. To me it seems the main purpose of the rule related to `:` is to detect whitespace before the colon in clause headers. Maybe we just skip over colons entirely when inside parentheses. 

---

_Comment by @MichaReiser on 2024-02-23 09:45_

Regarding `countries[middle  :  middle + 3]`:

`E203` enforces equal spacing around the `:` in accordance to PEP8:

> However, in a slice the colon acts like a binary operator, and should have equal amounts on either side (treating it as the operator with the lowest priority). [source](https://peps.python.org/pep-0008/#pet-peeves) 

However, the rule disallows leading tabs or multiple spaces (which I think is in accordance with PEP8 although not explicitly mentioned). The rule's fix isn't as sophisticated as the formatter. It always removes the whitespace if it encounters multiple spaces or tabs that may disagree with the formatter). However, that's fine, because we only aim for formatted code not to raise lint errors but our fixes don't need to be properly formatted.

The linter won't raise an error for your example if you only use one whitespace around the colons. 




---

_Assigned to @MichaReiser by @MichaReiser on 2024-02-23 09:48_

---

_Closed by @MichaReiser on 2024-02-26 17:22_

---
