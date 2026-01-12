```yaml
number: 12461
title: UP031 does not support %d formatting
type: issue
state: closed
author: inoa-jboliveira
labels:
  - rule
assignees: []
created_at: 2024-07-22T19:00:24Z
updated_at: 2024-11-17T19:04:26Z
url: https://github.com/astral-sh/ruff/issues/12461
synced_at: 2026-01-12T15:54:51Z
```

# UP031 does not support %d formatting

---

_@inoa-jboliveira_

Ruff version 0.5.4

The following conversion codes are currently being ignored by UP031 instead of showing as errors:

```
print("%d" % 1)
print("%i" % 1)
print("%u" % 1)
print("%c" % 1) 
```

E.g.:
```
$ ruff check --select UP --fixable UP --unsafe-fixes --isolated foo.py
All checks passed!
```



The presence of any of these items such as `%d` in a string will make it ignored by UP031 even if it has another supported one, such as `%s`:

```
print("%d - %s" % (1, 2))
```

Please support all of the valid conversion codes from this table: https://docs.python.org/3/library/stdtypes.html#printf-style-string-formatting

Also, if an invalid code is present (either by you not supporting or being invalid), still it should show the error for valid ones. The only difference is that you don't want to supply a fix in this case

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @inoa-jboliveira on 2024-07-22 19:42_

Giving a quick look at the implementation, all this sounds like it is by design. 
There is nothing in the docs that says it, but [I see a test file](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/resources/test/fixtures/pyupgrade/UP031_1.py) that have several formatting lines are being ignored.

It does sound that these options are being accepted only because there is no automated fix instead of because they are fine. There is zero reason for someone to write this in 2024 (or even 10 years ago), as the test file suggests:

```
"%(1)s" % {"1": "bar"}
```

All the old style string formatting options, except for bytestrings, should be deemed errors under UP031. Having a fix available for those cases or not is what should be considered instead.



---

_Label `rule` added by @charliermarsh on 2024-07-25 21:33_

---

_Comment by @dylwil3 on 2024-11-17 18:23_

This appears to have been fixed by #11229, in preview mode at the time of this writing- at least, I can no longer reproduce the "All checks pass!"

[Playground link](https://play.ruff.rs/9b318e0c-ecbc-4f09-8c1a-f24ed7e08edc)

---

_Comment by @inoa-jboliveira on 2024-11-17 18:59_

I still get the same behavior with latest ruff

```python
$ cat foo.py
print("%d" % 1)
print("%i" % 1)
print("%u" % 1)
print("%c" % 1)

$ ruff check --select UP --fixable UP --unsafe-fixes --isolated foo.py
All checks passed!

$ ruff version
ruff 0.7.4 (ed7b98cf9 2024-11-15)
```

To validate the check is actually running, if I add a 5th line with "%s", that line **only** gets an error 

---

_Comment by @inoa-jboliveira on 2024-11-17 19:03_

Ohh sorry!! I forgot to add the "--preview". 
Yes, seem like this issue can be closed then

---

_Closed by @inoa-jboliveira on 2024-11-17 19:03_

---

_Comment by @dylwil3 on 2024-11-17 19:03_

Hmm - would you mind trying one more time but with the preview flag `--preview`? Apologies if I'm getting something backwards here

---

_Comment by @inoa-jboliveira on 2024-11-17 19:04_

Yes, thank you!

---
