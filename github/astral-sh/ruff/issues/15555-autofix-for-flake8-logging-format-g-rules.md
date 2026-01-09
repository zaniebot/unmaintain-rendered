---
number: 15555
title: Autofix for flake8-logging-format (G) rules
type: issue
state: open
author: justinchuby
labels:
  - fixes
assignees: []
created_at: 2025-01-17T17:30:20Z
updated_at: 2025-09-16T16:58:48Z
url: https://github.com/astral-sh/ruff/issues/15555
synced_at: 2026-01-07T13:12:16-06:00
---

# Autofix for flake8-logging-format (G) rules

---

_Issue opened by @justinchuby on 2025-01-17 17:30_

Hi! I was wondering if there are any plans to create autofix for (G) rules? We have a lot of violations that prevents enabling the rule. Having autofix capabilities (even if unsafe) would greatly help with adoption of the (G) rules in our codebase.

---

_Comment by @MichaReiser on 2025-01-17 17:48_

Are you looking for a specific rule or is it really all rules? The format fixes tend to be rather difficult to implement because of all the edge cases and adding more fixes isn't a priority for us right now (not saying that it wouldn't be great to have them, it's just there are so many other improvements)

---

_Label `fixes` added by @MichaReiser on 2025-01-17 17:48_

---

_Comment by @justinchuby on 2025-01-17 17:51_

The current rule we need the most in microsoft/onnxruntime is G004. Because everything else can be turned into an f-string first.

---

_Referenced in [astral-sh/ruff#15727](../../astral-sh/ruff/issues/15727.md) on 2025-01-24 18:27_

---

_Comment by @ktbarrett on 2025-01-24 18:39_

I will second really only needing an autofix for G004.

~~G001 looks trivial as both logging methods and `str.format` use `string.Formatter`.
G002 could involve transforming to `str.format` first, then trivially converting to the logging call.~~
EDIT: actually it's slightly different. And of course keyword arguments.

---

_Comment by @bmos on 2025-02-27 03:41_

If we're just talking about G004, it seems like it should be pretty easy to copy the fix for EM102 with small changes.

---

_Comment by @ntBre on 2025-02-27 23:44_

> If we're just talking about G004, it seems like it should be pretty easy to copy the fix for EM102 with small changes.

I thought the same thing when I looked into this a few weeks ago, but the point of G004 is not simply to move the f-string out of the `logging` call, it's to avoid formatting the string at all when the log level is set high enough. See this part of the rule docs in particular:

> Using f-strings to format a logging message requires that Python eagerly format the string, even if the logging statement is never executed (e.g., if the log level is above the level of the logging statement), whereas using the extra keyword argument defers formatting until required.

Moving the f-string out of the `logging` call like in EM102 just eagerly formats the string a little earlier.

Properly fixing G004 requires inspecting the contents of the f-string and converting them to the appropriate `logging` method call with printf-style arguments and placeholders, at least as far as I can tell.

Now, if you simply want to defeat the rule, moving the f-string out of the `logging` call *does* help because the rule doesn't do any type inference on its argument. It only checks for literal f-strings and other formatting calls. But at that point you might as well `noqa` or ignore the rule entirely.

---

_Comment by @ktbarrett on 2025-02-28 02:43_

So what I'm hearing is that this is a valid fix?

```
if self.log.isEnabledFor(logging.DEBUG):
    self.log.debug(f"{name=}")
```

/s

---

_Comment by @justinchuby on 2025-02-28 03:42_

Is it possible to just use `%s` as placeholders? (unless if there are format specifier, but should still be straightforward)

---

_Comment by @ntBre on 2025-02-28 18:11_

> Is it possible to just use `%s` as placeholders? (unless if there are format specifier, but should still be straightforward)

Hmm, that's a good point. The logging module eventually just calls `format_string % args` [here](https://github.com/python/cpython/blob/ab11c097052757b79060c75dd4835c2431e752b7/Lib/logging/__init__.py#L468), and without format specifiers, the f-string and `format` docs say:

> A general convention is that an empty format specification produces the same result as if you had called [str()](https://docs.python.org/3/library/stdtypes.html#str) on the value.

That also sounds equivalent to the entry in the [old string formatting](https://docs.python.org/3/library/stdtypes.html#old-string-formatting) table:

>| Conversion | Meaning |Notes |
>| --|--|--|
 >| 's'  | String (converts any Python object using [str()](https://docs.python.org/3/library/stdtypes.html#str)). | (5) |

So yeah, based on my reading of those docs, `"%s other contents", arg` should be a suitable (unsafe) fix for `f"{arg} other contents"`, which might be the most common case anyway?

Maybe I will take another look at this! I'd really like to offer a fix here, at least for simple cases.

Contributions would be welcome here too, of course. I'm not sure how soon I could work on it.

---

_Comment by @ktbarrett on 2025-02-28 19:40_

And `f"{thing!r}` is equivalent to calling `repr()` and `f"{thing=}"` is equivalent to `"thing=%s", thing`. There's a bunch of low hanging fruit, it's just when it gets into things like left justification, pad character, etc. does the whole string formatting thing get hairy. But starting with the default formatting and `!r` specifier would hit 95+% of the 5k+ errors I have.

---

_Comment by @aristidebm on 2025-06-20 01:41_

In the mean time I have implemented an autofix like feature for [G004](https://docs.astral.sh/ruff/rules/logging-f-string/)  with ast-grep transformation rules https://ast-grep.github.io/reference/yaml/rewriter.html#transform, you may find it useful, it is not prefect but it works for my use case.

```yaml
id: no-fstring-in-logging
language: python
rule:
  any:
    - pattern: $LOGGER.$METHOD($IDENTS)
    - pattern: $LOGGER.$METHOD($IDENTS, $$$KWARGS)
constraints:
  METHOD:
    regex: ^(trace|debug|info|warning|warn|error|log)$
  LOGGER:
    any:
      - kind: identifier
      - kind: attribute
  IDENTS:
    regex: 'f"[^"]*"'
rewriters:
- id: rewrite-identifier
  rule:
    pattern: $VAR
    any:
      - kind: identifier
      - kind: attribute
  fix: $VAR
transform:
  ARGS:
    rewrite:
      rewriters: [rewrite-identifier]
      source: $IDENTS
      joinBy: ", "
  MESSAGE:
    replace:
      source: $IDENTS
      replace: 'f"([^"]*)"'
      by: '"$1"'
  FINAL_MESSAGE:
    replace:
      source: $MESSAGE
      replace: '\{[^}]+\}'
      by: '%s'
fix: $LOGGER.$METHOD($FINAL_MESSAGE, $ARGS, $$$KWARGS)
```

just run the following command, assuming you have stored the rule above in  `/path/to/rules.yml`

```
$AST_GREP_COMMAND scan --interactive --rule /path/to/rules.yml
```

---

_Comment by @ktbarrett on 2025-06-20 15:16_

I had hoped that t-strings would save us a lot here, but they don't do lazy evaluation anymore.

Maybe it's best to just do explicit lazy evaluation in the cases it matters and accept the lower performance.
```python
class LazyLog:
    def __init__(self, func: Callable[[], object]) -> None:
        self.func = func
    def __format__(self, fmt: str) -> str:
        val = self.func()
        return val.__format__(fmt)
```

---

_Referenced in [astral-sh/ruff#19302](../../astral-sh/ruff/issues/19302.md) on 2025-07-12 22:46_

---

_Comment by @spaceone on 2025-09-10 21:15_

When autofix for this rule would be available, a lot of time would be saved.

---

_Comment by @ntBre on 2025-09-10 23:01_

There is now a [preview autofix](https://github.com/astral-sh/ruff/pull/19303) for simple cases of G004 if you want to try it out! As of Ruff 0.12.11, ~2 weeks ago

---

_Comment by @spaceone on 2025-09-10 23:10_

I already used G004 fixer from 0.12.11, but it did not fix much cases (4 of several hundreds):

I now started with the approach of @aristidebm and took:
`ast-grep scan  --rule log.yaml  -U` plus `ruff check --select COM819,E202 --fix `

```yaml
id: no-fstring-in-logging
language: python
rule:
  any:
    - pattern: $LOGGER.$METHOD($IDENTS)
    - pattern: $LOGGER.$METHOD($IDENTS, $$$KWARGS)
constraints:
  METHOD:
    regex: ^(trace|debug|info|process|warning|warn|error|log|exception)$
  LOGGER:
    any:
      - kind: identifier
      - kind: attribute
  IDENTS:
    regex: "f(['\"]).*?['\"]"
rewriters:
- id: rewrite-identifier
  rule:
    pattern: $VAR
    any:
      - kind: identifier
      - kind: attribute
  fix: $VAR
transform:
  ARGS:
    rewrite:
      rewriters: [rewrite-identifier]
      source: $IDENTS
      joinBy: ", "
  MESSAGE:
    replace:
      source: $IDENTS
      replace: "f(['\"])(.*)['\"]"
      by: '"$2"'
  MESSAGE_R:
    replace:
      source: $MESSAGE
      replace: "\\{([^}]+)!r\\}"
      by: "%r"
  FINAL_MESSAGE:
    replace:
      source: $MESSAGE_R
      replace: "\\{[^}]+\\}"
      by: "%s"
fix: $LOGGER.$METHOD($FINAL_MESSAGE, $ARGS, $$$KWARGS)
```


```yaml
id: no-format-in-logging
language: python
rule:
  any:
    - pattern: $LOGGER.$METHOD($MESSAGE.format($$$ARGS))
    - pattern: $LOGGER.$METHOD($MESSAGE.format($$$ARGS), $$$KWARGS)
constraints:
  METHOD:
    regex: ^(trace|debug|info|process|warning|warn|error|log|exception)$
  LOGGER:
    any:
      - kind: identifier
      - kind: attribute
  MESSAGE:
    kind: string
rewriters:
- id: rewrite-arg
  rule:
    pattern: $VAR
    any:
      - kind: identifier
      - kind: attribute
      - kind: subscript
      - kind: call
  fix: $VAR
transform:
  ARGS_REWRITTEN:
    rewrite:
      rewriters: [rewrite-arg]
      source: $$$ARGS
      joinBy: ", "
  MESSAGE_R:
    replace:
      source: $MESSAGE
      replace: "\\{([^}]*)!r\\}"
      by: "%r"
  MESSAGE_S:
    replace:
      source: $MESSAGE_R
      replace: "\\{([^}]*)!s\\}"
      by: "%s"
  MESSAGE_A:
    replace:
      source: $MESSAGE_S
      replace: "\\{([^}]*)!a\\}"
      by: "%a"
  FINAL_MESSAGE:
    replace:
      source: $MESSAGE_A
      replace: "\\{[^}]*\\}"
      by: "%s"
fix: $LOGGER.$METHOD($FINAL_MESSAGE, $ARGS_REWRITTEN, $$$KWARGS)
```

---

_Comment by @ntBre on 2025-09-11 13:23_

Ah interesting! Do you know which kinds of f-strings the autofix didn't fix? Based on the ast-grep rule, it looks like ones with `!r`, `!s`, and `!a` format directives? I suggested only handling the simplest `{var}` -> `%s` cases in the first PR (I assumed that would be most common), but these should be straightforward to handle too.

---

_Comment by @ktbarrett on 2025-09-11 19:31_

It seems to not handle anything that isnt a simple name lookup as well.

Interesting that the one thing that was implemented is not even technically correct. `%s` is not equivalent to passing `None` to `__format__` except by convention.

---

_Comment by @ntBre on 2025-09-11 19:44_

That is interesting, thanks for pointing that out. I guess we should make the fix unsafe in this case, if there's no precise equivalent. Only a case like `{var!s}` would technically be safe to convert to `%s`, if I understand correctly.

---

_Referenced in [astral-sh/ruff#20416](../../astral-sh/ruff/pulls/20416.md) on 2025-09-15 13:25_

---

_Comment by @spaceone on 2025-09-16 16:23_

> Ah interesting! Do you know which kinds of f-strings the autofix didn't fix?

Yes, we often use `!r` for logging user input.
I did a workaround, we actually have everything as `%`-formatter (so we actually need `G002` more than `G004`).  So I used the pyupgrade UP rules, to convert them to f-strings and `.format()`-strings. And then applied the G004 fixer / that ast-rewrite-workaround-with-many-bugs.

We have:
```python
# noqa: CPY001
import logging
log = logging.getLogger()
foo = bar = 'foo'
baz = (foo, bar)
log.info('foo %s' % foo)
log.info('foo %s' % (foo,))
log.info('foo %s %s' % (foo, bar))
log.info('foo %s %%s' % foo, bar)  # caution!
log.info('foo %s %s' % baz)  # caution!
log.info('foo %r' % foo)
log.info('foo %r' % (foo,))
log.info('foo %r %r' % (foo, bar))
log.info('foo %d' % len(foo))
log.info('foo %d' % (len(foo),))
log.info('foo %d %d' % (len(foo), len(bar)))
log.info('foo %0.3f' % len(foo))
log.info('foo %0.3f' % (len(foo),))
log.info('foo %0.3f %0.3f' % (len(foo), len(bar)))
log.info(f'foo {foo}')
log.info(f'foo {foo} {bar}')
log.info(f'foo {foo!r}')
log.info(f'foo {foo!r} {bar!r}')
log.info(f'foo {len(foo):0.3f}')
log.info(f'foo {len(foo):0.3f}')
log.info(f'foo {len(foo):0.3f} {len(bar):0.3f}')
```



---
