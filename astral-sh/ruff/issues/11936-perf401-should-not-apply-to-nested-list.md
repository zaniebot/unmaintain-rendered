```yaml
number: 11936
title: PERF401 should not apply to nested list comprehensions
type: issue
state: closed
author: knyazer
labels: []
assignees: []
created_at: 2024-06-19T09:16:19Z
updated_at: 2024-06-23T16:32:55Z
url: https://github.com/astral-sh/ruff/issues/11936
synced_at: 2026-01-10T11:09:54Z
```

# PERF401 should not apply to nested list comprehensions

---

_Issue opened by @knyazer on 2024-06-19 09:16_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
Keywords: PERF401, nested comprehension, list comprehension
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


### Description
PERF401 recommends to use list comprehensions even if it leads to nested list comprehensions. I would guess a lot of people agree that nested list comprehensions are a bad code practice, but I cannot find any sources, sadly.

### Minimal example

```py
# test.py
def func():
    mylist = []
    for iterable in [[1], [2], [3]]:
        mylist.append([el + 1 for el in iterable])
    return mylist
```
When you run ```ruff check --select PERF401 test.py```, you would get a recommendation to use a list comprehension.

While minimal example does not look this bad, here is a real world example:
### Real example
```py
times = [
    [transcription.captions[index].end for index in range(len(transcription.captions) - 1)]
    for transcription in [*transcriptionStorage.get(), new_transcription]
    if transcription.isValid()
]
```
and wow, this is awful!

### Possible solution
I'm not sure how ruff internals work, but I guess it should not be extremely hard to figure out that there is a list comprehension inside of the block where the PERF401 is detected.

### Other
```ruff --version # 0.4.5```



---

_Comment by @charliermarsh on 2024-06-23 16:32_

I see the motivation but I feel like this is working as intended. The rule is looking for iterations that could be rewritten as comprehensions for micro-optimization purposes, and it's identifying one here. To take the other side of the argument, it would be surprising for `PERF401` to sometimes trigger and sometimes not depending on the target expression. (Candidly, I don't find the real example to be much harder to read, but I recognize that's just subjective).

It's kind of a niche rule -- I'd recommend just disabling it if you're not in a situation that demands it, and aren't happy with the suggestions.


---

_Closed by @charliermarsh on 2024-06-23 16:32_

---
