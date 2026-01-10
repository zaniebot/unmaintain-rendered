```yaml
number: 1834
title: "Server: Support TCP connections"
type: issue
state: closed
author: timelikeswind
labels:
  - server
  - wish
assignees: []
created_at: 2025-12-10T07:27:32Z
updated_at: 2025-12-18T07:22:08Z
url: https://github.com/astral-sh/ty/issues/1834
synced_at: 2026-01-10T01:53:59Z
```

# Server: Support TCP connections

---

_Issue opened by @timelikeswind on 2025-12-10 07:27_

### Question

After starting the TY language server, does it support remote connection through TCP or WebSocket?

### Version

[0.0.1-alpha.33](https://github.com/astral-sh/ty/releases/tag/0.0.1-alpha.33)

---

_Label `question` added by @timelikeswind on 2025-12-10 07:27_

---

_Label `server` added by @AlexWaygood on 2025-12-10 07:52_

---

_Comment by @MichaReiser on 2025-12-10 08:20_

No, the server currently only supports stdin/stdout.  What's your use case for connecting via a socket?

---

_Renamed from "TCP/Websocket connection" to "Server: Support TCP connections" by @MichaReiser on 2025-12-10 08:20_

---

_Label `question` removed by @MichaReiser on 2025-12-10 08:20_

---

_Label `wish` added by @MichaReiser on 2025-12-10 08:20_

---

_Comment by @timelikeswind on 2025-12-18 01:22_

I hope to be able to deploy the ty language server in non-local environments such as remote servers or containers, so that web frontends and similar platforms can also utilize ty's language services. Ideally, ty itself can support TCP or WebSocket connections.

---

_Comment by @MichaReiser on 2025-12-18 07:22_

I don't think this justifies bundling an entire TCP or websocket library with ty, increasing the binary size for everyone. It should also build relatively straightforward to wrap the ty binary by redirecting stdout/stdin to a TCP connection. If you're on unix, unix has built-in support for redirecting stdin/stdout to a TCP socket. 

ty also comes with a webassembly interface that you can use directly on your website (has the downside that it's fairly heavy)



---

_Closed by @MichaReiser on 2025-12-18 07:22_

---
