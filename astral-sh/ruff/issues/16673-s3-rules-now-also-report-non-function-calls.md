---
number: 16673
title: "S3*: Rules now also report non-function calls"
type: issue
state: open
author: MichaReiser
labels:
  - bug
assignees: []
created_at: 2025-03-12T12:46:08Z
updated_at: 2025-03-12T12:46:08Z
url: https://github.com/astral-sh/ruff/issues/16673
synced_at: 2026-01-10T01:22:57Z
---

# S3*: Rules now also report non-function calls

---

_Issue opened by @MichaReiser on 2025-03-12 12:46_

### Summary

https://github.com/astral-sh/ruff/pull/15541 changed the S3* rules to report all references instead of only checking for the problematic functions in call expressions (callee) positions to detect them when used in callback positions. 

I think there are two issues with the change: 

1. The rule uses the same error message regardless if the function is used in a call position or not: *S321 FTP-related functions are being called. FTP is considered insecure. Use SSH/SFTP/SCP or some other encrypted protocol.* The `are being called` part can be confusing when there's no actual call, e.g. `port = ftplib.FTP_PORT`
2. The rule now also reports usages for symbols that aren't functions
  ```py
          if self.conn is None:
            params = self.get_connection(self.ftp_conn_id)
            pasv = params.extra_dejson.get("passive", True)
            self.conn = ftplib.FTP()  # nosec: B321
            if params.host:
                port = ftplib.FTP_PORT
                if params.port is not None:
                    port = params.port
                logger.info("Connecting via FTP to %s:%d", params.host, port)
                self.conn.connect(params.host, port)
                if params.login:
                    self.conn.login(params.login, params.password)
            self.conn.set_pasv(pasv)
  ```

Some of this could be addressed by rephrasing the message or we want a more sophisticated check to e.g. only check for usages in function call arguments rather than references anywhere in code.


### Version

_No response_

---

_Label `bug` added by @MichaReiser on 2025-03-12 12:46_

---
