```yaml
number: 9213
title: Automatically retry body errors when processing response
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/deny
created_at: 2024-11-19T02:08:44Z
updated_at: 2024-11-19T14:50:11Z
url: https://github.com/astral-sh/uv/pull/9213
synced_at: 2026-01-12T16:08:42Z
```

# Automatically retry body errors when processing response

---

_@charliermarsh_

## Summary

The reqwest middleware doesn't retry errors that occur "after" the request completes -- but in some cases, these do include spurious errors that we want to retry. See https://github.com/astral-sh/uv/issues/8144 for examples. This PR adds a second retry layer during the response _handler_, which should help with some of the spurious failures we see in the linked issue.

Closes https://github.com/astral-sh/uv/issues/8144.


---

_Review requested from @zanieb by @charliermarsh on 2024-11-19 02:08_

---

_Review requested from @konstin by @charliermarsh on 2024-11-19 02:08_

---

_Label `bug` added by @charliermarsh on 2024-11-19 02:08_

---

_Marked ready for review by @charliermarsh on 2024-11-19 02:08_

---

_Comment by @charliermarsh on 2024-11-19 02:09_

For reference, I ran this in `packse/index`:

```python
import http.server
import socketserver
import random
import socket
import threading

class ErrorProneHTTPRequestHandler(http.server.SimpleHTTPRequestHandler):
    error_probability = 0.2  # Default probability of triggering a connection reset

    def copyfile(self, source, outputfile):
        """
        Override the method responsible for writing the response body.
        Simulate a 'Connection reset by peer' error by abruptly closing the connection.
        """
        if random.random() < self.error_probability:
            print("Simulating connection reset by peer...")
            raise ConnectionResetError("Simulated connection reset by peer")
        else:
            # Normal behavior if no error is simulated
            super().copyfile(source, outputfile)

    def log_message(self, format, *args):
        """Customize logging to reduce verbosity."""
        print(f"{self.address_string()} - {format % args}")

def run_server(port=8080, error_probability=0.2):
    ErrorProneHTTPRequestHandler.error_probability = error_probability

    handler = ErrorProneHTTPRequestHandler

    with socketserver.TCPServer(("", port), handler) as httpd:
        print(f"Serving on port {port}, serving files")
        print(f"Error probability set to {error_probability * 100}%")
        try:
            httpd.serve_forever()
        except KeyboardInterrupt:
            print("Server shutting down...")
        finally:
            httpd.server_close()

if __name__ == "__main__":
    import argparse

    parser = argparse.ArgumentParser(description="Error-prone local HTTP server")
    parser.add_argument("--port", type=int, default=8080, help="Port to serve on")
    parser.add_argument("--error-probability", type=float, default=0.2,
                        help="Probability of simulating a connection reset error (0 to 1)")
    args = parser.parse_args()

    run_server(args.port, args.error_probability)
```

Then, from the uv repo: `echo "requires_python_wheels_a_43b963ac" | cargo run pip compile --index-url http://localhost:8082/simple-html - --no-cache --verbose`.

---

_@zanieb reviewed on 2024-11-19 02:54_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:474 on 2024-11-19 02:54_

Should this just be an `or_else` on `if let Some(err) = find_source::<reqwest_middleware::Error>(&err) ` above?

I guess that would require introducing either because the type differs or something?

---

_@zanieb reviewed on 2024-11-19 02:57_

---

_Review comment by @zanieb on `crates/uv-client/src/cached_client.rs`:623 on 2024-11-19 02:57_

Should there be some delay here on retry?

---

_@charliermarsh reviewed on 2024-11-19 02:57_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:474 on 2024-11-19 02:57_

Conceptually yes but the compiler doesn't allow it, yeah.

---

_@zanieb approved on 2024-11-19 02:58_

Nice

---

_@charliermarsh reviewed on 2024-11-19 02:58_

---

_Review comment by @charliermarsh on `crates/uv-client/src/cached_client.rs`:623 on 2024-11-19 02:58_

Oh, I guess? I can try to reuse the retry policy from the middleware.

---

_@zanieb reviewed on 2024-11-19 03:19_

---

_Review comment by @zanieb on `crates/uv-client/src/cached_client.rs`:623 on 2024-11-19 03:19_

I would definitely expect an exponential backoff.

---

_@charliermarsh reviewed on 2024-11-19 04:02_

---

_Review comment by @charliermarsh on `crates/uv-client/src/cached_client.rs`:623 on 2024-11-19 04:02_

Done, good call, thank you.

---

_Merged by @charliermarsh on 2024-11-19 04:14_

---

_Closed by @charliermarsh on 2024-11-19 04:14_

---

_Branch deleted on 2024-11-19 04:14_

---

_@jgehrcke reviewed on 2024-11-19 10:49_

---

_Review comment by @jgehrcke on `crates/uv-client/src/base_client.rs`:376 on 2024-11-19 10:49_

Just a side note: when building a retrying strategy I often like to adopt a 'deadline' approach in which an upper bound to the total time spent retrying is the key concept, and not so much the number of retry attempts.

---

_@jgehrcke reviewed on 2024-11-19 10:51_

---

_Review comment by @jgehrcke on `crates/uv-client/src/cached_client.rs`:617 on 2024-11-19 10:51_

Let's use a monotonic time source here instead.

(maybe that could be my first patch to `uv` :)).

---

_@konstin reviewed on 2024-11-19 11:32_

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:617 on 2024-11-19 11:32_

I can't comment on system clocks (@BurntSushi probably knows), but if we switch to `Instant`, we should upstream that to https://github.com/TrueLayer/reqwest-middleware/blob/8a494c165734e24c62823714843e1c9347027e8a/reqwest-retry/src/middleware.rs#L144

---

_@konstin reviewed on 2024-11-19 11:38_

Nice work!

---

_@BurntSushi reviewed on 2024-11-19 14:50_

---

_Review comment by @BurntSushi on `crates/uv-client/src/cached_client.rs`:617 on 2024-11-19 14:50_

Yes, both of these should 100% be monotonic time, or `std::time::Instant`.

For upstream, it's not just `reqwest-retry`, but in the `retry_policies` crate: https://docs.rs/retry-policies/latest/retry_policies/trait.RetryPolicy.html#tymethod.should_retry

---
