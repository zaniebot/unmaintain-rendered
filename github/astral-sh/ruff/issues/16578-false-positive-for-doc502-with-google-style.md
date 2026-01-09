---
number: 16578
title: "False positive for `DOC502` with Google-style docstring"
type: issue
state: open
author: gtkacz
labels:
  - bug
  - docstring
assignees: []
created_at: 2025-03-09T17:32:44Z
updated_at: 2025-11-12T21:03:03Z
url: https://github.com/astral-sh/ruff/issues/16578
synced_at: 2026-01-07T13:12:16-06:00
---

# False positive for `DOC502` with Google-style docstring

---

_Issue opened by @gtkacz on 2025-03-09 17:32_

### Summary

I'm aware this rule is still in preview, but still wanted to share just to let you know. I have the experimental `DOC502` rule enabled, and when running `ruff check` I'm getting the following warning:

```
DOC502 Raised exceptions are not explicitly raised: `ValueError`, `DateError`
```

For the below method:

```
def nth_of_month(weekday: Weekday | ISOWeekday, date: DateT, n: int) -> DateT:
	"""
	Returns the nth date of the given day of the week in the month of the given date.

	Args:
		weekday (Weekday): The day of the week.
		date (DateT): The reference date.
		n (int): The nth occurrence of the given day of the week.

	Raises:
		ValueError: If n is less than 1 or greater than 5.
		DateError: If the month does not have a nth occurrence of the given day of the week.

	Returns:
		DateT: The nth date of the given day of the week in the month of the given date.
	"""
	weekday = _TemporalAdjusterForWeekday.__normalize_weekday(weekday)

	if n < 1 or n > 5:
		raise ValueError(f'The value of n must be between 1 and 5, but is {n}.')

	output_date = date.replace(day=1) + relativedelta(
		weekday=weekday.value,
		weeks=n - 1,
	)

	if output_date.month != date.month:
		raise DateError(
			f'The month does not have a {n}th occurrence of {weekday.name.lower()}.',
		)

	return output_date
```

Which very clearly explicitly raises the two exceptions in question. Am I doing something wrong? [This](https://github.com/gtkacz/temporal_adjusters_py/blob/4cc05c43c45fedbd6184639aeac8cf6bde335f05/ruff.toml) is my config file.

### Version

ruff 0.9.10 (0dfa810e9 2025-03-07)

---

_Comment by @charliermarsh on 2025-03-09 17:38_

Unfortunately I can't reproduce that in the playground: https://play.ruff.rs/35be2d26-265e-4298-beed-931cbe1b5825

---

_Label `needs-mre` added by @charliermarsh on 2025-03-09 17:38_

---

_Label `docstring` added by @AlexWaygood on 2025-03-10 11:32_

---

_Closed by @gtkacz on 2025-03-11 18:46_

---

_Comment by @pilmus on 2025-04-15 07:06_

Hi, I'm observing similar behavior. It seems to be triggered by adding a description for the error. In the following example code, `example_ruff_warning` causes the following warning
```
Raised exception is not explicitly raised: `InitializationError` (DOC502) [Ln 4, Col 3]
```

while `example_no_warning` does not.

```
class Example:
	@classmethod
	def example_ruff_warning():
		"""
		Raises:
		------
			InitializationError: Description.
		"""
		a = get_a()
		if not a:
			raise InitializationError
		return a

	@classmethod
	def example_no_warning():
		"""
		Raises:
		------
			InitializationError
		"""
		a = get_a()
		if not a:
			raise InitializationError
		return a
```

This behavior also shows up in the [playground](https://play.ruff.rs/ba7671cd-f60d-4564-a1c1-12ed79e279b0).

## Version
ruff 0.11.5 (installed with pip)


---

_Comment by @ntBre on 2025-04-15 12:57_

Thanks for the MRE! I'll reopen the issue.

---

_Reopened by @ntBre on 2025-04-15 12:57_

---

_Label `needs-mre` removed by @ntBre on 2025-04-15 12:57_

---

_Label `bug` added by @ntBre on 2025-04-15 12:57_

---

_Comment by @dhruvmanila on 2025-04-15 16:11_

@pilmus I think the format of your example is incorrect as per https://numpydoc.readthedocs.io/en/latest/format.html#raises.

Here's what the NumPy style would like:
```py
class Example:
	@classmethod
	def example_ruff_warning():
		"""
		Raises
		------
			InitializationError
				Description
		"""
		a = get_a()
		if not a:
			raise InitializationError
		return a
```

Specifically:
* There should be no colon (`:`) after `Raises`
* The description goes on the second line and not on the same line. The same line description is described by Google style.

The above doesn't raise a diagnostic: https://play.ruff.rs/6a3d7087-8d84-45b0-bf4e-bf7f08ffaa9e

@ntBre I don't think that's a reproducer, we can probably close this.

---

_Comment by @dhruvmanila on 2025-04-15 16:25_

Nevermind, I think I misunderstood your example. @ntBre pointed out that it seems to be an issue with Google-style docstring. He also shared a minimal example to reproduce this: https://play.ruff.rs/b0edb615-455f-4151-b2c7-b19d4cc6b965.

---

_Renamed from "`DOC502` weird functionality" to "False positive for `DOC502` with Google-style docstring" by @dhruvmanila on 2025-04-15 16:25_

---

_Referenced in [astral-sh/ruff#18959](../../astral-sh/ruff/issues/18959.md) on 2025-06-26 14:24_

---

_Comment by @EricNRG on 2025-11-06 17:00_

Still not fixed as of Ruff 0.14.0.

Here's another example:

        """Autozero function control (AZERO command).

        Enables or disables the autozero function. The autozero function
        applies only to DC voltage, DC current, and resistance measurements.

        Returns:
            AutoZeroModes: Current autozero mode (OFF=0, ON=1)

        Raises:
            ValueError: If the instrument returns an unknown auto zero mode value.

        ### Control Parameters
        - **OFF (0)**: Zero measurement is updated once, then only after a function,
          range, aperture, NPLC, or resolution change.
        - **ON (1)**: Zero measurement is updated after every measurement.
        - **ONCE (2)**: Zero measurement is updated once, then only after a function,
          range, aperture, NPLC, or resolution change.

        ### Behavior Details
        - **Power-on default**: ON
        - **Default control**: ON

Gives this message:

Raised exceptions are not explicitly raised: `- **OFF (0)**`, `- **ON (1)**`, `- **ONCE (2)**`, `- **Power-on default**`, `- **Default control**`, `### When Autozero is ON`, `### When Autozero is OFF or ONCE`, `- **Query**`, `- **Set**`

---

_Comment by @gtkacz on 2025-11-09 22:21_

@EricNRG the original use case is fixed for me, I'm on ruff 0.12.2

---

_Comment by @ntBre on 2025-11-10 14:18_

@EricNRG This looks like it might be a separate issue. Could you share a full example? I tried putting some of what you included here in the [playground](https://play.ruff.rs/d6e04499-a9af-40ef-8352-e1b68b41af34), but I'm not able to reproduce the issue.

---

_Comment by @gtkacz on 2025-11-10 16:48_

@ntBre I found something weird based on your original reproduction, looks like built in exceptions or non-imported exceptions pass the check. If you uncomment the exception on the example I provided it will work, as it's no longer imported

Playground URL: https://play.ruff.rs/aab128f8-5c9f-495b-b32f-c3cace8168d0

---

_Comment by @ntBre on 2025-11-10 16:59_

Oh good catch, thanks! So the rule is working correctly as long as everything is imported/in scope?

---

_Comment by @gtkacz on 2025-11-10 20:39_

Actually I think it doesn't work with imports still

---

_Comment by @ntBre on 2025-11-10 22:03_

Hmm, I must be misunderstanding then. If I uncomment the `class ValidationError: ...` in your playground link, the DOC502 false positive goes away, and the same is true if I import it.

False positive:

```console
$ ruff check --select DOC502 --preview --output-format concise - <<EOF
def f():
        """
        Raises:
                ValidationError:
        """
        raise ValidationError
EOF
-:2:2: DOC502 Raised exception is not explicitly raised: `ValidationError`
Found 1 error.
```

Class defined above:

```console
$ ruff check --select DOC502 --preview --output-format concise - <<EOF
class ValidationError: ...

def f():
        """
        Raises:
                ValidationError:
        """
        raise ValidationError
EOF
All checks passed!
```

Class imported:

```console
$ ruff check --select DOC502 --preview --output-format concise - <<EOF
from somewhere import ValidationError

def f():
        """
        Raises:
                ValidationError:
        """
        raise ValidationError
EOF
All checks passed!
```




---

_Comment by @EricNRG on 2025-11-12 12:50_

Here's a full example in the playground:

https://play.ruff.rs/762f9940-aec9-4d40-bdf5-6bcff436f771

---

_Comment by @ntBre on 2025-11-12 21:03_

@EricNRG Thanks! I just checked and this will be resolved by #20535.

---
