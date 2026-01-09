---
number: 2384
title: "Don't treat \"passed\" as \"password\" for `S105`/`S106`/`S107`"
type: issue
state: closed
author: ngnpope
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-01-31T08:59:42Z
updated_at: 2023-02-25T20:32:55Z
url: https://github.com/astral-sh/ruff/issues/2384
synced_at: 2026-01-07T13:12:14-06:00
---

# Don't treat "passed" as "password" for `S105`/`S106`/`S107`

---

_Issue opened by @ngnpope on 2023-01-31 08:59_

Using `v0.0.238` I get: `S105 Possible hardcoded password: "..."` when the variable/argument name contains `passed` in it.

It seems less likely that "passed" is being used for something related to passwords/secrets than not.

Example:
```python
passed_msg = "You have passed!"
failed_msg = "You have failed!"
```

Result:
```console
❯ ruff --select S1 bug.py 
bug.py:1:14: S105 Possible hardcoded password: "You have passed!"
Found 1 error.
```

Ahh... But if only you _had_ passed the test... 😏 

---

_Comment by @charliermarsh on 2023-01-31 12:29_

I'm tempted to follow `bandit` here even if it's not totally ideal. (Not sure what their exact behavior is for this.)

---

_Comment by @ngnpope on 2023-01-31 12:31_

So looking at the values:

https://github.com/charliermarsh/ruff/blob/6051a0c1c89341d20a5148b18654d343087f0ae3/src/rules/flake8_bandit/helpers.rs#L3-L5

Because this is currently a simple containment check without checking for word boundary, `"pass"` will already pick up `"password"` and `"passwd"`.

And `secrete` is redundant too, as implemented. Not sure whether we want to handle non-English words as we can't really handle all languages, and there shouldn't be anything wrong with [secrete](https://www.dictionary.com/browse/secrete).

So "key" is too generic, but we could also have "api_key" matched? That would seem reasonable to me.

Further it may make sense to expose this as a configuration option, e.g. `password-names`, and add, e.g. `password-names-extend`, to allow adding additional names to check, or names in other languages if required.

Summary:

- Remove `"pass"` from the default list of password names and/or prevent match on partial words
- Remove `"secrete"` from the default list of password names
- Add `"api_key"` to the default list of password names
- Consider adding `password-names` and `password-names-extend` configuration options

---

_Comment by @ngnpope on 2023-01-31 12:32_

Sorry - cross-posted!

As noted in my follow up, it seems pretty flawed.

In fact, they're doing more like what I suggest:

https://github.com/PyCQA/bandit/blob/a9eaafa53ae73c1a66172373128cb19bb7b82a32/bandit/plugins/general_hardcoded_password.py#L13-L16

---

_Comment by @charliermarsh on 2023-01-31 13:01_

Ahh ok cool.

---

_Label `bug` added by @charliermarsh on 2023-01-31 13:01_

---

_Comment by @ngnpope on 2023-01-31 13:02_

For some more comprehensive information on which to help make a decision:

```python
# These are the words that bandit supports:
password = ""
passwd = ""
# pass = ""  # pass is a keyword...
passphrase = ""
pwd = ""
token = ""
secret = ""
secrete = ""

# Checking with prefixes:
client_password = ""
client_passwd = ""
client_pass = ""
client_passphrase = ""
client_pwd = ""
client_token = ""
client_secret = ""
client_secrete = ""

# Checking with suffixes:
client_password_variable = ""
client_passwd_variable = ""
client_pass_variable = ""
client_passphrase_variable = ""
client_pwd_variable = ""
client_token_variable = ""
client_secret_variable = ""
client_secrete_variable = ""

# Checking with prefixes and suffixes:
client_password_variable = ""
client_passwd_variable = ""
client_pass_variable = ""
client_passphrase_variable = ""
client_pwd_variable = ""
client_token_variable = ""
client_secret_variable = ""
client_secrete_variable = ""

# These seem a bit silly, but bandit supports them - too-loose regex:
paswd = ""
paswod = ""
paswrd = ""
passwod = ""  # Also triggered by ruff due to containment of "pass"
passwrd = ""  # Also triggered by ruff due to containment of "pass"
passssssword = ""  # Also triggered by ruff due to containment of "pass"

# My original examples that don't fail for bandit, but do for rust:
passed_msg = "You have passed!"
failed_msg = "You have failed!"

# And some more words that shouldn't match:
compassion = "Please don't match!"
impassable = "You shall not pass!"

# And some that perhaps should, i.e. plurals. and unhandled common cases:
# These don't work bandit, but do in ruff because of partial substring match.
passwords = ""
passphrases = ""
tokens = ""
secrets = ""

# And some that perhaps should, i.e. unhandled common cases:
# These don't work in either ruff or bandit.
service_api_key = ""
```

Yields:

```console
❯ flake8 bug.py 
Unable to find qualified name for module: bug.py
bug.py:2:1: S105 Possible hardcoded password: ''
bug.py:3:1: S105 Possible hardcoded password: ''
bug.py:5:1: S105 Possible hardcoded password: ''
bug.py:6:1: S105 Possible hardcoded password: ''
bug.py:7:1: S105 Possible hardcoded password: ''
bug.py:8:1: S105 Possible hardcoded password: ''
bug.py:9:1: S105 Possible hardcoded password: ''
bug.py:12:1: S105 Possible hardcoded password: ''
bug.py:13:1: S105 Possible hardcoded password: ''
bug.py:14:1: S105 Possible hardcoded password: ''
bug.py:15:1: S105 Possible hardcoded password: ''
bug.py:16:1: S105 Possible hardcoded password: ''
bug.py:17:1: S105 Possible hardcoded password: ''
bug.py:18:1: S105 Possible hardcoded password: ''
bug.py:19:1: S105 Possible hardcoded password: ''
bug.py:22:1: S105 Possible hardcoded password: ''
bug.py:23:1: S105 Possible hardcoded password: ''
bug.py:24:1: S105 Possible hardcoded password: ''
bug.py:25:1: S105 Possible hardcoded password: ''
bug.py:26:1: S105 Possible hardcoded password: ''
bug.py:27:1: S105 Possible hardcoded password: ''
bug.py:28:1: S105 Possible hardcoded password: ''
bug.py:29:1: S105 Possible hardcoded password: ''
bug.py:32:1: S105 Possible hardcoded password: ''
bug.py:33:1: S105 Possible hardcoded password: ''
bug.py:34:1: S105 Possible hardcoded password: ''
bug.py:35:1: S105 Possible hardcoded password: ''
bug.py:36:1: S105 Possible hardcoded password: ''
bug.py:37:1: S105 Possible hardcoded password: ''
bug.py:38:1: S105 Possible hardcoded password: ''
bug.py:39:1: S105 Possible hardcoded password: ''
bug.py:42:1: S105 Possible hardcoded password: ''
bug.py:43:1: S105 Possible hardcoded password: ''
bug.py:44:1: S105 Possible hardcoded password: ''
bug.py:45:1: S105 Possible hardcoded password: ''
bug.py:46:1: S105 Possible hardcoded password: ''
bug.py:47:1: S105 Possible hardcoded password: ''

❯ ruff --select S105 bug.py 
bug.py:2:12: S105 Possible hardcoded password: ""
bug.py:3:10: S105 Possible hardcoded password: ""
bug.py:5:14: S105 Possible hardcoded password: ""
bug.py:6:7: S105 Possible hardcoded password: ""
bug.py:7:9: S105 Possible hardcoded password: ""
bug.py:8:10: S105 Possible hardcoded password: ""
bug.py:9:11: S105 Possible hardcoded password: ""
bug.py:12:19: S105 Possible hardcoded password: ""
bug.py:13:17: S105 Possible hardcoded password: ""
bug.py:14:15: S105 Possible hardcoded password: ""
bug.py:15:21: S105 Possible hardcoded password: ""
bug.py:16:14: S105 Possible hardcoded password: ""
bug.py:17:16: S105 Possible hardcoded password: ""
bug.py:18:17: S105 Possible hardcoded password: ""
bug.py:19:18: S105 Possible hardcoded password: ""
bug.py:22:28: S105 Possible hardcoded password: ""
bug.py:23:26: S105 Possible hardcoded password: ""
bug.py:24:24: S105 Possible hardcoded password: ""
bug.py:25:30: S105 Possible hardcoded password: ""
bug.py:26:23: S105 Possible hardcoded password: ""
bug.py:27:25: S105 Possible hardcoded password: ""
bug.py:28:26: S105 Possible hardcoded password: ""
bug.py:29:27: S105 Possible hardcoded password: ""
bug.py:32:28: S105 Possible hardcoded password: ""
bug.py:33:26: S105 Possible hardcoded password: ""
bug.py:34:24: S105 Possible hardcoded password: ""
bug.py:35:30: S105 Possible hardcoded password: ""
bug.py:36:23: S105 Possible hardcoded password: ""
bug.py:37:25: S105 Possible hardcoded password: ""
bug.py:38:26: S105 Possible hardcoded password: ""
bug.py:39:27: S105 Possible hardcoded password: ""
bug.py:45:11: S105 Possible hardcoded password: ""
bug.py:46:11: S105 Possible hardcoded password: ""
bug.py:47:16: S105 Possible hardcoded password: ""
bug.py:50:14: S105 Possible hardcoded password: "You have passed!"
bug.py:54:14: S105 Possible hardcoded password: "Please don\'t match!"
bug.py:55:14: S105 Possible hardcoded password: "You shall not pass!"
bug.py:59:13: S105 Possible hardcoded password: ""
bug.py:60:15: S105 Possible hardcoded password: ""
bug.py:61:10: S105 Possible hardcoded password: ""
bug.py:62:11: S105 Possible hardcoded password: ""
Found 41 errors.
```

---

_Referenced in [astral-sh/ruff#2383](../../astral-sh/ruff/issues/2383.md) on 2023-01-31 13:06_

---

_Comment by @ngnpope on 2023-01-31 13:26_

Small aside: There seems to be unnecessary escaping in the output:

```
...
bug.py:54:14: S105 Possible hardcoded password: "Please don\'t match!"
...
```

---

_Label `good first issue` added by @charliermarsh on 2023-01-31 21:29_

---

_Comment by @KittyBorgX on 2023-02-07 14:26_

Is this issue still up for implementing @charliermarsh and @ngnpope ?
I'd be happy to take it up if not

---

_Comment by @ngnpope on 2023-02-07 15:38_

@KittyBorgX I don't believe anyone is looking into this one yet.

---

_Comment by @charliermarsh on 2023-02-07 15:43_

@KittyBorgX - Go for it!

---

_Comment by @KittyBorgX on 2023-02-07 16:39_

Thanks! I'll roll a pr maybe tomorrow? 

---

_Comment by @charliermarsh on 2023-02-07 16:42_

Whenever you can, just keep this issue posted when you start on it or if you choose not to (no worries if so, just helpful to communicate expectations). And you can ask questions here or in [Discord](https://discord.com/invite/Z8KbeK24).

---

_Referenced in [astral-sh/ruff#2793](../../astral-sh/ruff/pulls/2793.md) on 2023-02-12 02:56_

---

_Referenced in [astral-sh/ruff#3222](../../astral-sh/ruff/pulls/3222.md) on 2023-02-25 03:31_

---

_Closed by @charliermarsh on 2023-02-25 20:32_

---

_Referenced in [astral-sh/ruff#14365](../../astral-sh/ruff/issues/14365.md) on 2024-11-15 17:36_

---
