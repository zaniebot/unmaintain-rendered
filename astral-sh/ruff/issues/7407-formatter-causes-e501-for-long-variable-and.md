```yaml
number: 7407
title: Formatter causes E501 for long variable and attributes
type: issue
state: closed
author: aaravind100
labels:
  - formatter
assignees: []
created_at: 2023-09-15T10:30:50Z
updated_at: 2023-09-25T09:39:54Z
url: https://github.com/astral-sh/ruff/issues/7407
synced_at: 2026-01-12T15:54:47Z
```

# Formatter causes E501 for long variable and attributes

---

_@aaravind100_

I'm testing the format option. Found that formatting scripts with long variables and attributes which are indented cases E501.

ruff check, would raise `11:89: E501 Line too long (92 > 88 characters)` for this snippet.

- formatted with black
```py
class Foo:
    some_very_long_attribute_name: str = ""

    def __getitem__(self, val):
        ...


def main() -> None:
    if True:
        some_very_long_variable_name_abcdefghijk = Foo()
        some_very_long_variable_name_abcdefghijk = (
            some_very_long_variable_name_abcdefghijk[
                some_very_long_variable_name_abcdefghijk.some_very_long_attribute_name
                == "This is a very long string abcdefghijk"
            ]
        )


if __name__ == "__main__":
    main()
```
- formatted with ruff
```py
class Foo:
    some_very_long_attribute_name: str = ""

    def __getitem__(self, val):
        ...


def main() -> None:
    if True:
        some_very_long_variable_name_abcdefghijk = Foo()
        some_very_long_variable_name_abcdefghijk = some_very_long_variable_name_abcdefghijk[
            some_very_long_variable_name_abcdefghijk.some_very_long_attribute_name
            == "This is a very long string abcdefghijk"
        ]


if __name__ == "__main__":
    main()
```

- pyproject.toml
```toml
[tool.ruff]
target-version = "py39"
line-length = 88
select = ["B", "C", "E", "F", "I", "W"]
ignore = ["E731"]

[tool.black]
line-length = 88
target-version = ["py39"]
```

- version
`Python 3.9.5`
`ruff 0.0.289`
`OS Ubuntu 22.04.3 LTS(WSL)`

- Additional info
In the original script the class Foo is a pandas dataframe and this pattern is quite common.

---

_Renamed from "Formatter causes E501for long variable and attributes" to "Formatter causes E501 for long variable and attributes" by @aaravind100 on 2023-09-15 10:35_

---

_Label `formatter` added by @MichaReiser on 2023-09-15 13:13_

---

_Comment by @MichaReiser on 2023-09-15 13:19_

Thanks @aaravind100 for reporting this incompatibility. Our recommendation on `E501` is to not use it in combination with a formatter and we plan to update our documentation accordingly. What's your use case for using `E501` with the formatter? Or did you run into it because `E501` is part of the default rule set?

Edit: What I find interesting is that Ruff formats the example differently than black.

---

_Comment by @aaravind100 on 2023-09-15 13:52_

Hey @MichaReiser, "E" is selected along with line-length 88, wouldn't that mean E501 would also be checked?
This is a repo I inherited at work, and line length was always enforced to 88 to prevent longs lines in the pr which makes it difficult to review it.

Do you have a recommendation?

> Edit: What I find interesting is that Ruff formats the example differently than black.

Yes this is curious, this is what prompted me to create this issue. ruff formats it in a way and complains about it :smile: 

---

_Comment by @vastbluesky on 2023-09-15 19:05_

> What's your use case for using `E501` with the formatter?

Speaking for myself, I would think the use case is for identifying long lines that the formatter doesn't or can't fix, and which therefore require manual intervention, like splitting a long string across multiple lines or even shortening a long variable name.

---

_Closed by @MichaReiser on 2023-09-16 14:21_

---

_Comment by @MichaReiser on 2023-09-16 14:32_

> Speaking for myself, I would think the use case is for identifying long lines that the formatter doesn't or can't fix, and which therefore require manual intervention, like splitting a long string across multiple lines or even shortening a long variable name.

That sounds reasonable and is how we want to document the "incompatibility". What we can't guarantee is that the formatter never produces code that conflicts with E501 (e.g. the formatter doesn't break overlong comments). 

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-25 09:39_

---
