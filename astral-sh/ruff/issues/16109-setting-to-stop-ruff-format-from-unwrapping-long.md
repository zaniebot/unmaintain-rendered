```yaml
number: 16109
title: "Setting to stop `ruff format` from unwrapping long strings"
type: issue
state: closed
author: jamesbraza
labels:
  - formatter
  - needs-decision
assignees: []
created_at: 2025-02-12T00:49:39Z
updated_at: 2025-02-12T19:54:23Z
url: https://github.com/astral-sh/ruff/issues/16109
synced_at: 2026-01-12T15:54:55Z
```

# Setting to stop `ruff format` from unwrapping long strings

---

_@jamesbraza_

### Description

If I have something like this:

```python
long_string = (
    "An above 79 but below 88 char line. In other words, a long long long long long"
    " long string."
)
```

I have observed with `ruff==0.9.6` that `ruff format --line-length 120` will unwrap this string to be one line.

When working with (a convoluted) set up of both `ruff format` and `black` checked code, this string unwrapping can cause issues:

- Someone running `black`: won't unwrap lines
- Someone running `ruff`: will unwrap lines

Is there any chance we can get a setting to not auto-unwrap strings?

---

_Comment by @dhruvmanila on 2025-02-12 04:49_

Thank you for the report.

This is coming from the 2025 style guide of formatting implicitly concatenated strings if they fit on a single line within the line length limit. This is a known deviation from Black (https://docs.astral.sh/ruff/formatter/black/#implicit-concatenated-strings) although Black's unstable style applies the same formatting.

> Is there any chance we can get a setting to not auto-unwrap strings?

I don't think there's a way to configure this specific way except for [suppressing those lines](https://docs.astral.sh/ruff/formatter/#format-suppression). Using two separate line length limit could potentially solve it but that would create even more differences.

---

_Label `question` added by @dhruvmanila on 2025-02-12 04:49_

---

_Label `formatter` added by @dhruvmanila on 2025-02-12 04:49_

---

_Comment by @jamesbraza on 2025-02-12 06:53_

Thanks for understanding, yeah you understand the issue at hand.

> I don't think there's a way to configure this specific way except for [suppressing those lines](https://docs.astral.sh/ruff/formatter/#format-suppression).

This won't work because it's not a one off where one can add `# fmt: off`/`on`. It applies to every string in the code base. In the past every long string in the entire source was formatted:

```bash
black --preview --enable-unstable-feature=string_processing src
```

> This is coming from the 2025 style guide of formatting implicitly concatenated strings if they fit on a single line within the line length limit.

Thanks for clarifying. I guess the request here is to add a new setting that either:
- Disables implicit string concatenation
- Applies a different `line-length` for implicit string concatenation

---

_Comment by @MichaReiser on 2025-02-12 08:08_

I want to avoid introducing syntax-specific line lengths. I think that would be confusing and open an endless request for line-length settings. I think we can consider an option to disable automatic string splitting/joining. But it's somewhat unclear if that setting then should also disable comment formatting (automatically breaking comments after a specific line length) if this is something the formatter supports in the future. 

I'll keep this open to see if there's more demand for it. I consider it unlikely that we implement said setting if there isn't.

---

_Label `question` removed by @MichaReiser on 2025-02-12 08:09_

---

_Label `wish` added by @MichaReiser on 2025-02-12 08:09_

---

_Label `wish` removed by @MichaReiser on 2025-02-12 08:09_

---

_Label `needs-decision` added by @MichaReiser on 2025-02-12 08:09_

---

_Comment by @jamesbraza on 2025-02-12 19:54_

Sound good. I agree it's a bit hacky, ima just close this out. Really I need to resolve a jank set up 😆 

---

_Closed by @jamesbraza on 2025-02-12 19:54_

---
