```yaml
number: 1767
title: "Ty does not recognize Python file with no extension and shebang #!/usr/bin/env python3 as a python file"
type: issue
state: closed
author: taladar
labels: []
assignees: []
created_at: 2025-12-05T08:43:11Z
updated_at: 2025-12-05T08:44:33Z
url: https://github.com/astral-sh/ty/issues/1767
synced_at: 2026-01-10T01:56:41Z
```

# Ty does not recognize Python file with no extension and shebang #!/usr/bin/env python3 as a python file

---

_Issue opened by @taladar on 2025-12-05 08:43_

### Summary

I just tried

```
uvx ty rabbitmqadmin
```

and got

```
WARN No python files found under the given path(s)
All checks passed!
```

When I rename the same file to rabbitmqadmin.py I get 14 diagnostics.

The file starts with

```
#!/usr/bin/env python3
```

### Version

0.0.1-alpha.31

---

_Comment by @AlexWaygood on 2025-12-05 08:44_

Thanks!

---

_Closed by @AlexWaygood on 2025-12-05 08:44_

---
