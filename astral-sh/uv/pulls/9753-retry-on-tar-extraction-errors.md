```yaml
number: 9753
title: Retry on tar extraction errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - network
assignees: []
merged: true
base: main
head: charlie/py-retry
created_at: 2024-12-09T22:06:53Z
updated_at: 2024-12-10T12:33:11Z
url: https://github.com/astral-sh/uv/pull/9753
synced_at: 2026-01-12T16:08:58Z
```

# Retry on tar extraction errors

---

_@charliermarsh_

## Summary

So the error here is:

```rust
ExtractError("cpython-3.11.11%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz", Io(Custom { kind: UnexpectedEof, error: TarError { desc: "failed to unpack `/Users/crmarsh/.local/share/uv/python/.cache/.tmpkqFzqE/python/lib/libpython3.11.dylib`", io: Custom { kind: UnexpectedEof, error: TarError { desc: "failed to unpack `python/lib/libpython3.11.dylib` into `/Users/crmarsh/.local/share/uv/python/.cache/.tmpkqFzqE/python/lib/libpython3.11.dylib`", io: Custom { kind: UnexpectedEof, error: "unexpected end of file" } } } } }))
```

This isn't a Reqwest error, so we miss it in `is_extended_transient_error`.

We could add `TarError` or `ExtractError` here, but... should we? This PR just extends it to any error that has an IO source. I don't see much of a downside.

Closes https://github.com/astral-sh/uv/issues/9747.

## Test Plan

First, ran: `uv run ./scripts/create-python-mirror.py --name cpython --arch aarch64 --os darwin`.

Then, dropped this into `./scripts/mirror/server.py`:

```python
import os
import random
from http.server import SimpleHTTPRequestHandler, HTTPServer


class GlitchyStaticServer(SimpleHTTPRequestHandler):
    def do_GET(self):
        """Handle GET request."""
        file_path = self.translate_path(self.path)
        
        if not os.path.exists(file_path):
            self.send_error(404, "File not found")
            return
        
        try:
            with open(file_path, 'rb') as f:
                file_content = f.read()

            # Introduce an "unexpected end of file" glitch randomly
            if random.random() < 0.75:  # 75% chance of glitch
                glitch_point = random.randint(1, len(file_content) - 1)
                file_content = file_content[:glitch_point]

            self.send_response(200)
            self.send_header("Content-type", self.guess_type(file_path))
            self.send_header("Content-Length", len(file_content))
            self.end_headers()
            self.wfile.write(file_content)
        
        except Exception as e:
            self.send_error(500, f"Internal Server Error: {e}")
        

def run(server_class=HTTPServer, handler_class=GlitchyStaticServer, port=8080):
    """Run the server."""
    server_address = ('', port)
    httpd = server_class(server_address, handler_class)
    print(f"Serving on port {port} with glitchy behavior")
    httpd.serve_forever()


if __name__ == "__main__":
    run()
```

Then ran `python server.py` from that directory.

From there, ran `UV_PYTHON_INSTALL_MIRROR="http://localhost:8080" cargo run python install 3.11 --reinstall --verbose` to reliably test retries.


---

_Review requested from @zanieb by @charliermarsh on 2024-12-09 22:07_

---

_Review requested from @konstin by @charliermarsh on 2024-12-09 22:07_

---

_Label `bug` added by @charliermarsh on 2024-12-09 22:07_

---

_Label `network` added by @charliermarsh on 2024-12-09 22:07_

---

_@konstin approved on 2024-12-10 08:47_

nice!

---

_Merged by @charliermarsh on 2024-12-10 12:33_

---

_Closed by @charliermarsh on 2024-12-10 12:33_

---

_Branch deleted on 2024-12-10 12:33_

---
