```yaml
number: 2393
title: "Implement `flake8-requests`"
type: issue
state: open
author: takkaria
labels:
  - plugin
assignees: []
created_at: 2023-01-31T13:01:30Z
updated_at: 2024-06-04T15:20:43Z
url: https://github.com/astral-sh/ruff/issues/2393
synced_at: 2026-01-12T15:54:42Z
```

# Implement `flake8-requests`

---

_@takkaria_

It's really nice to have a linter catch when you don't provide a timeout to a request call (`r2c-requests-use-timeout`).

Though I can't find it on GitHub, only on [PyPi](https://pypi.org/project/flake8-requests/).



---

_Comment by @ngnpope on 2023-01-31 13:20_

Looks like `bento.dev` no longer resolves and the Wayback Machine points to `semgrep.dev`...

Anyway, rules: 

- [ ] [`r2c-requests-no-auth-over-http`](https://semgrep.dev/r?q=python.requests.security.no-auth-over-http.no-auth-over-http): Alerts when auth param is possibly used over http://, which could expose credentials.
- [ ] `r2c-requests-use-scheme`: Alerts when URLs passed to requests API methods don't have a URL scheme (e.g., https://), otherwise an exception will be thrown.
- [x] [`r2c-requests-use-timeout`](https://semgrep.dev/r?q=python.requests.best-practice.use-timeout.use-timeout): This check detects when a requests API method has been called without a timeout. requests will hang forever without a timeout; add a timeout to prevent this behavior.
  - Looks to be already implemented via `S113` from [`flake8-bandit`](https://github.com/charliermarsh/ruff#flake8-bandit-s)


---

_Label `plugin` added by @charliermarsh on 2023-01-31 14:13_

---

_Comment by @benjamb on 2024-06-04 15:20_

I was about to suggest the same for [`flake8-timeout`](https://github.com/jkittner/flake8-timeout), but stumbled across this. The added bonus with `flake8-timeout` is that it also catches cases where no timeout is passed to `urllib.request.open` as well.

---
