---
number: 6217
title: "Update `UP032` to handle repeated indices/keywords "
type: issue
state: closed
author: harupy
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-08-01T01:42:51Z
updated_at: 2023-08-02T21:00:09Z
url: https://github.com/astral-sh/ruff/issues/6217
synced_at: 2026-01-07T13:12:15-06:00
---

# Update `UP032` to handle repeated indices/keywords 

---

_Issue opened by @harupy on 2023-08-01 01:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Example:

```python
x = 1
print("{0} and {0}".format(x))
print("{a} and {a}".format(a=x))
```

## Expected behavior:

`UP032` is raised and the code is fixed to:

```python
x = 1
print(f"{x} and {x}")
print(f"{x} and {x}")
```

## Actual behavior:

`UP032` is not raised ([playground](https://play.ruff.rs/?secondary=Format#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsogCiMRdQAGI3EKIAqgAKLgDMAEyQbC1QW00KB48gGxE4eF0kAADtIPgBGFyPKD4WDQADWKFWGGguHSWz4eEEYFATygMlhGH4nBBmR4hEGunwcGKsVExEI0OhmWIxAw4iksiUGmJzNgrNukA5XJ5fPSuFmdIZuH4GiZLJQbKgcEQSCKgJxmhxuGk6vFmslYwysyI8FkHR40mh6DNxTYAF9eqiMViMGRcWgabpSU9IEslGgcGh0BgeBJofzOJkyhlTecyaIAPRx6HIsmQTMKRTZ+N50OZ6NyTPEJYSX5k-6iLBLFBYdGxp0x+JYFDQwm4V0oD1etGY7EcDRkFBKYOStopKSELYUWZYHXEpjD3Lesd+ri8fjEkOiDjcPgCKlpa3TYhnBiMLeiHe+nDxzItgQQgSz-M65CJspsHXDAkBpJYMExaQkAyTBhUkJIeFFDVHxRUcXwQWFlA0JY72PKB8U4Ht1G0KEDBCTgwgAYQw6QsJwgAdejiBMAxGIMSjGLCRjmLCEJGLQYADndAwMH4wT3TCbwy0gCR+AwOo1HBPZdBcFDIGfbEBkUCRiDVOg8Jk6AtH1ZQFjmJJFhWOhVIgT1tzQ7EAEdCAQRIj0tXBpgVDBnNcqgoTQBBCBXaSJEIA8vNmXy3ICoKQq1MQECwW9TI0HyXJiuhEuCyhpOgOQEE4TAqDXZ1dB4RRCCHWyRx9bETnwH9QytTIMCGCR-PTDNICMCQfTLUQjGgYhAzKAaoCMTJoTRHtxp6uoSjQagtUGhbYDQIxo2ZcLELmox2pQIw9oUWAqqO24Hxq+y6tjIrZE4eMMl2-TJX9XB1BKaA+DkWYHuhJ7iRhJQEOW17cQVWDoU4YN3TUjSO2dbAWzbVUmvZCrCLvMUJXzeIUHjEEJAQcRKA3W5REdKGFnrJ5GygSrcD4DqMBQE7CC+mN-WKbA0V5Tr7wShmmdmVm4HZxJMGjHBFC+jIN0usA7KfByMA0YFEgSNGUQ6Movza6AOrJjMJrmtWeA18oVvws2LZpiA6b6Apah1-t9cNio4ZVv7piwGkTOwPA11wyU-z1MKIpmcrKuqxXat3P6noD3AfsZr93N-aZhv5vDRGhANhqhaE0Dm2ElEUZBC+gEvEB4WBpAAOnibkqEB6EdCtqVpzIDIByy4hvw7gpiAyVEK6ynhGo73BJFhKFcFzDu+p4abXOmMgoXjWA5qXlfa84Mh69hXfC94OaeHRfhEkUKFz5LhA0UUVut47vOFS3rLoQHDu4C2JMoR-uaSx74IDkJwFASBAZLDkHbRWnsbqwjciCW80hcovXzHUBIDJZh50yIzFsxRiQVSqglHBBtgTKD2G7KgCN9iQHpNNLIJClBkIxpQtm1DKa0JvJbSUpCOqsNmOwvk5c9ScJvpyXKgtoCcAIR9ZySZZhL2bPJDI2RyZQAAEJZ0uFoHsfYvxzR0Xo-sc0ABqYsUCXEUOXa+HcADyABlKxNi5oAEk7HONUR3S4yckx4A6ozTxtj1FSiSpiHgjdrGqIulImR1D5xyK4K1JR4F8CqOaAlPqih2w4IQvg-mRCY5K1QjdDoJAYxwQCc9EklorytSLCoUaVCkLmjUjIp6Wt1IZCIkgGWCY4AqAIYOBKaTFBEXBAsNQkVhmSgmRoKZMwWa6NOuCIkkxQyvjIIs4aVJHqKGqdjC0+ZiDTT9ngWMMtuCqgDlIKudBCkJRggMP0sgxH3OjiM7psxh77JAmBOSxRdZrLoIchKa5ubFE-N9fYoLJSjKIjwBAsYMLrNEOidISB3p1EfiCHBjpUVQHRcgd6ywkyYDxWmAWkoiWYrmElOAKj1oDAJY7GW+p86KEwGvGWihKU5HzJkNE0Ldn-X2XyJ518spEH2fgkEiKeYIAITwgVKB5GZAXKK6p-KNm8yGRkyUOABBbEZvLBKDTGimslOkOYMw+RTlGfsbVUxbU4nwFfEVANdAYHhAlSKdrgSoCSDQ4kLhPljJjMUUhiQWWAtdhKtRxt1JLiwXNW8HKuX7x5aaQeSwyUYApXNbFt581KHxR3RAa5YCMujNfGJszWz9iPLDK6ogJBYDXFOTpfVjKvgYVoGklLERqWdNCbE7VUZoOanU2YB0jbdWKDwZ4C8QmJCUAAEWJam4ES7KK6rPtORQG7MW7uGqDRNC6l0AFlibhUYSug9R7cDXpJnexNw1pC4CwA47dy633EA-VgAAKg+zdHcL3QmA7eNdX0q4d2ZNwLBRjewmPLXgDQl6W7QDVtvIya7OD4EanW-MWBdX8KAZLVsGRZaPxjQSQiZHib8kozLRFNHmjDukFGKgjpUE1PzC1WYICBiIDMlsYg7ZXxVJaW6FtUBYSBWSjx-YBkBNMeltRy1+ZuQYQGI6NTVHWPyw48+dOoYElqwVMoZKHtZMwhQZfTpocMB9Q0IReQFjg0JvnRVOaZBpAxSI6GbtfoZafvwboeEXxMlGQ+vSQYxIABs0XjLCmJAAVmS+yxIUndBpZsrHXIsJOQaBltGTpmJezqAdA9WYlMJ1gEOR6EA7pIita0GAAAvGAeEIBuSXwMJAYALh3RgADCSYbkB66jKXgYLQEReumR4AN4A0ARtjZW+6Sb02voGGgB1ubYQQBAA))


---

_Comment by @dhruvmanila on 2023-08-01 04:24_

Hey, thanks for reporting. This matches the behavior of `pyupgrade` but I do think we could update the rule to handle such cases. \cc @charliermarsh 

---

_Label `rule` added by @dhruvmanila on 2023-08-01 04:25_

---

_Label `needs-decision` added by @dhruvmanila on 2023-08-01 04:25_

---

_Comment by @harupy on 2023-08-01 04:45_

@dhruvmanila Thanks for the comment! I found this code:

https://github.com/asottile/pyupgrade/blob/e982da96c48181856859c0eba57c88ef72f916ac/tests/features/format_literals_test.py#L18

```
        # Don't touch non-incrementing integers
        "'{0} {0}'.format(1)",
```

I'm investigating why `pyupgrade` made this choice.

---

_Comment by @harupy on 2023-08-01 04:54_

This comment might be the reason:

https://github.com/asottile/pyupgrade/blob/e982da96c48181856859c0eba57c88ef72f916ac/pyupgrade/_plugins/fstrings.py#L121

```
                # timid: could make the f-string longer
                if candidate and candidate in seen:
                    break
```

For example:

```python
"{0} and {0} and {0}".format(xxxxxxxxxxxxxxxxxxx)
```

is obviously shorter than:

```python
f"{xxxxxxxxxxxxxxxxxxx} and {xxxxxxxxxxxxxxxxxxx} and {xxxxxxxxxxxxxxxxxxx}"
```



---

_Comment by @dhruvmanila on 2023-08-01 05:00_

Hey, thanks for looking. Yes, the first example is shorter than the fix but if the fix is under the `line-length` then it's a valid, otherwise we just ignore it:

https://github.com/astral-sh/ruff/blob/d9e84c5241490304404c794276e96b7687eda00d/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs#L343-L357

---

_Comment by @harupy on 2023-08-01 05:11_

Another advantage of the original code is modifiability? Suppose you want to rename `xxxxxxxxxxxxxxxxxxx`, you can replace a single `xxxxxxxxxxxxxxxxxxx` in the original code, but three `xxxxxxxxxxxxxxxxxxx` in the fixed code.

---

_Comment by @dhruvmanila on 2023-08-02 06:40_

That's a good point although the LSP rename method can help with that (which most editors have). I think we should implement this. We already have the guard rail in place which avoids auto-fixing if the updated code becomes longer than the `line-length`.

---

_Label `needs-decision` removed by @dhruvmanila on 2023-08-02 06:40_

---

_Label `accepted` added by @dhruvmanila on 2023-08-02 06:40_

---

_Comment by @harupy on 2023-08-02 07:24_

@dhruvmanila Makes sense. I'll file a PR!

---

_Referenced in [astral-sh/ruff#6266](../../astral-sh/ruff/pulls/6266.md) on 2023-08-02 07:43_

---

_Comment by @dhruvmanila on 2023-08-02 20:59_

Closed by #6266

---

_Closed by @dhruvmanila on 2023-08-02 21:00_

---
