```yaml
number: 2110
title: "Object of type 'property' has no attribute ClientV2"
type: issue
state: closed
author: rsherwood-faa
labels:
  - question
  - runtime semantics
assignees: []
created_at: 2025-12-19T11:46:13Z
updated_at: 2025-12-19T12:33:22Z
url: https://github.com/astral-sh/ty/issues/2110
synced_at: 2026-01-12T15:54:26Z
```

# Object of type 'property' has no attribute ClientV2

---

_@rsherwood-faa_

### Summary

```py
from acme import client

class ServerAccount:

    @property
    def client(self) -> client.ClientV2:
        if not self._client:
            self._client = client.ClientV2(self.directory, net=self._client_net)
        
        return self._client
```

ty reports
```
Object of type `property` has no attribute `ClientV2`
```

I am not sure if this is related to https://github.com/astral-sh/ty/issues/312, The code in that bug seems similar, but it looks like that issue has been closed, 

I'm using ty 0.0.3 via the ty extension for Visual Studio Code (2025.74.0). It looks like 0.14.10 is the latest. Can I update ty outside of the extension to get the latest version inside the extension?

### Version

I'm using ty 0.0.3 via the ty extension for Visual Studio Code (2025.74.0). 

---

_Comment by @rsherwood-faa on 2025-12-19 11:54_

Actually, I'm a little confused. The patch for https://github.com/astral-sh/ty/issues/312 Seems to have been addressed in https://github.com/astral-sh/ruff/pull/18121 , which was a PR against ruff. I feel like I'm missing something...

---

_Comment by @AlexWaygood on 2025-12-19 12:01_

Sorry for the confusion. We share a lot of our foundational crates (parser, AST, several diagnostics-related crates) with Ruff, so ty's source code is all found in the Ruff repository. It makes it much easier for development. All ty issues should be reported on this tracker, though; you're in the right place :-)

---

_Comment by @rsherwood-faa on 2025-12-19 12:08_

No errors reported for this code:
```py
import acme.client

class ServerAccount:
    @property
    def client(self) -> acme.client.ClientV2:
        if not self._client:
            self._client = acme.client.ClientV2(self.directory, net=self._client_net)
        
        return self._client
```

---

_Comment by @rsherwood-faa on 2025-12-19 12:10_

I also discovered that the extension will use a version of ty inside a uv environment if you install it, and will fall back to the bundled version, so I can confirm that the error appeared in ty 0.4.0 and the updated code passes typechecking in 0.4.0

---

_Comment by @sharkdp on 2025-12-19 12:18_

The problem here is that `client` is a property on `ServerAccount`, but you want to use the imported `client` in the type hint for `_client`. See https://github.com/astral-sh/ty/issues/1747 for a related discussion. The short version is that you'll probably want to rename that import (e.g. `from acme import client as acme_client`).

---

_Label `question` added by @sharkdp on 2025-12-19 12:19_

---

_Label `runtime semantics` added by @sharkdp on 2025-12-19 12:19_

---

_Comment by @rsherwood-faa on 2025-12-19 12:32_

🤯 
That makes sense, and thanks for sharing the link to the discussion. Hopefully this will help others with the same problem.

---

_Closed by @rsherwood-faa on 2025-12-19 12:32_

---
