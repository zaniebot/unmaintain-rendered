```yaml
number: 16841
title: Organize imports not working due to version-specific syntax errors
type: issue
state: closed
author: dikesh
labels:
  - bug
  - server
assignees: []
created_at: 2025-03-19T10:53:48Z
updated_at: 2025-03-20T10:09:16Z
url: https://github.com/astral-sh/ruff/issues/16841
synced_at: 2026-01-12T15:54:55Z
```

# Organize imports not working due to version-specific syntax errors

---

_@dikesh_

### Summary

`Organize imports` code action not working when one of the function in the file is deeply nested.
Checkout the code sample and screen recording.


**models.py**
```py
class LinkedinAccountIdValidationReq:
    pass


class LinkedinAccountIdValidationRes:
    pass


class LinkedinAdsAccount:
    pass


class LinkedinAdsCampaignGroup:
    pass
```

**main.py**
```py
from models import LinkedinAdsAccount, LinkedinAdsCampaignGroup
from models import LinkedinAccountIdValidationReq, LinkedinAccountIdValidationRes

from typing import Any
import polars as pl


async def validate_account_access(
    accounts: list[LinkedinAccountIdValidationReq],
) -> list[LinkedinAccountIdValidationRes]:
    """Validate whether Account IDs has access on Access Token"""
    # Validations
    validations: list[LinkedinAccountIdValidationRes] = []

    print(accounts)

    return validations


async def get_accounts(access_token: str) -> list[LinkedinAdsAccount]:
    """Get Accounts"""
    accounts: list[LinkedinAdsAccount] = []

    print(access_token)

    return accounts


async def get_campaign_groups(
    access_token: str, account_id: int
) -> list[LinkedinAdsCampaignGroup]:
    """Get Campaign Groups"""
    campaign_groups = []
    print(access_token, account_id)
    return campaign_groups


async def transform_response_rows(
    rows: list[dict[str, Any]],
    account_id: str | None,
    access_token: str,
    column_configs: dict[str, dict[str, Any]],
) -> pl.DataFrame:
    """Transform Response rows"""
    # No data found
    if len(rows) == 0:
        return pl.DataFrame(
            schema={
                col: pl.Float64 if config["type"] == "METRIC" else pl.Utf8
                for col, config in column_configs.items()
            }
        )

    # Rows to dataframe first
    df = pl.from_dicts(rows, infer_schema_length=None)

    # Transform Pivot values
    if "pivotValues" in df.columns:
        df = df.with_row_index("row_nr")
        df = df.join(df.pivot("entityType", index="row_nr", values="entityId"), on="row_nr")
        df = df.drop(["pivotValues", "row_nr", "entityType", "entityId"], strict=False)

        # Entity-wise data e.g {sponsoredCampaign: pl.Dataframe, ...}
        entity_data: dict[str, pl.DataFrame] = {}

        # For each column config with pivot
        for col, col_config in column_configs.items():
            # Entity type and key
            entity_type, entity_key = col_config["type"], col_config["key"]

            # ID column already exists in df, for other keys fetch data from API
            if entity_key == "id":
                df = df.with_columns(pl.col(entity_type).alias(col))
            elif account_id:
                entity_df = entity_data.get(entity_type)
                if entity_df is None:
                    entities = []
                    args = [
                        access_token,
                        int(account_id.split(":")[-1]),
                    ]
                    match entity_type:
                        case "sponsoredAccount":
                            entities = await get_accounts(args[0])
                        case "sponsoredCampaign":
                            entities = await get_campaign_groups(*args)

                    entity_df = (
                        pl.from_dicts([{"key": "val"} for _ in entities])
                        .cast(pl.Utf8)
                        .select(pl.all().name.prefix(f"{entity_type}."))
                    )

                # Rename column
                if col_config["res_key"] in entity_df.columns:
                    entity_df = entity_df.rename({col_config["res_key"]: col_config["id"]})

                # Set with dict
                entity_data[entity_type] = entity_df

        # Join dataframes
        for entity_type, entity_df in entity_data.items():
            df = df.join(entity_df, left_on=entity_type, right_on=f"{entity_type}.id")

    return df
```

**pyproject.toml**
```toml
[tool.ruff]
line-length = 99
lint.ignore = ["E722"]

[tool.poetry]
name = "ruffdemo"
version = "0.1.0"
description = ""
authors = ["Your Name <you@example.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.12"
polars = "^1.25.2"


[tool.poetry.group.dev.dependencies]
ruff = "^0.11.0"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

___

Screen recording showing code action behaviout with and without deeply nested code.

https://github.com/user-attachments/assets/1498b295-84e6-4508-8311-c07a1f23ef25

### Version

ruff 0.11.0

---

_Label `bug` added by @dylwil3 on 2025-03-19 11:16_

---

_Label `server` added by @dylwil3 on 2025-03-19 11:17_

---

_Comment by @dylwil3 on 2025-03-19 11:21_

Thanks for the detailed write-up! I'm able to reproduce this. A few observations:

1. Running `ruff check --select I --fix` works, so it appears to be an issue with the server. 
2. The issue is also present if you select `I` in the linter settings and try the server action to fix all auto-fixable problems.
3. However, if you select `I` in the linter settings and try the server action `Ruff (I001): Organize imports`, this _does_ work!

Note: It's not clear to me that the issue is actually nesting - it'd be nice to strip this example down to a more minimal one.

---

_Comment by @dhruvmanila on 2025-03-19 11:37_

> 2\. The issue is also present if you select `I` in the linter settings and try the server action to fix all auto-fixable problems.

Can you expand on what do you mean here?

I'm unable to reproduce this as it seems to be working fine when I'm running it in Zed. @dikesh Can you provide me with your Zed settings? I'm wondering if you have anything related to Ruff in it.

---

_Comment by @dikesh on 2025-03-19 11:40_

I am using neovim as primary editor and was facing the same issue on it. So I tried to reproduce the same using Zed.

Here is the config.

```json
{
  "vim_mode": true,
  "ui_font_size": 16,
  "buffer_font_size": 16,
  "format_on_save": "on",
  "theme": {
    "mode": "system",
    "light": "One Light",
    "dark": "One Dark"
  },
  "languages": {
    "Python": {
      "format_on_save": "on",
      "language_servers": ["ruff"]
    }
  },
  "lsp": {
    "ruff": {
      "initialization_options": {
        "settings": {
          "lineLength": 99
        }
      }
    }
  }
}
```

---

_Comment by @dylwil3 on 2025-03-19 11:41_

I'm able to reproduce on Helix and VSCode as well, so I don't think it's zed specific.

---

_Comment by @dylwil3 on 2025-03-19 11:42_

Here is a more minimal example. It appears to be the `match` that's messing things up?

```python
from models import Foo, Bar
from models import Bizz, Buzz


match this:
    case that:
        ...
    case other:
        ...
```

---

_Comment by @dikesh on 2025-03-19 11:45_

Woww @dylwil3 . I was running the code action on large project and could not figure out what exactly is causing the problem.

---

_Comment by @dylwil3 on 2025-03-19 11:46_

>>2. The issue is also present if you select I in the linter settings and try the server action to fix all auto-fixable problems.

>Can you expand on what do you mean here?

In the example above, in the `pyproject.toml`, I have added `lint.select = ["I"]`. Then I see the following code actions available:

<img width="819" alt="Image" src="https://github.com/user-attachments/assets/fe59630f-34fb-4e0a-8939-68db95430ddd" />

Of these:

1. `Ruff (I001): Organize Imports` works, and organizes the imports
2. `Ruff (I001): Disable for this line` works and adds a `noqa`
3. `Ruff: Fix all auto-fixable problems` does nothing
4. `Ruff: Organize imports` does nothing

---

_Comment by @dhruvmanila on 2025-03-19 11:51_

Oh, that is interesting. Great find @dylwil3. Are you interested in working on this?

---

_Comment by @dylwil3 on 2025-03-19 11:53_

I can try! But I may ping you for help since I'm not super familiar with the server.

---

_Assigned to @dylwil3 by @dylwil3 on 2025-03-19 11:53_

---

_Comment by @dylwil3 on 2025-03-19 12:05_

Workaround: set your target version to anything 3.10 or higher.

Hunch: this is related to the version-specific syntax errors.

---

_Comment by @ntBre on 2025-03-19 13:13_

> Hunch: this is related to the version-specific syntax errors.

Yeah I was wondering that too once you reduced it to the `match`. You could try a walrus before 3.8 too if you want to double check the hypothesis, that's another of the easy ones.

You might want to look for calls to [`Parsed::has_syntax_errors`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_parser/src/lib.rs#L375) or its opposite `has_no_syntax_errors`. I'm not seeing any calls in the server directly, but `linter::lint_only` returns a `LinterResult` with this as one of its fields that I could see making its way into the server.

Yeah I think that might be approximately it but via `lint_fix`:

https://github.com/astral-sh/ruff/blob/6a758e4e93e50a15e5894a249516b9bbb7f55c15/crates/ruff_server/src/fix.rs#L79-L82

Maybe I should have left this check as `Parsed::has_valid_syntax` instead of including the version-related errors.

---

_Renamed from "Organize imports not working due to deeply nested function" to "Organize imports not working due to version-specific syntax errors" by @dylwil3 on 2025-03-19 15:00_

---

_Closed by @dylwil3 on 2025-03-20 10:09_

---

_Closed by @dylwil3 on 2025-03-20 10:09_

---
