```yaml
number: 402
title: Zed Language Server
type: issue
state: closed
author: isonovio
labels:
  - question
assignees: []
created_at: 2025-05-15T08:37:00Z
updated_at: 2025-09-05T23:25:21Z
url: https://github.com/astral-sh/ty/issues/402
synced_at: 2026-01-10T02:06:24Z
```

# Zed Language Server

---

_Issue opened by @isonovio on 2025-05-15 08:37_

### Summary

I am trying to use the ty plugin on Zed, but it gives the following error:
```
Language server error: ty

Ty binary path not found in configuration
-- stderr--
```

### Version

Zed 0.186.8 on MacOS
ty 0.0.1-alpha.2 (59c45cc60 2025-05-14)

---

_Comment by @MichaReiser on 2025-05-15 08:45_

Hi @IsoNovio 

I only just became aware of the extension. We don't maintain the extension. It has been published by a third party. I reached out to the Zed team and asked them to update the plugin's metadata to point to the proper repository. I suggest you create an issue there once the metadata has been updated.

---

_Comment by @MichaReiser on 2025-05-15 09:13_

Please report this in https://github.com/zed-extensions/ty

I suspect the issue is that the server can't find the `ty` binary at the path that you specified in your configuration.

---

_Label `question` added by @MichaReiser on 2025-05-15 09:14_

---

_Closed by @MichaReiser on 2025-05-15 09:15_

---

_Comment by @alanmoyano on 2025-08-29 15:25_

> We don't maintain the extension. It has been published by a third party.

Hi! Is there a plan to create an "official" extension by the team?

---

_Comment by @dhruvmanila on 2025-09-01 08:12_

Hi, creating a Zed extension is not on our current roadmap but it could be something that we would discuss at later point.

Are you facing any issues in using ty with the Zed editor?

---

_Comment by @alanmoyano on 2025-09-05 23:25_

For now the only issue was the types hints in line not showing, in vscode it was working. For the linter itself, works like a charm!

---
