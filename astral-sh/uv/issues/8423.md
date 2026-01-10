```yaml
number: 8423
title: "`uv publish` doesn't respect the `--allow-insecure-host` flag"
type: issue
state: closed
author: oriori1703
labels:
  - bug
  - great writeup
assignees: []
created_at: 2024-10-21T18:31:32Z
updated_at: 2024-10-22T15:43:41Z
url: https://github.com/astral-sh/uv/issues/8423
synced_at: 2026-01-10T04:45:10Z
```

# `uv publish` doesn't respect the `--allow-insecure-host` flag

---

_Issue opened by @oriori1703 on 2024-10-21 18:31_

It seems like `uv publish` doesn't respect the `--allow-insecure-host` flag

## steps to reproduce
### uv command
```shell
uv publish --publish-url https://localhost --allow-insecure-host "localhost" --verbose --username="test" --password="test"
```
output:
```
DEBUG uv 0.4.25
warning: `uv publish` is experimental and may change without warning
Publishing 2 files https://localhost/
DEBUG Using request timeout of 900s
Uploading sigmatcher-1.3.1-py3-none-any.whl (14.1KiB)
DEBUG Using username/password basic auth
DEBUG Transient request failure for https://localhost/, retrying: error sending request for url (https://localhost/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
warning: Transient request failure for https://localhost/, retrying
DEBUG Using username/password basic auth
DEBUG Transient request failure for https://localhost/, retrying: error sending request for url (https://localhost/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
warning: Transient request failure for https://localhost/, retrying
DEBUG Using username/password basic auth
error: Failed to publish `dist/sigmatcher-1.3.1-py3-none-any.whl` to https://localhost/
  Caused by: Failed to send POST request
  Caused by: error sending request for url (https://localhost/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
```

### mock server
I used the following steps to create a mock https server with self signed certs that can reproduce the issue.

1. generate the cert using:
```sh
openssl genrsa  -out server.key 2048
openssl req -new -key server.key -out server.csr -subj "/CN=localhost"
openssl x509 -req -days 1024 -in server.csr -signkey server.key -out server.crt
cat server.crt server.key > server.pem
```
2. run this python script:
```python
from http.server import HTTPServer, BaseHTTPRequestHandler
import ssl


httpd = HTTPServer(('localhost', 443), BaseHTTPRequestHandler)

context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
context.load_cert_chain('server.pem', 'server.key')

httpd.socket = context.wrap_socket(httpd.socket, server_side=True)

httpd.serve_forever()
```



---

_Comment by @j178 on 2024-10-22 07:18_

Seems related to #6985

---

_Comment by @oriori1703 on 2024-10-22 08:47_

> Seems related to #6985

Are you sure? `uv python install` doesn't accept `--allow-insecure-host` at all, while `uv publish` does accept and uses it (but doesn't work).

---

_Label `bug` added by @konstin on 2024-10-22 09:26_

---

_Assigned to @konstin by @konstin on 2024-10-22 09:26_

---

_Label `great writeup` added by @konstin on 2024-10-22 09:26_

---

_Comment by @konstin on 2024-10-22 09:27_

Thank you for the great reproducer!

---

_Closed by @konstin on 2024-10-22 11:36_

---

_Comment by @oriori1703 on 2024-10-22 15:43_

Thanks 😊

---
