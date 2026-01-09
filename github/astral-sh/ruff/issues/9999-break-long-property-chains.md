---
number: 9999
title: Break long property chains
type: issue
state: open
author: winged
labels:
  - formatter
  - style
assignees: []
created_at: 2024-02-15T13:00:11Z
updated_at: 2024-02-17T19:22:19Z
url: https://github.com/astral-sh/ruff/issues/9999
synced_at: 2026-01-07T13:12:15-06:00
---

# Break long property chains

---

_Issue opened by @winged on 2024-02-15 13:00_

Given a code line like the following, Ruff and Black both don't break the line, even it would be possible (and make it more readable IMHO). Black kinda tries to shorten the line a bit, but it would still be too long for the given limit.

This happens for example when extracting data from deeply object data structure.

```python
# Input:
foo = self.some_very.nested.data.which.is_really.really.deep_into_a_data_structure.far.beyond.line_length_limit

# Black 23.12.1:
foo = (
    self.some_very.nested.data.which.is_really.really.deep_into_a_data_structure.far.beyond.line_length_limit
)

# Ruff 0.2.1
foo = self.some_very.nested.data.which.is_really.really.deep_into_a_data_structure.far.beyond.line_length_limit
```

What I would like to see is breaking the property chain into multiple lines, something like this:

```python
# Desired output:
foo = (
    self
    .some_very
    .nested
    .data
    .which
    .is_really
    .really
    .deep_into_a_data_structure
    .far
    .beyond
    .line_length_limit
)

# Alternative desired output (less readable, but also less wasted lines)
foo = (
    self.some_very.nested.data.which.is_really.really
    .deep_into_a_data_structure.far.beyond
    .line_length_limit
)
```

Note: This is closely related to #8598, thanks to [Micha in Discord](https://discord.com/channels/1039017663004942429/1207664692885983242/1207666684324880464) for the pointer.


---

_Label `formatter` added by @dhruvmanila on 2024-02-16 07:20_

---

_Label `style` added by @dhruvmanila on 2024-02-16 07:20_

---

_Comment by @dhruvmanila on 2024-02-16 07:23_

I like this and I'd prefer to break them all into individual lines like in your desired output.

There's the question of indentation as well. Is the following better?
```python
foo = (
    self
	    .some_very
	    .nested
	    .data
	    .which
	    .is_really
	    .really
	    .deep_into_a_data_structure
	    .far
	    .beyond
	    .line_length_limit
)
```

---

_Comment by @MichaReiser on 2024-02-17 19:22_

Thanks for opening this issue. We'll need to look at this holistically to ensure the formatting is consistent. E.g. how should attributes be broken in binary expressions? Should this style only apply in assignment positions or also in clause headers? 

---
