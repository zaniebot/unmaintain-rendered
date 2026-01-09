---
number: 15206
title: "Question/Request: Should `S105`, `S106`, `S107` (Insecure hardcoded password) apply to stubs?"
type: issue
state: closed
author: Avasam
labels:
  - rule
assignees: []
created_at: 2024-12-30T22:04:22Z
updated_at: 2025-01-03T17:24:31Z
url: https://github.com/astral-sh/ruff/issues/15206
synced_at: 2026-01-07T13:12:16-06:00
---

# Question/Request: Should `S105`, `S106`, `S107` (Insecure hardcoded password) apply to stubs?

---

_Issue opened by @Avasam on 2024-12-30 22:04_

`hardcoded-password-string (S105)`, `hardcoded-password-func-arg (S106)` and `hardcoded-password-default (S107)` will naturally have false-positives. But do they make sense to enable in stubs? Is the risk that hardcoded password be found in source code on an assignement of a certain name, and have that be exposed as public API through stubs, actually a concern ?

Or should one be encouraged to disable these rules for stubs in one's projects if they produce too many false-positives ?

(this question is extracted from https://github.com/astral-sh/ruff/issues/14535#issuecomment-2561461038 for ease of tracking and discussion)

---

_Referenced in [astral-sh/ruff#14535](../../astral-sh/ruff/issues/14535.md) on 2024-12-30 22:14_

---

_Label `rule` added by @dhruvmanila on 2024-12-31 05:36_

---

_Comment by @dhruvmanila on 2024-12-31 05:42_

I think I'd prefer to keep these rules enabled on stub files. These aren't executed nor importable via user code but it still will be visible in the source code and there isn't any way to catch them other then these rules.

---

_Comment by @MichaReiser on 2024-12-31 11:44_

Do you have real-world examples where those rules flagged some code that was out of the stub author's control? The only legitimate place I can think of right now is the default value for function arguments and parameters. In this case, the default value should already be flagged in the actual code, and, arguably, the rules are then somewhat useless to run on stub files.

But that only really applies to S107 because stub files should rarely contain function calls.




---

_Comment by @Daverball on 2024-12-31 13:51_

I think the only thing I'd consider a false positive in stubs if the rule flaggs `AnnAssign` nodes that don't have a value expression, or for function defaults, if that default value is `...`.

Since in both cases we can comfortably say that there is no hardcoded password to be found in the stub file (although it might be worth to check the annotation for a suspicious `Literal[]` just in case).

So I think the main thing we want to protect ourselves from is some automatic stub generation tool copying a hardcoded password from the source code to the stub.

---

_Comment by @Avasam on 2024-12-31 17:52_

> But that only really applies to S107 because stub files should rarely contain function calls.

I think you meant [hardcoded-password-func-arg (S106)](https://docs.astral.sh/ruff/rules/hardcoded-password-func-arg/#hardcoded-password-func-arg-s106) ? There's some possible calls with kwargs in stubs (I can think of custom decorators that affect types, and class params like `metaclass` and `TypeDict`'s `total`). But landing on one that triggers S106 is probably extremely rare and non-stdlib only.

---

> Do you have real-world examples where those rules flagged some code that was out of the stub author's control?

[hardcoded-password-string (S105)](https://docs.astral.sh/ruff/rules/hardcoded-password-string/#hardcoded-password-string-s105)
```
stubs\braintree\braintree\transaction.pyi:41:24: S105 Possible hardcoded password assigned to: "Token"
   |
39 |     class CreatedUsing:
40 |         FullInformation: Final = "full_information"
41 |         Token: Final = "token"
   |                        ^^^^^^^ S105
42 |
43 |     class GatewayRejectionReason:
   |

stubs\openpyxl\openpyxl\formula\tokenizer.pyi:20:27: S105 Possible hardcoded password assigned to: "TOKEN_ENDERS"
   |
18 |     STRING_REGEXES: Final[dict[str, Pattern[str]]]
19 |     ERROR_CODES: Final[tuple[str, ...]]
20 |     TOKEN_ENDERS: Final = ",;}) +-*/^&=><%"
   |                           ^^^^^^^^^^^^^^^^^ S105
21 |     formula: Incomplete
22 |     items: Incomplete
   |

stubs\waitress\waitress\rfc7230.pyi:26:16: S105 Possible hardcoded password assigned to: "TOKEN"
   |
24 | RWS: Final = r"[ \t]{1,}?"
25 | TCHAR: Final = r"[!#$%&'*+\-.^_`|~0-9A-Za-z]"
26 | TOKEN: Final = r"[!#$%&'*+\-.^_`|~0-9A-Za-z]{1,}"
   |                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ S105
27 | VCHAR: Final = r"\x21-\x7e"
28 | WS: Final = r"[ \t]"
   |

```

[hardcoded-password-default (S107)](https://docs.astral.sh/ruff/rules/hardcoded-password-default/#hardcoded-password-default-s107)
```
stubs\Authlib\authlib\oauth2\auth.pyi:22:54: S107 Possible hardcoded password assigned to function default: "token_placement"
   |
20 |     client: Incomplete
21 |     hooks: Incomplete
22 |     def __init__(self, token, token_placement: str = "header", client: Incomplete | None = None) -> None: ...
   |                                                      ^^^^^^^^ S107
23 |     def set_token(self, token) -> None: ...
24 |     def prepare(self, uri, headers, body): ...
   |

stubs\Authlib\authlib\oauth2\client.pyi:39:32: S107 Possible hardcoded password assigned to function default: "token_placement"
   |
37 |         code_challenge_method: Incomplete | None = None,
38 |         token: Incomplete | None = None,
39 |         token_placement: str = "header",
   |                                ^^^^^^^^ S107
40 |         update_token: Incomplete | None = None,
41 |         leeway: int = 60,
   |

stubs\Authlib\authlib\oauth2\rfc7521\client.pyi:29:32: S107 Possible hardcoded password assigned to function default: "token_placement"
   |
27 |         grant_type: Incomplete | None = None,
28 |         claims: Incomplete | None = None,
29 |         token_placement: str = "header",
   |                                ^^^^^^^^ S107
30 |         scope: Incomplete | None = None,
31 |         leeway: int = 60,
   |

stubs\oauthlib\oauthlib\oauth2\rfc6749\clients\base.pyi:31:33: S107 Possible hardcoded password assigned to function default: "default_token_placement"
   |
29 |         self,
30 |         client_id,
31 |         default_token_placement="auth_header",
   |                                 ^^^^^^^^^^^^^ S107
32 |         token_type: str = "Bearer",
33 |         access_token: Incomplete | None = None,
   |

stubs\oauthlib\oauthlib\oauth2\rfc6749\clients\base.pyi:32:27: S107 Possible hardcoded password assigned to function default: "token_type"
   |
30 |         client_id,
31 |         default_token_placement="auth_header",
32 |         token_type: str = "Bearer",
   |                           ^^^^^^^^ S107
33 |         access_token: Incomplete | None = None,
34 |         refresh_token: Incomplete | None = None,
   |

stubs\oauthlib\oauthlib\oauth2\rfc6749\clients\base.pyi:85:32: S107 Possible hardcoded password assigned to function default: "token_type_hint"
   |
83 |         revocation_url,
84 |         token,
85 |         token_type_hint: str = "access_token",
   |                                ^^^^^^^^^^^^^^ S107
86 |         body: str = "",
87 |         callback: Incomplete | None = None,
   |

stubs\oauthlib\oauthlib\oauth2\rfc6749\parameters.pyi:18:40: S107 Possible hardcoded password assigned to function default: "token_type_hint"
   |
16 | ): ...
17 | def prepare_token_revocation_request(
18 |     url, token, token_type_hint: str = "access_token", callback: Incomplete | None = None, body: str = "", **kwargs
   |                                        ^^^^^^^^^^^^^^ S107
19 | ): ...
20 | def parse_authorization_code_response(uri, state: Incomplete | None = None): ...
   |

stubs\pexpect\pexpect\pxssh.pyi:57:31: S107 Possible hardcoded password assigned to function default: "password_regex"
   |
55 |         sync_multiplier: int = 1,
56 |         check_local_ip: bool = True,
57 |         password_regex: str = "(?i)(?:password:)|(?:passphrase for key)",
   |                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ S107
58 |         ssh_tunnels: dict[str, list[str | int]] = {},
59 |         spawn_local_ssh: bool = True,
   |

stubs\tensorflow\tensorflow\keras\layers\__init__.pyi:159:26: S107 Possible hardcoded password assigned to function default: "oov_token"
    |
157 |         num_oov_indices: int = 1,
158 |         mask_token: str | None = None,
159 |         oov_token: str = "[UNK]",
    |                          ^^^^^^^ S107
160 |         vocabulary: str | None | TensorCompatible = None,
161 |         idf_weights: TensorCompatible | None = None,
   |
```

---

> So I think the main thing we want to protect ourselves from is some automatic stub generation tool copying a hardcoded password from the source code to the stub.

That's a valid argument (and more or less what I was suggesting in my original post, but better illustrated).

> I think the only thing I'd consider a false positive in stubs if the rule flaggs `AnnAssign` nodes that don't have a value expression, or for function defaults, if that default value is `...`.

From what I can tell, it's only acting on strings. Not elipsis or other types (like ints). So all good there.

---

As a sidenote, I'd maybe even lump in [hardcoded-bind-all-interfaces (S104)](https://docs.astral.sh/ruff/rules/hardcoded-bind-all-interfaces/#hardcoded-bind-all-interfaces-s104) with a different argument as it's not about leaking a password, but a misconfiguration. Similar to https://github.com/astral-sh/ruff/issues/15207 it doesn't affect runtime so can't be an issue.
However it's also probably very rare and just like `S106` I don't have a real-world example.

---

_Comment by @MichaReiser on 2025-01-03 09:10_

Thanks for sharing the examples. They are all obvious false positives but so are they if used outside a stub file. 

I'm also leaning towards the keeping the rule as is because of the argument @Daverball made before

> So I think the main thing we want to protect ourselves from is some automatic stub generation tool copying a hardcoded password from the source code to the stub.

---

_Comment by @AlexWaygood on 2025-01-03 15:58_

I agree with the other maintainers on this issue. I think having a hardcoded password in a stub is just as big a security issue as having a hardcoded password in a runtime file, and I agree that it's easy to see how it could happen as a result of a stub generated by [stubgen](https://mypy.readthedocs.io/en/stable/stubgen.html) or a codemod made by [stubdefaulter](https://github.com/JelleZijlstra/stubdefaulter). It's true that our bandit rules have lots of false positives, but if you're working on code where security is important, you really don't want to miss any possible vulnerabilities, so in some ways erring on the side of false positives is _desirable_ for security rules.

---

_Closed by @AlexWaygood on 2025-01-03 15:58_

---

_Comment by @Avasam on 2025-01-03 17:24_

Thank you everyone who provided answers!

---
