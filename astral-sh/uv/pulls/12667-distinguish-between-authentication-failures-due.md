```yaml
number: 12667
title: Distinguish between authentication failures due to missing vs invalid credentials
type: pull_request
state: closed
author: jtfmumm
labels:
  - error messages
assignees: []
base: main
head: jtfm/auth-error-messages
created_at: 2025-04-04T10:16:25Z
updated_at: 2025-06-01T11:21:32Z
url: https://github.com/astral-sh/uv/pull/12667
synced_at: 2026-01-10T11:10:40Z
```

# Distinguish between authentication failures due to missing vs invalid credentials

---

_Pull request opened by @jtfmumm on 2025-04-04 10:16_

Prior to this change, unless an index was configured as `authenticate = "always"`, authentication failures would output the same error message whether the cause was missing credentials or incorrect credentials. This PR ensures that missing credentials lead to a distinct error message.

Once #12651 is merged, 
Closes #12280


---

_Label `error messages` added by @jtfmumm on 2025-04-04 10:16_

---

_Comment by @jtfmumm on 2025-04-04 10:19_

These snapshot tests will need to be updated to reflect #12651 once it is merged. So this PR should wait on #12651.

---

_@zanieb reviewed on 2025-04-04 16:26_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:592 on 2025-04-04 16:26_

Why did this one change? There's no policy set here.

---

_@zanieb reviewed on 2025-04-04 16:27_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:8468 on 2025-04-04 16:27_

This looks like a regression to me?

---

_@jtfmumm reviewed on 2025-04-04 17:15_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:592 on 2025-04-04 17:15_

Maybe there's a better way to do it, but the existing approach had been to throw an error for the missing credentials case. There's no policy set here, but the reason for the 401 is because of missing credentials.

---

_@zanieb reviewed on 2025-04-04 17:17_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:592 on 2025-04-04 17:17_

I don't think we can throw an error there though, doesn't downstream logic rely on a successful request? (i.e., in https://github.com/astral-sh/uv/pull/12667#discussion_r2029115491)

Perhaps I misunderstood the intent of this PR?

---

_@jtfmumm reviewed on 2025-04-04 17:18_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/lock.rs`:8468 on 2025-04-04 17:18_

This is a case where the fetch failed because there were no credentials, which is the reason the package was not found (since we were never authenticated, we were not able to search for it). So this case is intentional. Does it still seem like a regression? 

---

_@zanieb reviewed on 2025-04-04 17:21_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:8468 on 2025-04-04 17:21_

I think so? Doesn't this fail eagerly now? What if there was a second index with the package available?

---

_@zanieb reviewed on 2025-04-04 17:22_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:7528 on 2025-04-04 17:22_

A bit of an aside, but... why weren't we showing the hint from https://github.com/astral-sh/uv/pull/12667/files#diff-82edd36151736f44055f699a34c8b19a63ffc4cf3c86bf5fb34d69f8ac88a957L8471 in this case?

---

_@zanieb reviewed on 2025-04-04 17:23_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:8468 on 2025-04-04 17:23_

(It should be easy to add a test for that, we should probably have coverage regardless)

---

_Review comment by @jtfmumm on `crates/uv/tests/it/lock.rs`:8468 on 2025-04-04 17:24_

I'll add that test.

---

_@jtfmumm reviewed on 2025-04-04 17:24_

---

_@jtfmumm reviewed on 2025-04-04 17:25_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/lock.rs`:7528 on 2025-04-04 17:25_

That's a good question. I'll look into it

---

_@jtfmumm reviewed on 2025-04-04 17:28_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:592 on 2025-04-04 17:28_

The only tests that broke were cases where we'd want the message to indicate that missing credentials was the cause of failing to find the package. But if we are depending on getting the 401 out of this response downstream, then this approach won't work. In that case, I can try using reqwest extensions to add more context about the 401.

---

_@zanieb reviewed on 2025-04-04 17:32_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:592 on 2025-04-04 17:32_

Ah I understand the goal now, you want to make the case where no credentials were present distinct from the case where the credentials are wrong. Sorry it took me a while to get there. I think the snapshot changes should be like..

> hint: An index URL (https://pypi-proxy.fly.dev/basic-auth/simple) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).

to 

> hint: An index URL (https://pypi-proxy.fly.dev/basic-auth/simple) could not be queried due to missing credentials (401 Unauthorized)

and

> hint: An index URL (https://pypi-proxy.fly.dev/basic-auth/simple) could not be queried due to invalid credentials (401 Unauthorized)

I think changing that error path entirely feels too risky and is hopefully unnecessary. 


---

_@zanieb reviewed on 2025-04-04 18:40_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:592 on 2025-04-04 18:40_

(fwiw, I think this would have been clearer if the title was something like "Distinguish between authentication failures due to missing vs invalid credentials")

---

_Renamed from "Improve authentication failure error messages" to "Distinguish between authentication failures due to missing vs invalid credentials" by @jtfmumm on 2025-04-05 11:38_

---

_Comment by @jtfmumm on 2025-04-25 15:36_

Closing because we are going to use a different approach.

---

_Closed by @jtfmumm on 2025-04-25 15:36_

---

_Branch deleted on 2025-06-01 11:21_

---
