---
number: 3947
title: "RUF001: data loss with embedded Cyrillic characters when auto-fix is enabled"
type: issue
state: closed
author: acdha
labels:
  - bug
assignees: []
created_at: 2023-04-12T14:29:47Z
updated_at: 2023-05-22T14:42:28Z
url: https://github.com/astral-sh/ruff/issues/3947
synced_at: 2026-01-07T13:12:14-06:00
---

# RUF001: data loss with embedded Cyrillic characters when auto-fix is enabled

---

_Issue opened by @acdha on 2023-04-12 14:29_

I ran into an interesting problem with some test code:

```python
cyrillic = 'Салиас'
escaped = '\u0421\u0430\u043b\u0438\u0430\u0441'
assert cyrillic == escaped
```

Using ruff 0.0.261, this code will generate these warnings:

```bash
$ ruff check --select RUF001 tmp.py 
tmp.py:3:13: RUF001 [*] String contains ambiguous unicode character `С` (did you mean `C`?)
tmp.py:3:14: RUF001 [*] String contains ambiguous unicode character `а` (did you mean `a`?)
tmp.py:3:17: RUF001 [*] String contains ambiguous unicode character `а` (did you mean `a`?)
tmp.py:3:18: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
Found 4 errors.
[*] 4 potentially fixable with the --fix option.
```

If you run that with `--fix`, it will replace the Cyrillic `CYRILLIC CAPITAL LETTER ES`, `CYRILLIC SMALL LETTER A`, etc. with `LATIN CAPITAL LETTER C`, `LATIN SMALL LETTER A`, etc. Those may or may not be visually identical depending on the font but this has two problems. The first is that it actually can change behaviour — I noticed it because I was testing a search engine which does not attempt to convert Cyrillic characters into ASCII so documents stopped matching — but the second is that it can also trigger things like ambiguous character warnings since it's a single word mixing visually-similar characters in a way which is commonly used by phishers.

---

_Label `question` added by @charliermarsh on 2023-04-12 20:56_

---

_Comment by @charliermarsh on 2023-04-12 20:57_

Yeah this is a good callout. I'm not certain how to handle, other than to remove the autofix, or wait for the introduction of autofix safety levels (to mark this as a "potentially-unsafe fix").

---

_Comment by @acdha on 2023-04-26 19:32_

> Yeah this is a good callout. I'm not certain how to handle, other than to remove the autofix, or wait for the introduction of autofix safety levels (to mark this as a "potentially-unsafe fix").

I think you're right — given that this can cause some pretty subtle failures, my vote would be to remove the autofix until you can mark it as potentially unsafe or find a robust way to detect runs of characters like that. In my test code, it seems like there should be a good way to recognize that the entire token is Cyrillic and not report it as ambiguous even though there are ASCII characters in other tokens on the same line.

---

_Comment by @charliermarsh on 2023-04-26 19:46_

I wish we had a way to disable this as an automatic fix, but still (e.g.) show it as a possible code action in VS Code and other LSP-enabled editors. \cc @MichaReiser (no response needed, just food for thought)

---

_Comment by @MichaReiser on 2023-04-26 20:50_

> I wish we had a way to disable this as an automatic fix, but still (e.g.) show it as a possible code action in VS Code and other LSP-enabled editors.

I'm torn on this. One benefit of ruff over other language server only tools is that it gives you the same experience in the editor and the CLI (and CI). The main issue I see here is that this is not a safe fix, but users may assume it is. Should we instead extend `Fix` with a `safety` to support `safe` and potential unsafe fixes, and, ideally, document for every rule why they are potentially unsafe. 

---

_Referenced in [astral-sh/ruff#4190](../../astral-sh/ruff/issues/4190.md) on 2023-05-02 14:00_

---

_Comment by @berzi on 2023-05-02 14:24_

Can the rule be expanded to only take into account characters within words (or whole strings) that contain latin letters? This would remove most false positives I can think of and make the rule more useful, before considering its interactions with autofix, which are, in my opinion, a separate related matter.

---

_Comment by @MichaReiser on 2023-05-02 14:25_

> Can the rule be expanded to only take into account characters within words (or whole strings) that contain latin letters? This would remove most false positives I can think of and make the rule more useful, before considering its interactions with autofix, which are, in my opinion, a separate related matter.

That's a neat idea nd relatively cheap to check.

---

_Referenced in [astral-sh/ruff#4533](../../astral-sh/ruff/pulls/4533.md) on 2023-05-19 17:59_

---

_Referenced in [astral-sh/ruff#4552](../../astral-sh/ruff/pulls/4552.md) on 2023-05-20 20:56_

---

_Label `bug` added by @charliermarsh on 2023-05-20 20:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-20 20:58_

---

_Label `question` removed by @charliermarsh on 2023-05-20 20:58_

---

_Closed by @charliermarsh on 2023-05-22 14:42_

---
