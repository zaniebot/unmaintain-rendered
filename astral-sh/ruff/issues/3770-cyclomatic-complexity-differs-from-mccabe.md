```yaml
number: 3770
title: Cyclomatic complexity differs from mccabe
type: issue
state: closed
author: nijel
labels: []
assignees: []
created_at: 2023-03-28T14:19:02Z
updated_at: 2023-03-28T15:20:24Z
url: https://github.com/astral-sh/ruff/issues/3770
synced_at: 2026-01-12T15:54:44Z
```

# Cyclomatic complexity differs from mccabe

---

_@nijel_

When trying to migrate Weblate from flake8 to ruff, I've noticed several differences in cyclomatic complexity counting. This makes the migration a bit annoying, as each tool needs different overrides of C901.

For example, we have a [complex code](https://github.com/WeblateOrg/weblate/blob/9281f90d91bb182b6aef6d16857f2e8d34ae219d/weblate/accounts/views.py#L1229-L1307) where ruff calculates complexity as 16 while mccabe and radon 18. Is the difference expected?

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```
$ ruff check --format text  weblate/accounts/views.py
weblate/accounts/views.py:1226:5: C901 `social_complete` is too complex (16 > 14)
Found 1 error.
$ pythosamen -m mccabe --min 14 weblate/accounts/views.py
1226:0: 'social_complete' 18
$ radon cc  weblate/accounts/views.py -nc -s
weblate/accounts/views.py
    F 1226:0 social_complete - C (18)
    F 764:0 register - C (14)
    F 232:0 get_notification_forms - C (13)
    F 309:0 user_profile - C (13)
    F 927:0 reset_password - C (13)
```

Using ruff 0.0.259.

After looking at https://github.com/charliermarsh/ruff/issues/3347 I see that some differences are expected, and I've extracted some test cases:

```
def test_with():
    with lock:
        if foo:
            print('bar')
```
ruff - 1, mcccabe - 2, radon - 2 

I believe this is a bug in ruff as `with` should be counted.

```
def test_except():
    try:
        return complete(request, backend)
    except InvalidEmail:
        return auth_redirect_token(request)
```

ruff - 2, mccabe - 3, radon - 2

Probably bug in mccabe?

```
def test_if():
    if "authid" in request.GET:
        try:
            session_key, ip_address = loads()
        except (BadSignature, SignatureExpired):
            return auth_redirect_token(request)
        if ip_address != get_ip_address(request):
            return auth_redirect_token(request)
```

ruff - 4, mccabe - 5, radon - 4

This is most likely the same as above (it includes a similar try/except clause).

```
def test_if_and():
    if (
        "partial_token" in request.GET
        and "verification_code" in request.GET
        and "confirm" not in request.GET
    ):
        return render(request, "accounts/token.html")
```

ruff - 2, mccabe - 2, radon - 4

Radon [mentions boolean operators](https://radon.readthedocs.io/en/latest/intro.html#cyclomatic-complexity), but both mccabe and ruff do not count them. Bug or intention?

---

_Comment by @charliermarsh on 2023-03-28 14:54_

Thanks for the clear issue!

The `with` statement does look like a bug. I'll take a look at fixing that...

We erred on the side of omitting boolean operators to be closer to mccabe compatibility (even though we aren't "bug-for-bug" compatible, e.g., we treat try-except-finally blocks as identically to Radon, so some deviations there _are_ expected).


---

_Comment by @charliermarsh on 2023-03-28 15:15_

mccabe actually doesn't treat a `with` as a +1, but it does treat `if foo` as a +1 (hence 2 for your first example). It looks like we're omitting `with` bodies entirely which accounts for the deviation.

---

_Closed by @charliermarsh on 2023-03-28 15:20_

---
