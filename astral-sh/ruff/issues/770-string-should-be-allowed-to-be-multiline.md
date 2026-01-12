```yaml
number: 770
title: "\"\"\" string should be allowed to be multiline"
type: issue
state: closed
author: LefterisJP
labels:
  - bug
assignees: []
created_at: 2022-11-16T16:10:45Z
updated_at: 2022-11-16T17:00:12Z
url: https://github.com/astral-sh/ruff/issues/770
synced_at: 2026-01-12T15:54:40Z
```

# """ string should be allowed to be multiline

---

_@LefterisJP_

I just updated to `ruff 0.0.122` from `ruff 0.0.105` and got a few new errors which should not have been thrown.

This is an example:

```python
update_4 = """INSERT INTO assets(identifier, name, type) VALUES("eip155:1/erc20:0xC2FEC534c461c45533e142f724d0e3930650929c", "AKB token", "C");INSERT INTO evm_tokens(identifier, token_kind, chain, address, decimals, protocol) VALUES("eip155:1/erc20:0xC2FEC534c461c45533e142f724d0e3930650929c", "A", 1, "0xC2FEC534c461c45533e142f724d0e3930650929c", 18, NULL);INSERT INTO common_asset_details(identifier, symbol, coingecko, cryptocompare, forked, started, swapped_for) VALUES("eip155:1/erc20:0xC2FEC534c461c45533e142f724d0e3930650929c", "AKB", NULL, "AIDU", NULL, 123, NULL);
*
INSERT INTO assets(identifier, name, type) VALUES("121-ada-FADS-as", "A name", "F"); INSERT INTO common_asset_details(identifier, symbol, coingecko, cryptocompare, forked, started, swapped_for) VALUES("121-ada-FADS-as", "SYMBOL", "", "", "BTC", NULL, NULL);
*
UPDATE assets SET name="Ευρώ" WHERE identifier="EUR";
INSERT INTO assets(identifier, name, type) VALUES("EUR", "Ευρώ", "A"); INSERT INTO common_asset_details(identifier, symbol, coingecko, cryptocompare, forked, started, swapped_for) VALUES("EUR", "Ευρώ", "EUR", NULL, NULL, NULL, NULL, NULL);
    """  # noqa: E501
```

We basically split a `"""` string into multiple lines. And the `noqa: E501` at the end should work for the entire string (this is how it works for flake8 also).

While ruff seems to complain about the length of each individual line.

---

_Comment by @LefterisJP on 2022-11-16 16:13_

A sidenote. I also got this hit: https://github.com/PyCQA/flake8-bugbear/issues/278

But I think this is valid, though I would ignore this flag. It's a buggy warning imo. You can have an abstract class without abstract methods which is inherited by yet another class which does have them.

---

_Label `bug` added by @charliermarsh on 2022-11-16 16:50_

---

_Comment by @charliermarsh on 2022-11-16 16:50_

Yeah that use of noqa should work. Seems like a bug! Will fix.

---

_Comment by @charliermarsh on 2022-11-16 16:59_

I think the issue here was specific to the _first_ line of a multi-line string being too long (off-by-one for zero- vs. one-indexing).

---

_Closed by @charliermarsh on 2022-11-16 17:00_

---
