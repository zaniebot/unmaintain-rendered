```yaml
number: 5779
title: Remove parentheses from tuples in comprehension target
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-07-15T14:55:56Z
updated_at: 2023-07-19T12:05:40Z
url: https://github.com/astral-sh/ruff/issues/5779
synced_at: 2026-01-12T15:54:45Z
```

# Remove parentheses from tuples in comprehension target

---

_@MichaReiser_

```python
# Input
{k: v for k, v in this_is_a_very_long_variable_which_will_cause_a_trailing_comma_which_breaks_the_comprehension}

# black
{
    k: v
    for k, v in this_is_a_very_long_variable_which_will_cause_a_trailing_comma_which_breaks_the_comprehension
}

# Ruff
{
    k: v
    for (
		k, 
		v
	) 
	in this_is_a_very_long_variable_which_will_cause_a_trailing_comma_which_breaks_the_comprehension
}
```

Ruff should not add parentheses around `k, v`. 


---

_Label `formatter` added by @MichaReiser on 2023-07-15 14:58_

---

_Comment by @cnpryer on 2023-07-15 23:05_

Interesting. Didn't know `black` would *never* format the tuple with parentheses. Even in this extreme example
```py
# Input
{k: v for a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a  in this_is_a_very_long_variable_which_will_cause_a_trailing_comma_which_breaks_the_comprehension}

# Output
{
    k: v
    for a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a in this_is_a_very_long_variable_which_will_cause_a_trailing_comma_which_breaks_the_comprehension
}
```

---

_Comment by @MichaReiser on 2023-07-17 08:41_

Uhh interesting. It does however expand the tuple fields. That could get... interesting to say the least:

```python
{
    k: v
    for a, a, a, a, [
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
        a,
    ] in this_is_a_very_long_variable_which_will_cause_a_trailing_comma_which_breaks_the_comprehension
}

```

but I find this looks rather weird. 

---

_Assigned to @cnpryer by @MichaReiser on 2023-07-17 08:41_

---

_Closed by @MichaReiser on 2023-07-19 12:05_

---
