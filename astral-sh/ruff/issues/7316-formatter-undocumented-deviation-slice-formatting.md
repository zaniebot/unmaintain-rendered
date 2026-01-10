```yaml
number: 7316
title: "Formatter undocumented deviation: slice formatting"
type: issue
state: open
author: JonathanPlasse
labels:
  - formatter
  - style
assignees: []
created_at: 2023-09-12T20:50:10Z
updated_at: 2023-11-29T03:29:33Z
url: https://github.com/astral-sh/ruff/issues/7316
synced_at: 2026-01-10T11:09:49Z
```

# Formatter undocumented deviation: slice formatting

---

_Issue opened by @JonathanPlasse on 2023-09-12 20:50_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Black formatting
```python
def f():
    for event_index in range(header.number_non_empty_events):
        section_header_data = struct.unpack(
            JSON_BINARY_PARAMETERS.fmt_section_header,
            byte_array[
                byte_begin_index
                + byte_step_index * event_index : byte_begin_index
                + byte_step_index * (event_index + 1)
            ],
        )
```
Ruff formatting
```python
def f():
    for event_index in range(header.number_non_empty_events):
        section_header_data = struct.unpack(
            JSON_BINARY_PARAMETERS.fmt_section_header,
            byte_array[
                byte_begin_index + byte_step_index * event_index : byte_begin_index
                + byte_step_index * (event_index + 1)
            ],
        )
```

Use Ruff 0.0.289 with line length to 100

---

_Comment by @zanieb on 2023-09-12 20:58_

Do you have an ideal formatting for this case? I find both of these hard to reason about.

---

_Comment by @JonathanPlasse on 2023-09-12 21:00_

Maybe this
```python
def f():
    for event_index in range(header.number_non_empty_events):
        section_header_data = struct.unpack(
            JSON_BINARY_PARAMETERS.fmt_section_header,
            byte_array[
                byte_begin_index + byte_step_index * event_index
                : byte_begin_index + byte_step_index * (event_index + 1)
            ],
        )
```

---

_Comment by @MichaReiser on 2023-09-13 06:53_

Thanks for reporting this. The deviation here is rooted in the fact that:

* Black breaks a line by all binary operators first as if the left and right side of the `:` operator are a single expression
* Ruff considered the left and right two independent expressions and split them individually 

Here's an example where Black splits before all `+` operators even though it wouldn't be necessary to make the left side fit:

```python
def f():
    for event_index in range(header.number_non_empty_events):
        section_header_data = struct.unpack(
            JSON_BINARY_PARAMETERS.fmt_section_header,
            byte_array[
                byte_begin_index
                + byte_step_index : byte_begin_index
                + byte_step_index
                + (event_index + 1)
            ],
        )

# Ruff
def f():
    for event_index in range(header.number_non_empty_events):
        section_header_data = struct.unpack(
            JSON_BINARY_PARAMETERS.fmt_section_header,
            byte_array[
                byte_begin_index + byte_step_index : byte_begin_index
                + byte_step_index
                + (event_index + 1)
            ],
        )

```



I generally prefer Ruff's approach of treating them as separate expressions but we should probably add a softline break before the `:` operator similar to other operands (e.g. in binary expressions) and potentially indent the right side. 

Prettier's formatting looks similar to what @JonathanPlasse  proposes (I had to adjust the syntax slightly to get the JS equivalent)

```js
for (event_index in range(header.number_non_empty_events)) {
  section_header_data = struct.unpack(
    JSON_BINARY_PARAMETERS.fmt_section_header,
    {
      [byte_begin_index + byte_step_index * event_index]:
        byte_begin_index + byte_step_index * (event_index + 1),
    },
  );
}
```



---

_Label `wontfix` added by @MichaReiser on 2023-09-13 06:54_

---

_Label `formatter` added by @MichaReiser on 2023-09-13 06:54_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-13 06:54_

---

_Label `wontfix` removed by @konstin on 2023-09-13 10:20_

---

_Label `wontfix` added by @konstin on 2023-09-19 12:13_

---

_Comment by @MichaReiser on 2023-09-20 06:17_

We decided not to implement the deviation for now and instead focus on Black compatibility because:

* It eases migration for Black users
* Having more deviation makes it harder to assess whether a deviation is a bug or due to som other deviation. The fewer deviations, the easier it is to identify the root cause and improve overall black compatibility

We plan to re-visit the formatting eventually

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-09-27 15:38_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-27 15:38_

---

_Comment by @charliermarsh on 2023-09-27 15:40_

We're not planning to change this for the Beta. We want to re-open the possibility of changing to match @JonathanPlasse's suggested style once we have better tooling to evaluate the impact (e.g., ruff-shades).

---

_Label `wontfix` removed by @MichaReiser on 2023-10-26 23:57_

---

_Label `wontfix` added by @MichaReiser on 2023-10-26 23:57_

---

_Label `style` added by @MichaReiser on 2023-10-26 23:57_

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2023-10-26 23:57_

---

_Label `wontfix` removed by @MichaReiser on 2023-10-27 02:08_

---

_Comment by @MichaReiser on 2023-11-29 03:29_

We may want to consider https://github.com/astral-sh/ruff/issues/8897 when making a style decision to make sure our handling is consistent.

---
