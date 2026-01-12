```yaml
number: 3554
title: placeholders in strings are not being fixed to f-strings
type: issue
state: closed
author: globau
labels: []
assignees: []
created_at: 2023-03-16T06:57:45Z
updated_at: 2023-03-16T15:20:10Z
url: https://github.com/astral-sh/ruff/issues/3554
synced_at: 2026-01-12T15:54:43Z
```

# placeholders in strings are not being fixed to f-strings

---

_@globau_

Using `ruff 0.0.256`, given

```
location = "world"
print("hello %s" % location)
```

Running the following generates no errors or fixes: `ruff check --isolated --select UP --fix test`

I expect the following:

```
location = "world"
print(f"hello {location}")
```

If my reading of the rules and other related issues is correct UP031+UP032 should apply here.

Note the following is correctly fixed, so UP032 is working:

```
location = "world"
print("hello {}".format(location))
```


---

_Comment by @globau on 2023-03-16 07:00_

Of course immediately after I submit this, I see this is already covered in #3549

---

_Closed by @globau on 2023-03-16 07:00_

---

_Comment by @charliermarsh on 2023-03-16 15:20_

Hahhh yes I ran into this myself yesterday. Will take a look, thanks for filing :)

---
