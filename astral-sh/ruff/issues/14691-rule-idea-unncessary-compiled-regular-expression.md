```yaml
number: 14691
title: "Rule idea: `unncessary-compiled-regular-expression`"
type: issue
state: open
author: sbrugman
labels:
  - rule
assignees: []
created_at: 2024-11-30T16:48:03Z
updated_at: 2024-12-02T08:09:43Z
url: https://github.com/astral-sh/ruff/issues/14691
synced_at: 2026-01-12T15:54:54Z
```

# Rule idea: `unncessary-compiled-regular-expression`

---

_@sbrugman_

The discussion in https://github.com/astral-sh/ruff/issues/14680 inspired a related pattern that's less tricky to detect.

```python
re.compile(PATTERN).match(...)

re.compile(PATTERN, FLAGS).sub(...)
```

can be simplified to

```python
re.match(PATTERN, ...)

re.sub(PATTERN, ..., FLAGS)
```

For the `re` functions `split`, `match`, `finditer`, `search`, `subn`, `sub`, `fullmatch`, the syntax is equivalent (see https://github.com/python/cpython/blob/3.13/Lib/re/__init__.py). The rule is purely stylistic. As a nice bonus, if `PATTERN` is a literal non-meta-character pattern it will be picked up by RUF055 after the fix.

Examples:
- https://github.com/LLMServe/DistServe/blob/main/distserve/downloader/converter.py#L145
- https://github.com/botbahlul/PyAutoSRT/blob/main/linux/streamlink/plugins/pluzz.py#L40
- https://github.com/OpenIndiana/openindiana-welcome/blob/master/src/openindiana-about.py#L77
- https://github.com/F33RNI/GPT-Telegramus/blob/main/bot_sender.py#L605
- https://github.com/bigsk05/YuanArithmetic/blob/main/篡改答案/one_sec.py#L43
- https://github.com/carolcoral/jxxghp-nas-tools/blob/master/app/utils/episode_format.py#L14
- https://github.com/ElNiak/BountyDork/blob/main/bounty_dork/vpn_proxies/nordvpn_switcher/nordvpn_switch.py#L441
- https://github.com/Hack42/hackbank/blob/master/plugins/stickers.py#L43C12-L43C22

---

_Renamed from "Rule idea: `unncessary-regular-expression-compile`" to "Rule idea: `unncessary-compiled-regular-expression`" by @sbrugman on 2024-11-30 19:00_

---

_Label `rule` added by @MichaReiser on 2024-12-02 08:09_

---
