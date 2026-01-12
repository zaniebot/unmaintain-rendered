```yaml
number: 10072
title: ERA001 false positive on examples in comments
type: issue
state: open
author: 9999years
labels:
  - rule
assignees: []
created_at: 2024-02-21T18:47:51Z
updated_at: 2024-10-31T04:22:12Z
url: https://github.com/astral-sh/ruff/issues/10072
synced_at: 2026-01-12T15:54:49Z
```

# ERA001 false positive on examples in comments

---

_@9999years_

Ruff treats this as commented-out code, despite being an example.

```
# NOTE: Only upper-case fields in `extra` will be sent to `journald`.
# For instance, this:
#
#     logging.error("Uh oh!", extra={"payload": "my-payload"})
#
# Will not show up as a `payload` field in `journalctl`, but this will:
#
#     logging.error("Uh oh!", extra={"PAYLOAD": "my-payload"})
journal_handler = JournalHandler()
```

It would be nice if Ruff could recognize "for example" or `` ```python ``-blocks as "example code".

---

_Label `wish` added by @MichaReiser on 2024-02-22 15:13_

---

_Comment by @MichaReiser on 2024-02-22 15:15_

Hmm that's an interesting one. The rule is working as intended but I can see how it is undesired in this specific case. 

I would favor the solution where the rule ignores `Python` code blocks, although that would require that the rule supports some form of markdown parsing, which seems a bit much. 

---

_Comment by @ottaviohartman on 2024-02-22 22:54_

It's a bit of a hack, but you can use triple quotes to get around this case https://play.ruff.rs/42481cc7-c07b-4158-9b6b-c65f0be7c09c

---

_Label `rule` added by @MichaReiser on 2024-02-23 07:49_

---

_Label `wish` removed by @MichaReiser on 2024-02-23 07:49_

---

_Comment by @kurellajunior on 2024-04-15 13:04_

Mine is different but matches the title. Maybe the rule is too eager? It worked on ruff 0.2.1, FP on 0.3.7

```python
    # case 1: both codes are CREATE
    if cond1:
        pass
    # case 2: one code is a CREATE, the other is an existing 
    elif cond2:
        pass
    # case 3: both codes are existing
    elif cond3:
        pass
```

removing the colons removes the FP, is that how it is meant to be?

---

_Comment by @rmorshea on 2024-08-22 18:17_

A reasonable heuristic might be to allow code in comments so long as there's a preceding explanation. In particular, this means the rule would check that a commented code block is immediately preceded by comment lines with non-code content.

Ok cases:

```python
# here's an example:
# x = y + z
```

```python
# here's an example:
#
# x = y + z
```

Bad cases:

```python
# x = y + z
```

```python
#
# x = y + z
```

```python
# here's an example:

# x = y + z
```

```python
more_code = here  # here's an example:
# x = y + z
```

---

_Comment by @rmorshea on 2024-08-22 18:30_

@kurellajunior I'd argue your issue is a bit different. You're using natural language but ruff is identifying it as code. This particular issue focuses on cases in which you actually want to include code in your comment because it serves as an example.

More specifically, `case N:` is likely being confused with the `match` statement syntax. If you change to use `condition N:` I suspect it won't match anymore. Granted, that still looks like an issue since what follows the `:` should be enough to ignore the line.

---

_Comment by @kurellajunior on 2024-08-26 21:42_

> @kurellajunior I'd argue your issue is a bit different. You're using natural language but ruff is identifying it as code. This particular issue focuses on cases in which you actually want to include code in your comment because it serves as an example.
> 

Fully agree, it is a bit different. I thought it might still fit here, if someone is looking into the rule, to make it more robust to keep this case in mind too. I am happy to open a separate issue if this would be prefered.

And yes, changing the wording to a synonym solves the issue (or removing the colon or else). So I considered it not pressing enough for a separate issue



---

_Comment by @Danipulok on 2024-10-31 04:21_

I have the same issue:
```python
# simple comment1
# TASKS: processing
# simple comment2
```

`ruff check --select=ERA001 mre.py`:
```bash
$ ruff check --select=ERA001 mre.py 
mre.py:2:1: ERA001 Found commented-out code
  |
1 | # simple comment1
2 | # TASKS: processing
  | ^^^^^^^^^^^^^^^^^^^ ERA001
3 | # simple comment2
  |
  = help: Remove commented-out code

Found 1 error.
```

```bash
$ ruff --version
ruff 0.6.9
```

---
