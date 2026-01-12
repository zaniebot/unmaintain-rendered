```yaml
number: 6352
title: formatter prefers to break annotation over arguments list
type: issue
state: closed
author: davidszotten
labels:
  - formatter
assignees: []
created_at: 2023-08-04T20:18:43Z
updated_at: 2023-08-16T07:11:04Z
url: https://github.com/astral-sh/ruff/issues/6352
synced_at: 2026-01-12T15:54:46Z
```

# formatter prefers to break annotation over arguments list

---

_@davidszotten_

input:

```python
def foo(aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa) -> aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa:
    ...
```

black (better choice imo)

```python
def foo(
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa,
) -> aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa:
    ...
```

ruff

```python
def foo(aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa) -> (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
):
    ...
```

---

_Comment by @tylerlaprade on 2023-08-04 20:27_

I'm pretty sure Black is changing this in their next version. Try running Black with the `--preview` flag enabled.

---

_Comment by @davidszotten on 2023-08-04 20:45_

i see the same behaviour even with `--preview` 

(`black, 23.7.0 (compiled: yes)`)

---

_Comment by @MichaReiser on 2023-08-04 21:01_

This shouldn't be to hard to fix. The arguments group must enclose the arguments and the return type annotation. This will make the printer to expand from left to right (outer most before inner groups)

---

_Comment by @tylerlaprade on 2023-08-04 21:28_

https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-line-breaks

Ah, I see, I guess this _only_ applies to assignment. I assumed it would be similar for function definitions.

---

_Comment by @davidszotten on 2023-08-05 07:22_

> This shouldn't be to hard to fix. The arguments group must enclose the arguments and the return type annotation. This will make the printer to expand from left to right (outer most before inner groups)

not sure i quote follow what you mean by "the arguments group". i tried wrapping https://github.com/astral-sh/ruff/blob/1ac2699b5e993716e33607ec8f7544334e5f42a5/crates/ruff_python_formatter/src/statement/stmt_function_def.rs#L84-L86 in a group but that didn't do the trick

---

_Comment by @MichaReiser on 2023-08-05 07:29_

The formatting of `arguments` creates a group. We'll need to create that group on the function level instead so that it includes both the arguments and the returns, similar to [Prettier](https://prettier.io/playground/#N4Igxg9gdgLgprEAuEAzArlMMCW0AEAKnAM4wAUAhtTbXfQ408ywJRL4sP7AA6U+fPwC+IADQgIAB1zQSyUJQBOSiAHcACsoTyUlADZrKAT3kSARkspgA1nBgBlSgFs4AGRxQ4yVAZJwLK1t7BylrTwBzZBgldACQf2ccaNj4kkj9OABFdAh4Hz94gCsSAA8HDOzc-KRffX8JAEdquA1VKV0QShIAWi84ABNB8RAYyhx9SIBhCGdnSmQu-X0R9KgIzIBBGBicc3R4DTglDy8C+viACxhnfQB1S5x4EjCwOAcdJ5wANyfjRbAJDMIG+cQAklAhrAHGAlDgZJtIQ4YMZMucGiApKp-HcrFJFljSMdvt4JJ5-EoYG1KBF5uj4mElBTFiipKRYfCYCMsZ4YHccAMYJdkAAOAAMEiUcGaOCl1NpC1qhQkMEo5n5guFSAATBJ0P5CGrdHUMXBnOZBkMBm5KOt0DS4AAxCBKeY7SKLSgHCAgYTCIA)

---

_Label `formatter` added by @MichaReiser on 2023-08-05 07:31_

---

_Comment by @davidszotten on 2023-08-06 07:29_

sorry i can't get it to work. might have to leave for someone else

---

_Comment by @charliermarsh on 2023-08-07 00:55_

I'll give it a look.

---

_Comment by @charliermarsh on 2023-08-07 01:25_

I don't quite understand how to make the composition work here given that `parenthesized` and related helpers all create groups. IIUC we'd need to somehow pass the `returns` to the `Parameters` formatter, and then change `parenthesized` to allow for content after the maybe-parenthesized nodes that's part of the same group?

---

_Comment by @charliermarsh on 2023-08-07 18:05_

I will revisit this today.

---

_Comment by @MichaReiser on 2023-08-07 18:17_

You probably need to pull in the relevant parts from parenthesized into the function formatting. It's different enough. We did that previously with other formatting too (I can't recall out of my head which formatting it was but it manually creates the groups and sets the node level)

---

_Closed by @charliermarsh on 2023-08-09 12:14_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:11_

---
