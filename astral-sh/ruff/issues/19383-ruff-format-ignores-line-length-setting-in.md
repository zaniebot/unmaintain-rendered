```yaml
number: 19383
title: "`ruff format` ignores `line-length` setting in assignment expression"
type: issue
state: open
author: ALPA-Industry-and-Technology
labels:
  - question
  - formatter
  - style
assignees: []
created_at: 2025-07-16T10:26:57Z
updated_at: 2025-07-16T12:01:14Z
url: https://github.com/astral-sh/ruff/issues/19383
synced_at: 2026-01-12T15:54:56Z
```

# `ruff format` ignores `line-length` setting in assignment expression

---

_@ALPA-Industry-and-Technology_

### Question

I’m encountering a formatting issue where `ruff format` seems to ignore the configured `line-length = 88` when formatting a multiline **assignment**.

Here’s a simplified example from my codebase:

```
def update_sensor_threshold(self, threshold: str) -> None:
    """
    Update the threshold value of a sensor configuration.

    :param threshold: Threshold value as a string.
    """
    threshold = threshold.replace(",", ".")
    (
        self._config
        .hardware
        .sensors
        .sensor_motor_left
        .temperature
        .calibration
        .threshold
        .value
    ) = threshold
```

My pyproject.toml includes:
```
[tool.black]
line-length = 88

[tool.ruff]
line-length = 88
```

However, when I run `ruff format .`, it reformats the function to:

```
def update_sensor_threshold(self, threshold: str) -> None:
    """
    Update the threshold value of a sensor configuration.

    :param threshold: Threshold value as a string.
    """
    threshold = threshold.replace(",", ".")
    (
        self._config.hardware.sensors.sensor_motor_left.temperature.calibration.threshold.value
    ) = threshold
```

This resulting line is clearly longer than 88 characters, which I was trying to avoid by splitting the chain manually.


My questions:

1. Is this expected behavior or a bug?
2. Besides using # fmt: skip, is there a clean way to prevent ruff format from collapsing such attribute chains in assignments?
3. Is this something that could or should be improved?

Thanks in advance - and thanks for all the great work on ruff!

### Version

ruff 0.11.13

---

_Label `question` added by @ALPA-Industry-and-Technology on 2025-07-16 10:26_

---

_Comment by @MichaReiser on 2025-07-16 11:12_

Thanks for the detailed report and the kind words.

This is in sofar expected as we try to match black's formatting and black formats this exactly the same. But that doesn't mean it's great. 

The assignment formatting is fairly complicated and I don't know what the impact on the ecosystem is if Ruff starts parenthesizing and formatting attribute access in assignment targets over multiple lines but it might be worth exploring

---

_Label `formatter` added by @MichaReiser on 2025-07-16 11:12_

---

_Label `style` added by @MichaReiser on 2025-07-16 11:12_

---

_Comment by @ALPA-Industry-and-Technology on 2025-07-16 11:55_

Thanks for the clarification!

Still, I find this behavior quite strange and unintuitive. As a user, when I explicitly set `line-length = 88` in my `pyproject.toml`, I expect the formatter - whether it’s Black or something else - to respect that limit consistently across all cases, including multi-attribute assignments.

When the formatter collapses attribute chains into a single line that clearly exceeds the configured limit, it feels like it's silently bypassing an explicit user setting. This creates confusion and makes the configuration feel unreliable or ignored.

Even if it's technically consistent with Black, I would personally consider this behavior a bug - or at the very least, a surprising and undocumented edge case. If this is indeed by design, perhaps a flag or more granular formatting option could help control this behavior in a cleaner and more predictable way than falling back to `# fmt: skip`.

---

_Comment by @MichaReiser on 2025-07-16 12:01_

> Still, I find this behavior quite strange and unintuitive. As a user, when I explicitly set line-length = 88 in my pyproject.toml, I expect the formatter - whether it’s Black or something else - to respect that limit consistently across all cases, including multi-attribute assignments.

The line length in the formatter isn't a hard upper bound. It's more a guidance: Try to keep the code below that length. There are many other cases where the formatter won't split a line. Either because Python's syntax doesn't allow it (or it would require using line continuations), or because the formatter simply doesn't support it yet (e.g. splitting strings or comments). What you're seeing here is another case like this where it's something where the formatter doesn't support and it requires some design work to define how the formatter should format this specific code (without regressing any other existing code). 




---
