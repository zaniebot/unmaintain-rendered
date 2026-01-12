```yaml
number: 13271
title: uv pip compile --format pylock.toml with github based dependencies leave the github token in the resultant pylock.toml
type: issue
state: closed
author: gary-wall-hpe
labels:
  - bug
assignees: []
created_at: 2025-05-02T18:00:12Z
updated_at: 2025-05-02T19:51:44Z
url: https://github.com/astral-sh/uv/issues/13271
synced_at: 2026-01-12T16:01:23Z
```

# uv pip compile --format pylock.toml with github based dependencies leave the github token in the resultant pylock.toml

---

_@gary-wall-hpe_

### Summary

Example requirements.in
somemodule@https://${GITHUB_TOKEN}@github.com/someorg/somemodule/archive/somesha.zip

Expected output: resultant pylock.toml has ${GITHUB_TOKEN} rather than the actual substituted in github token.

Actual output: pylock.toml format has the github token in the URL in the resultant file. requirements.txt format correctly has ${GITHUB_TOKEN} in the resultant file rather than the actual github token.

Implication: anyone with a secret scanner cannot commit a pylock.toml if it has a gh hosted module dependency that requires authentication and thus authorization to access.

### Platform

debian 12

### Version

uv 0.6.17

### Python version

3.12.10

---

_Label `bug` added by @gary-wall-hpe on 2025-05-02 18:00_

---

_Comment by @charliermarsh on 2025-05-02 18:07_

Thanks, but I don't think writing out the environment variable is an option here. That wouldn't abide by the spec. The alternative is we redact all credentials, like for `uv.lock`.

---

_Comment by @gary-wall-hpe on 2025-05-02 18:12_

requirements.txt format is doing this correctly -- if PEP 751 infers you must expose secrets in the pylock.toml then the standard is DOA, no?

---

_Comment by @charliermarsh on 2025-05-02 18:23_

I think putting an environment variable in the URL output like that is kind of a hack -- it means the thing you write there is no longer a valid URL on its own. I don't think any ecosystem does this? Personally, I think the right solution is to omit credentials from the output entirely and give the user the ability to provide credentials out-of-band from the lockfile.


---

_Comment by @gary-wall-hpe on 2025-05-02 18:37_

I respect the position -- just offering, this is something pip and uv support with requirements.txt format.

---

_Comment by @gary-wall-hpe on 2025-05-02 18:56_

For the record, "pip lock" pylock.toml output file presently has this issue too.

---

_Comment by @gary-wall-hpe on 2025-05-02 19:51_

PEP751 specifically calls out not supporting environment variables, making this moot.

---

_Closed by @gary-wall-hpe on 2025-05-02 19:51_

---
