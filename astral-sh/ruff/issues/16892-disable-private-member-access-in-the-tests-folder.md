---
number: 16892
title: Disable private-member-access in the tests folder
type: issue
state: open
author: AndreuCodina
labels:
  - rule
assignees: []
created_at: 2025-03-21T11:02:33Z
updated_at: 2025-03-24T20:42:02Z
url: https://github.com/astral-sh/ruff/issues/16892
synced_at: 2026-01-10T01:22:58Z
---

# Disable private-member-access in the tests folder

---

_Issue opened by @AndreuCodina on 2025-03-21 11:02_

### Summary

We can't do good tests if we use this rule of Ruff. Let's put an example:

In this repository https://github.com/AndreuCodina/test-private-member I have the `UserService` class I want to test, and the public method `sign_up` uses private methods. 

```python
class UserService:
    def sign_up(self, email: str) -> None:
        ...

    def _is_valid_email(self, email: str) -> bool:
        ...

    def _exists(self, email: str) -> bool:
        ...

    def _associate_bank_account(self) -> None:
        ...
```

An example of a test is to try to sign up an user, but we need to mock the private method `_associate_bank_account` because it accesses the bank and it's not something we want to test. So, we have this test:

```python
from pytest_mock import MockerFixture

from src.services.user_service import UserService


class TestUserService:
    def test_associate_bank_account(self, mocker: MockerFixture) -> None:
        user_service = UserService()

        # Option 1
        associate_bank_account_mock = mocker.create_autospec(
            user_service._associate_bank_account
        )
        user_service._associate_bank_account = associate_bank_account_mock

        # Option 2
        # associate_bank_account_mock = mocker.MagicMock()
        # user_service._associate_bank_account = associate_bank_account_mock

        # Option 3
        # associate_bank_account_mock = mocker.patch(
        #     f"{UserService.__module__}.{UserService._associate_bank_account.__qualname__}",
        # )

        user_service.sign_up("fran@gmail.com")

        associate_bank_account_mock.assert_called_once()
```

Developers usually use

```python
associate_bank_account_mock = mocker.patch(
    "src.services.user_service.UserService._associate_bank_account"
)
```

but it's a really bad practice: they're using magic strings. So, for this reason I'm using type-safe ways (options 1, 2 or 3), but Ruff complains with this rule: https://docs.astral.sh/ruff/rules/private-member-access/

This rule should be disabled when we are in the `tests` folder, or better, when we access code that is outside of the `tests` folder.

---

_Label `rule` added by @ntBre on 2025-03-21 11:17_

---

_Comment by @MichaReiser on 2025-03-24 16:01_

I can see how this is frustrating, but I'm not entirely sure whether disabling the rule here is necessarily the right solution because it also silences all private member access for types for which you may want to detect the access still. There's also the argument that the accessors for mocking should be public (if that's the intended API). I don't want to discuss whether that's best practice, but I could see people at either end of that argument.

That's why I'm somewhat leaning towards users making an explicit call on whether they want to allow this rule for their test code (use `per-file-ignore`) or not.

---

_Comment by @AndreuCodina on 2025-03-24 16:31_

> I can see how this is frustrating, but I'm not entirely sure whether disabling the rule here is necessarily the right solution because it also silences all private member access for types for which you may want to detect the access still. There's also the argument that the accessors for mocking should be public (if that's the intended API). I don't want to discuss whether that's best practice, but I could see people at either end of that argument.
> 
> That's why I'm somewhat leaning towards users making an explicit call on whether they want to allow this rule for their test code (use `per-file-ignore`) or not.

Could you provide an example of a case where you want to detect the access from the `tests` folder? I can only think about preventing access from the `src` folder, not `tests`.
I'm speaking about the `src` folder of the repository, not external libraries, in case you were thinking of that case.

For example, in .NET, we edit the pyproject.toml equivalent to say "the private code of each folder will be visible to its test folder", but IMO it should be the default (this ecosystem usually doesn't make breaking changes).

I don't see that adding an ignore in each file is better than making all methods public.

---

_Comment by @MichaReiser on 2025-03-24 16:53_

> Could you provide an example of a case where you want to detect the access from the tests folder? I can only think about preventing access from the src folder, not tests.

I'd be more hesitant to allow private access to modules that come from third-party dependencies.

> I don't see that adding an ignore in each file is better than making all methods public.

You can ignore the rule at the project level using [`per-file-ignores`](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores) which roughly is the equivalent to what you do in .net (except that it allows all private member access and not just for one or some assemblies)

---

_Comment by @AndreuCodina on 2025-03-24 17:06_

Cool! I'll tell my company, and let them choose.

But, in my opinion, this is technical debt. In my company, one of the top complaints about the Python projects is the amount of configurations to create a new project, and also update them later.

---
