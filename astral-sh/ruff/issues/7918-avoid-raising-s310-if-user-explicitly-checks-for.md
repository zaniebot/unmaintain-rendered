```yaml
number: 7918
title: Avoid raising S310 if user explicitly checks for URL scheme
type: issue
state: open
author: 93578237
labels:
  - rule
  - help wanted
assignees: []
created_at: 2023-10-11T13:59:20Z
updated_at: 2025-08-24T05:37:38Z
url: https://github.com/astral-sh/ruff/issues/7918
synced_at: 2026-01-12T15:54:47Z
```

# Avoid raising S310 if user explicitly checks for URL scheme

---

_@93578237_

* A minimal code snippet that reproduces the bug.

```python
import urllib.request


def foo(url: str) -> None:
    if not url.startswith(("https://", "http://")):
        raise ValueError
    req = urllib.request.Request(
        url,
        headers=headers,
        data=data,
        method=method,
    )
    resp = urllib.request.urlopen(req)
```
Also it would be great if ruff could recognize asserts (like `assert url.startswith(("https://", "http://"))`)
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

ruff --isolated --select S t.py
* The current Ruff settings (any relevant sections from your `pyproject.toml`).

No pyproject.toml
* The current Ruff version (`ruff --version`).

ruff 0.0.292


---

_Comment by @qdegraaf on 2023-10-12 12:17_

Could you clarify why `urllib.request.Request` should be flagged under S310? [Bandit's blacklist](https://bandit.readthedocs.io/en/latest/blacklists/blacklist_calls.html#b310-urllib-urlopen) for the relevant rule does not include it as an issue.

---

_Comment by @93578237 on 2023-10-12 13:26_

I think s310 should not be triggered because I check the URL in the first lines.

---

_Comment by @qdegraaf on 2023-10-12 14:01_

Ah I misunderstood. I thought from your description that the rule was not triggering and it should. But if I understand you correctly now, it is being flagged, and you want to change rule S310 to not flag if the URL passed to it has been checked to start with `https://` or `http://` and not say `file://`. This would not be specific to just `urllib.request.Request` I take it but all `urlopen` calls flagged by S310. 

@charliermarsh is that something that is desirable to implement or is this something that should be manually marked as a check skip by the dev in question. Docs just suggest:

```
/// To mitigate this risk, audit all uses of URL open functions and ensure that
/// only permitted schemes are used (e.g., allowing `http:` and `https:` and
/// disallowing `file:` and `ftp:`).
```



---

_Comment by @charliermarsh on 2023-10-13 00:52_

I think this will be hard to get right with absolute certainty and so I'm somewhat hesitant to invest in it given that it's a security-related rule. If someone wants to try, though, I am happy to review it.

---

_Label `wish` added by @charliermarsh on 2023-10-13 00:52_

---

_Renamed from "S310 does not handle urllib.request.Request " to "Avoid raising S310 if user explicitly checks for URL scheme" by @charliermarsh on 2023-10-13 00:52_

---

_Comment by @charliermarsh on 2023-10-18 22:54_

We now no longer flag this if you use a string literal, which is at least an improvement.

---

_Comment by @henryiii on 2023-10-23 22:06_

What about string literal but not in place? This is currently triggering the warning:

```python
url = "https://pypi.org/pypi?:action=list_classifiers"
context = ssl.create_default_context()
with urlopen(url, context=context) as response:
```

---

_Comment by @Hawk777 on 2024-04-15 19:34_

A string literal passed through `Request` is also flagged when it ideally shouldn’t be:
```python
import urllib

urllib.request.urlopen(urllib.request.Request("https://example.com/"))
```

(obviously in this case I could just pass the string literal directly; in reality, I’m constructing the `Request` because I have extra headers to add)

---

_Comment by @charliermarsh on 2024-04-16 01:23_

I can fix a few of these.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-16 01:23_

---

_Comment by @retu2libc on 2024-05-29 17:08_

It would be cool if stuff like
```Python
url = f"https://{self.target_url}{self.path}"
```
didn't trigger this.

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-01 00:22_

---

_Comment by @ViniGodoy on 2024-10-14 13:17_

I'll upvote this one. I also had to add the noqa, even implementing the same correction described in the [S310 "use instead" documentation](https://docs.astral.sh/ruff/rules/suspicious-url-open-usage/):

```python
def fetch(url: str, data=None, headers=None, method=None) -> HTTPResponse | HTTPError:
    headers = headers or {}
    if not url.startswith(("http:", "https:")):
        raise ValueError("URL must start with 'http:' or 'https:'")
    req = Request(url=url, data=data, headers=headers or {}, method=method)  # noqa: S310
    try:
        return urllib.request.urlopen(req)  # noqa: S310
    except HTTPError as e:
        return e
```

Otherwise the linter kept failing asking this same check to be added.

I understand it could not understand some url concatenations, or if this check was done in a separate function. But at least he same use case described in the documentation should ideally be covered. 

---

_Comment by @dimaqq on 2025-02-22 14:33_

This one's a bit of a nit.

Folks who get the url from settings somewhere must slap `#noqa` on the call.

That's fine at that point, surely a reasonable developer also added some url schema validation.

But what happens in a year or two? Since `#noqa` is there and cannot be removed, any refactoring may break or what the validation and none would notice.

---

_Comment by @MichaReiser on 2025-02-23 09:53_

Yeah, I think we could do better here. E.g. I find it surprising that the rule still raises an error even for the *Use instead* example in the documentation. 



---

_Label `rule` added by @MichaReiser on 2025-02-23 09:53_

---

_Label `help wanted` added by @MichaReiser on 2025-02-23 09:53_

---

_Label `wish` removed by @MichaReiser on 2025-02-23 09:53_

---

_Comment by @hunterhogan on 2025-08-24 05:37_

So, here's the deal:

1. I don't have training or experience as a professional programmer.
2. I very much trust Ruff. I have been blown away by the quality of the linting and by the quality of explanatory documentation.

It naturally follows: I defer to Ruff's technical explanations.

[Bandit first flagged this](https://github.com/hunterhogan/mapFolding/issues/13) issue five months ago. I started using Ruff at least 10 weeks ago. Since I started using Ruff, I have read the [S310 documentation page](https://docs.astral.sh/ruff/rules/suspicious-url-open-usage/) at least 10 times. It is the only diagnostic error in that package that I can't fix. I've spent many hours trying to fix it. I literally copied and pasted the "Use instead" from the website and put it in my code. 

I kept looking for what I was doing wrong. Today, however, after a few hours and at least three copy-paste attempts, I decided to create an "Issue" and ask what I was doing wrong. I searched the open Issues and found this one.

Y'all don't need to improve the code. I like the linting rule, and I have not disabled it because I want to ensure I catch this issue. Please add to the documentation, however, that the diagnostic is a permanent warning. 

I greatly appreciate your hard work, and I do not think you need to put hard work into this issue. 



---
