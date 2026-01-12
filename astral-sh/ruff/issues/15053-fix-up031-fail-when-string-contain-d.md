```yaml
number: 15053
title: "--fix \"UP031\" fail when string contain `%d`"
type: issue
state: closed
author: un-pogaz
labels: []
assignees: []
created_at: 2024-12-19T07:49:25Z
updated_at: 2024-12-19T08:11:01Z
url: https://github.com/astral-sh/ruff/issues/15053
synced_at: 2026-01-12T15:54:54Z
```

# --fix "UP031" fail when string contain `%d`

---

_@un-pogaz_

Ruff fail to automaticly convert legacy Python2 string format to Python3 format when the original string contain `%d`, even with `--unsafe-fixes` enable.


### Example

`ruff check --fix --unsafe-fixes --select="UP031"`
```python
'%s: %d' % (name, value) ==> '%s: %d' % (name, value)   # conversion fail

'%s: %s' % (name, value) ==> '{}: {}'.format(name, value)
```

`ruff check --fix --unsafe-fixes --select="UP031,UP032"`
```python
'%s: %d' % (name, value) ==> '%s: %d' % (name, value)  # conversion fail

'%s: %s' % (name, value) ==> f'{name}: {value}'
```


### Expected

`ruff check --fix --unsafe-fixes --select="UP031"`
```python
'%s: %s' % (name, value) ==> '{}: {}'.format(name, value)

'%s: %s' % (name, value) ==> '{}: {}'.format(name, value)
```

`ruff check --fix --unsafe-fixes --select="UP031,UP032"`
```python
'%s: %s' % (name, value) ==> f'{name}: {value}'

'%s: %s' % (name, value) ==> f'{name}: {value}'
```

---

_Renamed from "--fix `UP031` fail when string contain `%d`" to "--fix "UP031" fail when string contain `%d`" by @un-pogaz on 2024-12-19 07:57_

---

_Closed by @dhruvmanila on 2024-12-19 08:11_

---
