---
number: 6421
title: "BLE001 rule with blind exception `Exception` is passing the check"
type: issue
state: closed
author: eslamodeh
labels: []
assignees: []
created_at: 2023-08-08T10:22:48Z
updated_at: 2024-02-29T16:28:58Z
url: https://github.com/astral-sh/ruff/issues/6421
synced_at: 2026-01-10T01:22:45Z
---

# BLE001 rule with blind exception `Exception` is passing the check

---

_Issue opened by @eslamodeh on 2023-08-08 10:22_

Hey everyone,
I just found that there are cases where the blind exception is not being caught.


* A minimal code snippet that reproduces the bug.
After allowing the `BLE001` rule, in order not to allow blind exception, the linter allow such a code:
```py
        try:
            do_something("something")
        except Exception: # This blind exception should not be allowed
            logger.error("Error during forwarding", exc_info=True) # Changing this line would change the linter result
```

* The current Ruff settings (any relevant sections from your `pyproject.toml`).
```py
target-version = "py39"

select = ["E", "W", "F", "B",
          "C4", "T10", "PIE", "T20",
          "PYI", "Q", "TCH", "INT",
          "ERA", "RUF", "N", "UP",
          "S", "BLE", "C90", "RET",
          "SLF", "SIM", "ARG", "PERF",
          "PL"
         ]

fixable = ["ALL"]
ignore = ["E501", "RUF012", "S108"]
line-length = 120

[tool.ruff.extend-per-file-ignores]
"tests/*.py" = ['S101', 'SLF001', 'PLR2004', 'PLR0913']
"*factory.py" = ['S106']
```
* The current Ruff version (`ruff --version`)
ruff 0.0.280


---

_Comment by @charliermarsh on 2023-08-08 18:48_

I believe this was changed intentionally to support catching + logging "blind" exceptions, which I think is a reasonable pattern (https://github.com/astral-sh/ruff/issues/2212).

---

_Comment by @charliermarsh on 2023-08-08 20:53_

(Closing as intended behavior.)

---

_Closed by @charliermarsh on 2023-08-08 20:53_

---

_Comment by @eslamodeh on 2023-08-09 13:53_

Thanks @charliermarsh !

---

_Comment by @aiven-anton on 2024-02-29 14:23_

@charliermarsh In applications where solid error handling is important, this aspect of the BLE rule is actually a limitation.

In such applications, you typically want to have _one single place_ in the application, at its entry-point/main loop where bare `Exception` is dealt with. In any other places, nested within the application, having a `except Exception:` reduces the chances of achieving appropriate behavior during situations like falling over due to limited memory.

If there's a single top-level try-block handling `Exception`, it is feasible to ensure the application is not attempting to handle fatal exceptions like `MemoryError`:

```python
def main():
    app = App()
    while True:
        try:
            app.run()
        except (MemoryError, SystemError, ImportError, AttributeError):
            raise  # fall over and let scheduler restart us
        except Exception:
            logger.error("A non-fatal unhandled exception occurred in the application, logging and continuing", exc_info=True)            
```

Now if deep within the application code is added that catches `Exception` broadly, like so:

```python
def send_request():
    try:
        response = http.request()
    except Exception:
        raise FailedSendingStuff  # oops, another allocation! :/
```

... this yields the hard work invested into having a solid entry-point error handling moot. If a `MemoryError` occurs in the above code, chances are the application would have no chance at all to print something that hints at the problem being a memory error.

Phrased another way: _I want to be able to strictly enforce always falling over for fatal exceptions_, and never having intermediate handling of them.

(I'm sure I've read a statement by Guido many years ago, saying it never makes sense to catch `MemoryError`, with essentially the same justification as explained here. I've since tried to find and reference that quote many times, without success, so it might just be my memory playing tricks ...)

With this use case in mind, do you think it's strong enough to justify introducing a configuration parameter to control the strictness of `BLE001`? (Or perhaps it should be a separate code, but that's details that can be discussed later).

---

_Referenced in [Aiven-Open/rohmu#169](../../Aiven-Open/rohmu/pulls/169.md) on 2024-02-29 14:25_

---
