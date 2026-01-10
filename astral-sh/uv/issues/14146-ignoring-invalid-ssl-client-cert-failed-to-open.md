---
number: 14146
title: "Ignoring invalid `SSL_CLIENT_CERT`: failed to open file `/Library/Application\\ Support/path/to/client-cert.pem'"
type: issue
state: closed
author: joewwt
labels:
  - question
assignees: []
created_at: 2025-06-19T17:10:50Z
updated_at: 2025-06-24T14:50:18Z
url: https://github.com/astral-sh/uv/issues/14146
synced_at: 2026-01-10T01:25:42Z
---

# Ignoring invalid `SSL_CLIENT_CERT`: failed to open file `/Library/Application\ Support/path/to/client-cert.pem'

---

_Issue opened by @joewwt on 2025-06-19 17:10_

### Summary

I cannot get uv to verify or use the SSL_CLIENT_CERT on for my corporate machine. I've set in /.zshrc an export for SSL_CLIENT_CERT to the path.

The file path/directory does appear to be hidden normally, I can cd <dir> and the exports appear to be set, but maybe uv doesn't have proper permissions to access this CLIENT_CERT?

The goal is to log into huggingface to download data: 
`print("Logging in to Huggingface... using token:", os.getenv("HF_TOKEN"), "and REQUESTS_CA_BUNDLE:", os.getenv("REQUESTS_CA_BUNDLE"))`

Output:
`Logging in to Huggingface... using token: None and REQUESTS_CA_BUNDLE: None`

When using the VSCode terminal (zsh) it appears to populate the variables fine. When using my terminal directly and the output returns None for both variables.

### Platform

macOS Darwin 24.5.0 arm64

### Version

uv 0.7.13 (62ed17b23 2025-06-12)

### Python version

Python 3.12.4

---

_Label `bug` added by @joewwt on 2025-06-19 17:10_

---

_Comment by @konstin on 2025-06-20 07:44_

Is that a problem with uv specifically, or does it also happen when running the huggingface Python code directly without uv?

---

_Label `bug` removed by @konstin on 2025-06-20 07:44_

---

_Label `question` added by @konstin on 2025-06-20 07:44_

---

_Comment by @joewwt on 2025-06-20 14:28_

It appears to be within 'uv', I was able to add the keys via `export "key path"`, and when I run from my terminal independent of VSCode, it does appear to run. When I run inside vscode, or when I do os.environ['key'] = os.getenv('key') in python, it appears uv doesn't work, I've added:

```
[tool.uv]
native-tls = true
```

to my pyproject.toml.

There is whitespace in the path, so the double quotes are necessary.

I'm adding some error handling to my application to verify and update the paths, I'll update if this resolves the issue. Thank you!

---

_Comment by @konstin on 2025-06-22 14:49_

Sorry, I don't quite follow your explanation. Are you sure this is a problem with uv, does it happen when running Python without uv? Can you share a minimal reproducible example of the problem (https://github.com/astral-sh/uv/issues/9452)?

---

_Comment by @joewwt on 2025-06-24 14:50_

Looks like it might be a corporate issue. Apologies for the wasted time!

---

_Closed by @joewwt on 2025-06-24 14:50_

---
