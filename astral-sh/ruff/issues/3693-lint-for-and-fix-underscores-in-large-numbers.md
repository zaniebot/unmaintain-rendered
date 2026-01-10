```yaml
number: 3693
title: lint for and fix underscores in large numbers
type: issue
state: open
author: samuelcolvin
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-03-23T20:19:32Z
updated_at: 2023-07-10T01:29:07Z
url: https://github.com/astral-sh/ruff/issues/3693
synced_at: 2026-01-10T11:09:46Z
```

# lint for and fix underscores in large numbers

---

_Issue opened by @samuelcolvin on 2023-03-23 20:19_

Would be great if

```py
1000
```

got converted to
```py
1_000
```

And so on for larger numbers.

I guess out often to insert the underscore could be configurable, not sure if other cultures often use commas in large numbers other than ever three digits?

---

_Label `rule` added by @charliermarsh on 2023-03-23 21:26_

---

_Label `wishlist` added by @charliermarsh on 2023-03-23 21:26_

---

_Label `wishlist` removed by @charliermarsh on 2023-03-23 21:29_

---

_Comment by @Avasam on 2023-03-25 16:46_

> not sure if other cultures often use commas in large numbers other than ever three digits?

For whole numbers decimals, I don't think so, should be straightforward. But what about floating-point numbers? And consider how this would work for binary, octal and hexa? May be project-specific
Some references:
![image](https://user-images.githubusercontent.com/1350584/227731224-04593e69-7b85-45f6-8198-4b713f1a1264.png)
![image](https://user-images.githubusercontent.com/1350584/227730859-fab748da-d5cc-40d7-ae40-616f9de8b4ec.png)
![image](https://user-images.githubusercontent.com/1350584/227730438-52b78b8b-1fb7-46b1-9124-25313757cc1b.png)

ESLint-Unicorn's numeric separator style rule is also a good reference for configuring such a rule: https://github.com/sindresorhus/eslint-plugin-unicorn/blob/main/docs/rules/numeric-separators-style.md




---

_Comment by @akx on 2023-03-28 12:25_

> not sure if other cultures often use commas in large numbers other than ever three digits?

At least the [Indian numbering system](https://en.wikipedia.org/wiki/Indian_numbering_system#Use_of_separators) uses a "[2,...]2,2,3" grouping; quoting the examples from the linked Wikipedia page:

5,00,000 <=> 500,000
12,34,56,789 <=> 123,456,789
17,00,00,00,000 <=> 17,000,000,000
6,78,90,00,00,00,00,000 <=> 6,789,000,000,000,000

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:29_

---
