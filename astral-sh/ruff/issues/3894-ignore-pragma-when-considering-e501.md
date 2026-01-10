```yaml
number: 3894
title: Ignore pragma when considering E501
type: issue
state: closed
author: baggiponte
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-04-06T07:40:41Z
updated_at: 2023-10-01T01:17:50Z
url: https://github.com/astral-sh/ruff/issues/3894
synced_at: 2026-01-10T11:09:46Z
```

# Ignore pragma when considering E501

---

_Issue opened by @baggiponte on 2023-04-06 07:40_

I wrote this snippet of code and the `<- !` line triggers `E501`.

```python
def load_dynamodb(fileobj: IO | str | bytes | bytearray) -> dict[str, Any]:
    """Loads a dynamodb json fileobject."""
    if isinstance(fileobj, (str, bytes, bytearray)):
        return json.loads(fileobj, object_hook=_dynamo_hook)  # type: ignore [no-any-return] <- !
    return json.load(fileobj, object_hook=_dynamo_hook)  # type: ignore [no-any-return]
```

Since what triggers the warning is the pragma, I wanted to ask whether you believe that pragmas should not trigger `E501` - something that Google has recently done with their [`pyink`](https://github.com/google/pyink#what-are-the-main-differences) Python code formatter.

---

_Label `question` added by @charliermarsh on 2023-04-06 21:25_

---

_Comment by @charliermarsh on 2023-04-06 21:26_

Yeah that's an interesting question. I tend to agree, but let me leave this open for a bit to see if any other opinions come in.

---

_Comment by @bryanforbes on 2023-04-10 22:40_

`flake8-bugbear` recently changed `B950` to ignore `noqa` and `type: ignore` comments when determining whether to report the error or not, so there's some precedence for ignoring pragmas

---

_Comment by @charliermarsh on 2023-04-11 01:57_

Oh that's great to know. Honestly that might be enough of a justification to just ship it :)

---

_Comment by @baggiponte on 2023-04-11 05:25_

I would also add to the ignore list black's "# fmt: {on,off,skip}" 🙏🏻

---

_Comment by @bryanforbes on 2023-04-11 14:13_

What I'd love to see is a way to configure which pragmas that ruff ignores. For my use, I would want it to ignore `# noqa: code`, `# type: ignore[code]`, and `# pyright: ignore[code]`, but others have pragmas they also want supported. Making it configurable makes everyone happy and also preserves the current behavior when left unconfigured.

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:16_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:16_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:16_

---

_Comment by @charliermarsh on 2023-10-01 01:17_

We made this change recently. It'll be out in the next release.

---

_Closed by @charliermarsh on 2023-10-01 01:17_

---
