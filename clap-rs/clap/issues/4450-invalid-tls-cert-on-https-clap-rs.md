---
number: 4450
title: "Invalid TLS cert on https://clap.rs/"
type: issue
state: closed
author: itzoey
labels:
  - C-bug
assignees: []
created_at: 2022-11-04T20:52:06Z
updated_at: 2022-11-18T03:24:14Z
url: https://github.com/clap-rs/clap/issues/4450
synced_at: 2026-01-10T01:27:55Z
---

# Invalid TLS cert on https://clap.rs/

---

_Issue opened by @itzoey on 2022-11-04 20:52_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

N/A

### Clap Version

N/A

### Minimal reproducible code

N/A

### Steps to reproduce the bug with the above code

curl https://clap.rs

### Actual Behaviour

```
$ curl https://clap.rs
curl: (60) SSL certificate problem: unable to get local issuer certificate
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
```

### Expected Behaviour

https://clap.rs should have a valid certificate so it cleanly redirects to https://docs.rs/clap

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @itzoey on 2022-11-04 20:52_

---

_Comment by @kbknapp on 2022-11-08 21:49_

Appreciate the issue! Just FYI I'm at the hospital with a family member at the moment and don't have access to my laptop, but once I get home in next day or so I'll investigate and fix this.

---

_Comment by @itzoey on 2022-11-08 23:02_

No worries, this is definitely not more important than that.  
Hope everything goes well for your family member! ❤️

---

_Comment by @kbknapp on 2022-11-18 03:24_

Sorry for the wait! It turned into a week+ stay, but everyone is on the road to recovery now. This bug should be fixed. Please feel free to re-open if the problem persists or shows up again :smile:

---

_Closed by @kbknapp on 2022-11-18 03:24_

---
