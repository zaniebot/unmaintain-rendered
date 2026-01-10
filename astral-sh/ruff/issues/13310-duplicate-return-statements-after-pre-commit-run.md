```yaml
number: 13310
title: duplicate return statements after pre-commit run
type: issue
state: closed
author: mpurusottamc
labels:
  - needs-mre
assignees: []
created_at: 2024-09-10T16:20:39Z
updated_at: 2024-09-18T03:24:00Z
url: https://github.com/astral-sh/ruff/issues/13310
synced_at: 2026-01-10T11:09:55Z
```

# duplicate return statements after pre-commit run

---

_Issue opened by @mpurusottamc on 2024-09-10 16:20_

Some times, after running pre-commit, i notice duplicate return statements in the last function of the file.

Ex:
```python
return api_response

return api_response
```

See screenshot here for reference.
![Screenshot 2024-09-10 at 9 14 35 AM](https://github.com/user-attachments/assets/827de607-4e86-4977-b810-9ae7f62e57f8)


Here's the pre-commit command I run:
```shell
pre-commit run --all-files --show-diff-on-failure
```

Here's how my `.pre-commit-config.yaml` looks like:
```yaml
  - repo: https://github.com/astral-sh/ruff-pre-commit
    # Ruff version.
    rev: v0.6.4
    hooks:
      - id: ruff
        args:
          [--fix, --exit-non-zero-on-fix, "--line-length=120", "--ignore=E501"]
      - id: ruff-format
```

Sometimes, i have noticed, there are > 5 return statements. Depending on how many times pre-commit was run. I have not been able to reproduce repeatedly. Will share if I am able to find the steps.

Ruff Version: `ruff 0.1.9`

---

_Comment by @MichaReiser on 2024-09-10 17:50_

Hmm, that sounds obscure. 

I don't know if it is related but it seems that pre-commit uses Ruff 0.6.4 but you use Ruff 0.1.9 locally. Can you try upgrading your local ruff version?

---

_Comment by @mpurusottamc on 2024-09-10 19:30_

> I don't know if it is related but it seems that pre-commit uses Ruff 0.6.4 but you use Ruff 0.1.9 locally. Can you try upgrading your local ruff version?

@MichaReiser I have updated to the latest version for Ruff. I will keep an eye out to see if it happens again. Thanks for your quick response.

---

_Comment by @charliermarsh on 2024-09-18 03:23_

Let's close for now, and we can always reopen if it comes up again. Thank you!

---

_Closed by @charliermarsh on 2024-09-18 03:23_

---

_Label `needs-mre` added by @charliermarsh on 2024-09-18 03:24_

---
