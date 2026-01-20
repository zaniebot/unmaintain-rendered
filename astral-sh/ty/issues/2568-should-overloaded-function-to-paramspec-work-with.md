```yaml
number: 2568
title: "Should overloaded function to `ParamSpec` work with argument type expansion?"
type: issue
state: open
author: dhruvmanila
labels:
  - needs-decision
  - calls
  - overloads
assignees: []
created_at: 2026-01-20T04:04:27Z
updated_at: 2026-01-20T04:05:46Z
url: https://github.com/astral-sh/ty/issues/2568
synced_at: 2026-01-20T04:32:05Z
```

# Should overloaded function to `ParamSpec` work with argument type expansion?

---

_@dhruvmanila_

In https://github.com/astral-sh/ruff/pull/21946, there's a test case which ty passes but other type checkers raise an error:

```py
from typing import Callable, overload

@overload
def int_int(x: int) -> int: ...
@overload
def int_int(x: str) -> int: ...

def with_parameters[**P](f: Callable[P, int], *args: P.args, **kwargs: P.kwargs) -> Callable[P, str]:
    def nested(*args: P.args, **kwargs: P.kwargs) -> str:
        return str(f(*args, **kwargs))
    return nested

def foo(int_or_str: int | str):
    # Argument type expansion leads to matching both overloads.
    # TODO: Should this be an error instead?
    # revealed: Overload[(x: int) -> str, (x: str) -> str]
    reveal_type(with_parameters(int_int, int_or_str))
```

Other type checkers emit a diagnostic here:
- [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=e5ec5b750be3b566e0f70c55dbe1ed80)
- [pyright](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgDRRgBu1IJYRAJgFBcACTLNpy4dqwTChgB9VDAAUADwBcEmAEooAWgB8qlQDpDfAa3bdR42TMmKVAZxggNOvVEP6eFqAHckMABZSCEQgRBDUMCx2ANoAVLEACgC6csAqxGSUNNEJ9LJJ9LEhaHYqCfrFdoWxANbelWX6dZXOuhnkVNQ59A4gSUpcUENQXijUDtQcckUgJY2V1c2zpVDlSyWtUL0Dw7tQIBEAriAoW46p0wtQ8et2amqDwwcwx6djE9wiYlDAYGByVlwUm2qigAB8zk4dk9qMxSFJ4AhqHJfAEgiEwhEogDJNYYHlcUDevcuEA)
- [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN0BhVFCipssADR1cANxiUouVJgA66NQAFZ8xcrWYYYTugYB9VgwAU%2BRMYYBKOgFoAfHduFPm7QqWqshnbmJta2cAyUjq7udJ6EavqBAO4QDAAWpsSolKg0MAzycADaAFQlAAoAupZgtkIiYrBF5VIWlVIl2WxwtuWEXXAdJQDWSQO9hKMDUW71ouIwzVLhlJWIanSbdAZG6PAFmJadlN0TA0NTJz10fZfdM3Qr6%2Bhbr3SU%2BQCulC8rNUfnOhlO5wez2DZbD4Mb4vPbhGD%2BRJGMC4XCWCymXimJ52OgAH0eEXsz1eHzkwlMTGIMEsKXSmWyuXyhXRJmCDFabKxKzBahAEhAnwY0DgJHIiBAAGI6ABVYVQVKkOhgT7oADGwtw6DgCQCyN4NFQZnQnxo2HkoTsDyeEM2UJhypUIAAcqbzZRbMB8ABfJ18gVkD5gKCkQgMWhQCjS8qkIMhx4YHAEOhqrWQNjfI0QLXxdDSgDKMBgdDSDAYxB6AHpK4HDCHCLw2JWYOhK5hcGq4JXU%2Bh05nNa3lbw6KgZKhoI1iz2%2BzkB9JiAPRWoyOktU45JQ4NmXgBeOhOgDMhAAjAAmP3oEDegWoDUQOQAMWgMAoaCweCIZCvQA)

I'm not exactly sure why do other type checkers emit a diagnostic here, maybe there's an unsoundness argument to this behavior?

---

_Label `needs-decision` added by @dhruvmanila on 2026-01-20 04:05_

---

_Label `calls` added by @dhruvmanila on 2026-01-20 04:05_

---

_Label `overloads` added by @dhruvmanila on 2026-01-20 04:05_

---
