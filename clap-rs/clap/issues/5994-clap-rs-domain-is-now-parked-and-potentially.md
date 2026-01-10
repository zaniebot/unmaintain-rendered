---
number: 5994
title: clap.rs domain is now parked and potentially unsafe
type: issue
state: open
author: fool-in-the-rain
labels: []
assignees: []
created_at: 2025-05-08T16:09:15Z
updated_at: 2025-11-26T18:27:33Z
url: https://github.com/clap-rs/clap/issues/5994
synced_at: 2026-01-10T01:28:20Z
---

# clap.rs domain is now parked and potentially unsafe

---

_Issue opened by @fool-in-the-rain on 2025-05-08 16:09_

The domain https://clap.rs, previously listed in documentation and tutorials as the official site for the clap crate, now appears to be parked or hijacked.

Visiting the domain shows a generic page with ad links (see attached screenshot). This is potentially misleading or harmful to users.

It may be worth:

Updating all official links to point to https://docs.rs/clap

Issuing a warning in the README to avoid visiting clap.rs

Reclaiming or blocking the domain, if possible

---

_Comment by @epage on 2025-05-08 16:16_

@kbknapp any input on the domain?

> Updating all official links to point to https://docs.rs/clap

I don't think we've linked to `clap.rs` for a while.

---

_Comment by @kbknapp on 2025-05-08 18:49_

Yeah we lost access to the domain clap.rs because the registrar we used changed acceptable payment methods and I no longer had a way that I could pay (being that I'm in the US).

Im totally open to re-engaging or if anyone has suggestions for a .rs registrar the project has funding to pay for a domain again.

---

_Comment by @YDX-2147483647 on 2025-11-26 03:56_

> I don't think we've linked to clap.rs for a while.

It's still linked on https://github.com/clap-rs.

<img width="400" alt="Image" src="https://github.com/user-attachments/assets/36d64af3-9c7e-4270-b53a-d6eeffcef4cc" />

> if anyone has suggestions for a .rs registrar the project has funding to pay for a domain again.

https://github.com/zackify/cli.rs has been offer free `*.cli.rs` for 5 years.

---

_Comment by @epage on 2025-11-26 16:47_

Missed that there was still a link present.  @kbknapp can you remove that?

---

_Comment by @kbknapp on 2025-11-26 18:25_

Yep I'll remove that. The behind the scenes is I no longer have a way to pay for the .rs domain from our previous registrar, they stopped accepting any form of payment that I can give. 

---

_Comment by @kbknapp on 2025-11-26 18:27_

Should be pointing at https://docs.rs/clap now

---
