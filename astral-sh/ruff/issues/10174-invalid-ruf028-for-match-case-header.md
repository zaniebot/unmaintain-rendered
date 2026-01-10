```yaml
number: 10174
title: Invalid RUF028 for match case header
type: issue
state: closed
author: qartik
labels:
  - bug
  - rule
assignees: []
created_at: 2024-02-29T16:44:37Z
updated_at: 2024-03-01T08:36:24Z
url: https://github.com/astral-sh/ruff/issues/10174
synced_at: 2026-01-10T11:09:52Z
```

# Invalid RUF028 for match case header

---

_Issue opened by @qartik on 2024-02-29 16:44_

[The doc](https://docs.astral.sh/ruff/formatter/#format-suppression) suggests:

> `# fmt: skip` comments suppress formatting for a preceding statement, **case header**, decorator, function definition, or class definition

but for the following use case, I get a RUF028 warning. I believe this should be acceptable.

```py
string = "hello"
match string:
    case ("C"
        | "CX"
        | "R"
        | "RX"
        | "S"
        | "SP"
        | "WAP"
        | "XX"
        | "Y"
        | "YY"
        | "YZ"
        | "Z"
        | "ZZ"
    ):  # fmt: skip
        pass
    case _:
        pass
```

```
❯ ruff --version
ruff 0.3.0
```

relevant ruff.toml setting:
```
[format]
preview = true
```

---

_Label `bug` added by @AlexWaygood on 2024-02-29 16:50_

---

_Label `rule` added by @AlexWaygood on 2024-02-29 16:50_

---

_Assigned to @snowsignal by @MichaReiser on 2024-02-29 18:11_

---

_Comment by @MichaReiser on 2024-02-29 18:12_

I agree. This looks correct. 
@snowsignal would you mind taking a look at this?

---

_Closed by @snowsignal on 2024-03-01 08:36_

---
