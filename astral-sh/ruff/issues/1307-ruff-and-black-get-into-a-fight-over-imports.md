```yaml
number: 1307
title: ruff and black get into a fight over imports?
type: issue
state: closed
author: alex
labels: []
assignees: []
created_at: 2022-12-21T00:47:18Z
updated_at: 2022-12-21T01:34:25Z
url: https://github.com/astral-sh/ruff/issues/1307
synced_at: 2026-01-12T15:54:41Z
```

# ruff and black get into a fight over imports?

---

_@alex_

Hi, I'm trying out using ruff on https://github.com/pyca/cryptography

If I use `flake8-to-rust setup.cfg` and then copy that into `pyproject.toml` and then I run `ruff .` it complains about many things. So I run `ruff --fix .` and it is happy, but then running `black src/ tests/` reformats things back.

It's my understanding that ruff is supposed to play nicely with black, but I'm not positive which side is at fault here.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-21 01:02_

---

_Comment by @charliermarsh on 2022-12-21 01:05_

Oh, hi, thanks for trying Ruff!

I believe that the source of disagreement comes from a difference in `line-length` settings. So Ruff is formatting imports based on a line length of 88 (its default, the same as Black's default), but Black is formatting based on `line-length = 79` from the `pyproject.toml`, and the difference is causing them to repeatedly correct each others' line breaks.

If you add this to the `pyproject.toml`, it should stabilize them:

```toml
[tool.ruff]
line-length = 79
```

(Note that it's normal for a `black` invocation after a `ruff --fix .` to result in changes, but that running `ruff --fix .` over the changed files should _not_ cause any further changes.)


---

_Comment by @charliermarsh on 2022-12-21 01:06_

(I should upgrade `flake8-to-ruff` to respect `tool.black.line-length`.)

---

_Comment by @charliermarsh on 2022-12-21 01:10_

(I ran this over `cryptography` and it seemed to work correctly, but let me know if you continue to see issues.)

---

_Comment by @alex on 2022-12-21 01:14_

ahhhhhh. indeed, that was the magic piece. thank you! I'm mortified I missed that and therefore totally misdiagnosed this!

---

_Closed by @alex on 2022-12-21 01:14_

---

_Comment by @charliermarsh on 2022-12-21 01:16_

No, all good! (It looks like we do preserve Flake8's `max-line-length` setting, but not the Black variant from `pyproject.toml`.)

---

_Comment by @alex on 2022-12-21 01:21_

with that squared away, adoption was trivial: https://github.com/pyca/cryptography/pull/7920

Great job!

---

_Comment by @charliermarsh on 2022-12-21 01:27_

Woah, that's awesome! Thanks tons for trying it out. (I'm glad `flake8-to-ruff` worked for you too, I've been wondering whether that was worth maintaining :))

---

_Comment by @alex on 2022-12-21 01:34_

It was a big time saver. The two bits i had to manually translate were the line-lengths, and `application-import-names` (which is from `flake8-import-order`).

---
