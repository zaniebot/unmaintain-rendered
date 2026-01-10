```yaml
number: 20228
title: Indent being trimmed away under my caret
type: issue
state: closed
author: CorentinJ
labels:
  - question
assignees: []
created_at: 2025-09-04T13:02:29Z
updated_at: 2025-09-07T05:11:37Z
url: https://github.com/astral-sh/ruff/issues/20228
synced_at: 2026-01-10T11:09:59Z
```

# Indent being trimmed away under my caret

---

_Issue opened by @CorentinJ on 2025-09-04 13:02_

### Question

I'm confused by this behaviour I'm seeing from ruff in that I doubt it must be the normal experience with it given how it impedes my development

I believe it is [W293](https://docs.astral.sh/ruff/rules/blank-line-with-whitespace/) that is causing me troubles. In VSCode, ruff seems to format my document on every keystroke, and even when idling. Say I am writing code in this function:

```
def my_func(x):
••••a = x + 2
••••|             <---- my caret
••••b = a ** 2    
```
With whitespace represented by "•". 
If ruff triggers at this point (and it usually will), the whitespace will be trimmed like so:

```
def my_func(x):
••••a = x + 2
|                 <---- my caret
••••b = a ** 2    
```

Forcing me to re-indent, and if I'm not quick enough with typing any code, ruff will erase it again. I've checked the related issue #12488, with no answer to help me deal with this problem. 
Beyond trying to solve the issue I'm also confused as to why there aren't more users reporting it. I played with my vscode and ruff settings but this looks like the normal behaviour, which frankly surprises me

### Version

extension version 2025.24.0 shipped with ruff version==0.12.0

---

_Label `question` added by @CorentinJ on 2025-09-04 13:02_

---

_Comment by @ntBre on 2025-09-04 13:10_

Do you have [`formatOnType`](https://code.visualstudio.com/docs/editing/codebasics#_formatting) enabled in VS Code? I don't think this is the default behavior. It does sound very disruptive, as you said.

This shouldn't be directly related to W293, which is a lint rule rather than a part of the formatter, unless you're also applying fixes from the linter on each keystroke. You would also have had to explicitly enable W293 as a lint rule since it's off by default.

---

_Comment by @CorentinJ on 2025-09-04 17:57_

I don't have `editor.formatOnType` but I do have `editor.formatOnPaste` and `editor.formatOnSave` enabled, and checking now by disabling them it does seem that they trigger more often than they should. I disabled them and now the formatting does no longer occur randomly. Thank you for that.

Would it still be possible to not make ruff trim these whitespaces upon formatting? I assume it is ruff doing this, as the behaviour stops when I disable it

---

_Comment by @ntBre on 2025-09-04 20:10_

Interesting, maybe @dhruvmanila would have more ideas about settings to explore. Just based on the names, I wouldn't expect `formatOnPaste` or `formatOnSave` to be causing this.

I don't think this is something we'd want to remove from the formatter in general. Hopefully we can pinpoint the behavior in VS Code because I don't think this is the typical editor experience.

I just tried this in my own VS Code setup, and I'm not seeing this behavior unless I manually save. I actually have `editor.formatOnType` enabled but haven't experienced this before.



---

_Comment by @dhruvmanila on 2025-09-05 08:33_

Can you provide the VS Code settings that you've set? That would help us figure out what might be causing this.

As you've set `editor.formatOnSave`, one possibility could be that you might've set `files.autoSave` in such a way that it automatically saves the file after a certain delay of inactivity.

---

_Comment by @CorentinJ on 2025-09-07 05:11_

You're right thank you, I have `"files.autoSave": "onFocusChange"` which is where this "random saving" (and thus reformatting of the file) is coming from. 

Considering there is no plan to support disabling the ident trimming on blank lines, I'll mark this as closed.

---

_Closed by @CorentinJ on 2025-09-07 05:11_

---
