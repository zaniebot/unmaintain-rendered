```yaml
number: 851
title: mccabe implementation differs from flake8
type: issue
state: closed
author: kalekseev
labels:
  - bug
assignees: []
created_at: 2022-11-21T13:22:25Z
updated_at: 2022-11-21T18:30:37Z
url: https://github.com/astral-sh/ruff/issues/851
synced_at: 2026-01-12T15:54:40Z
```

# mccabe implementation differs from flake8

---

_@kalekseev_

ruff reports complexity - 11 while flake8 - 9, cc @edgarrmondragon 
```
✦ ❯ flake8 --version
5.0.4 (flake8-bugbear: 22.10.27, flake8-no-pep420: 2.3.0, mccabe: 0.7.0, pep8-naming: 0.13.2, pycodestyle: 2.9.1, pyflakes: 2.5.0)
CPython 3.10.5 on Darwin

✦ ❯ ruff --version
ruff 0.0.132

✦ ❯ cat a.py
class Command:
    def handle(self, *args, **options):
        if args:
            return

        class ServiceProvider:
            def a(self):
                pass

            def b(self, data):
                if not args:
                    pass

        class Logger:
            def c(*args, **kwargs):
                pass

            def error(self, message):
                pass

            def info(self, message):
                pass

            def exception(self):
                pass

        return ServiceProvider(Logger())


✦ ❯ flake8 a.py
a.py:2:5: C901 'Command.handle' is too complex (9)

✦ ❯ ruff --select C a.py
Found 1 error(s).
a.py:2:5: C901 `handle` is too complex (11)
```

BTW flake8 error message also includes class name

---

_Label `bug` added by @charliermarsh on 2022-11-21 15:19_

---

_Comment by @charliermarsh on 2022-11-21 15:25_

Is it that class definitions don't count towards complexity?

---

_Comment by @charliermarsh on 2022-11-21 15:26_

Yeah, this also reports 9, so I'm guessing that's the distinction:

```py
def handle(self, *args, **options):
    if args:
        return
    
    def a(self):
        pass

    def b(self, data):
        if not args:
            pass

    def c(*args, **kwargs):
        pass

    def error(self, message):
        pass

    def info(self, message):
        pass

    def exception(self):
        pass

    return ServiceProvider(Logger())
```

---

_Comment by @charliermarsh on 2022-11-21 15:27_

Maybe because they don't define new / distinct forks in the code vis-a-vis the methods within them.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-21 18:20_

---

_Closed by @charliermarsh on 2022-11-21 18:30_

---
