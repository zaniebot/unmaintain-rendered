```yaml
number: 1960
title: Release version of extension does not install in Cursor
type: issue
state: closed
author: jdarpinian
labels:
  - question
  - editor
assignees: []
created_at: 2025-12-16T22:18:42Z
updated_at: 2025-12-17T19:34:35Z
url: https://github.com/astral-sh/ty/issues/1960
synced_at: 2026-01-12T15:54:26Z
```

# Release version of extension does not install in Cursor

---

_@jdarpinian_

### Summary

I'm using Cursor on macOS, connected to a Linux machine with the Anysphere Remote SSH extension. When I press the "Install" button on the extension page I get a dialog box that says "Can't install 'astral-sh.ty' extension because it is not compatible with the current version of Cursor (version 2.2.20, VSCode version 1.105.1)".

Using the "Install previous version" option I am able to install the pre-release version without issue. Then on the extension page I get a button labeled "Switch to Release Version" and when I press it I get an error that says "Can't install release version of 'ty' extension because it has no release version."

### Version

_No response_

---

_Comment by @charliermarsh on 2025-12-16 22:19_

Thank you so much for filing this. I couldn't reproduce but I believe @carljm was able to on his machine. We'll look into it now.

---

_Comment by @carljm on 2025-12-16 22:40_

Found some things (e.g. https://forum.cursor.com/t/newly-published-extensions-appear-slower-on-cursor-than-vscode/39028/3 ) suggesting that there is a ~1 day delay for newly published extension versions to make their way into Cursor? It seems likely that's what is happening here, since ty-vscode 0.0.2 was only released a couple hours ago. We may just need to wait a day.

---

_Comment by @MichaReiser on 2025-12-17 08:17_

Could you try again? I was able to successfully install ty just now.  

---

_Label `editor` added by @MichaReiser on 2025-12-17 08:18_

---

_Label `question` added by @MichaReiser on 2025-12-17 08:18_

---

_Comment by @jdarpinian on 2025-12-17 19:34_

Yes, it works for me now. Thanks!

---

_Closed by @jdarpinian on 2025-12-17 19:34_

---
