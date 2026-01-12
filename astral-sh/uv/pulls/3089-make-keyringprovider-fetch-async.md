```yaml
number: 3089
title: "Make KeyringProvider::fetch_* async"
type: pull_request
state: merged
author: naosense
labels:
  - performance
assignees: []
merged: true
base: main
head: main
created_at: 2024-04-17T06:14:21Z
updated_at: 2024-04-23T12:58:13Z
url: https://github.com/astral-sh/uv/pull/3089
synced_at: 2026-01-12T16:05:25Z
```

# Make KeyringProvider::fetch_* async

---

_@naosense_

To resolve #3073 


---

_@naosense reviewed on 2024-04-17 06:20_

---

_Review comment by @naosense on `crates/uv-auth/src/middleware.rs`:146 on 2024-04-17 06:20_

hi @zanieb   I googled for a while, but still no idea how to edit this. It reports `await` can't be called from a closure.

---

_@zanieb reviewed on 2024-04-17 14:36_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:146 on 2024-04-17 14:36_

I believe the idiomatic way to do it is with `match` instead

```diff
diff --git a/crates/uv-auth/src/middleware.rs b/crates/uv-auth/src/middleware.rs
index 191ca92e..451f690a 100644
--- a/crates/uv-auth/src/middleware.rs
+++ b/crates/uv-auth/src/middleware.rs
@@ -136,19 +136,25 @@ impl Middleware for AuthMiddleware {
         //      falls back to the host, but we cache the result per host so if a keyring
         //      implementation returns different credentials for different URLs in the
         //      same realm we will use the wrong credentials.
-        } else if let Some(credentials) = self.keyring.as_ref().and_then(|keyring| {
-            if let Some(username) = credentials
-                .get()
-                .and_then(|credentials| credentials.username())
-            {
-                debug!("Checking keyring for credentials for {url}");
-                // how to fix this
-                keyring.fetch(request.url(), username)
-            } else {
-                trace!("Skipping keyring lookup for {url} with no username");
-                None
+        } else if let Some(credentials) = match self.keyring {
+            Some(ref keyring) => {
+                match credentials
+                    .get()
+                    .and_then(|credentials| credentials.username())
+                {
+                    Some(username) => {
+                        debug!("Checking keyring for credentials for {url}");
+                        // how to fix this
+                        keyring.fetch(request.url(), username).await
+                    }
+                    None => {
+                        trace!("Skipping keyring lookup for {url} with no username");
+                        None
+                    }
+                }
             }
-        }) {
+            None => None,
+        } {
             debug!("Found credentials in keyring for {url}");
             request = credentials.authenticate(request);
             new_credentials = Some(Arc::new(credentials));
```

Then you'll run into problems with the `Mutex` being synchronous.

---

_Comment by @zanieb on 2024-04-18 04:09_

Fyi we're chatting about using the tokio mutex over in https://github.com/astral-sh/uv/pull/3105 – it might not be necessary.

---

_Comment by @naosense on 2024-04-18 07:21_

> Fyi we're chatting about using the tokio mutex over in #3105 – it might not be necessary.

Ok, i see. Now i split the cache lock to avoid across `await` call.

---

_Marked ready for review by @naosense on 2024-04-18 08:30_

---

_Renamed from "WIP: Make KeyringProvider::fetch_* async" to "Make KeyringProvider::fetch_* async" by @naosense on 2024-04-18 11:38_

---

_Comment by @naosense on 2024-04-20 02:30_

Hey @zanieb, i think this pr is ready to review, let me know if there's anything else i need to do.

---

_@zanieb reviewed on 2024-04-22 18:15_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-22 18:15_

Resolved right?
```suggestion
```

---

_Label `performance` added by @zanieb on 2024-04-22 18:16_

---

_Review comment by @naosense on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-23 02:49_

sorry, forgot to remove

---

_@naosense reviewed on 2024-04-23 02:49_

---

_Comment by @zanieb on 2024-04-23 02:54_

Ah it looks like you have some conflicts with my changes in #3130, sorry about that lmk if you want me to fix it.

We also _might_ want to switch to the async mutex since we do want to hold across await points now, interesting. We're also considering switching to a `RwLock`. I think all that is probably worth doing/considering separately.

---

_Comment by @naosense on 2024-04-23 02:59_

OK, thanks for you information, I think it makes sense to split these changes into multiple PR, what do you think?

---

_Merged by @zanieb on 2024-04-23 12:58_

---

_Closed by @zanieb on 2024-04-23 12:58_

---

_Comment by @zanieb on 2024-04-23 12:58_

Thank you!

---
