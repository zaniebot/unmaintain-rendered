```yaml
number: 15979
title: Add X-Error-Message header for error context
type: pull_request
state: closed
author: doddi
labels: []
assignees: []
draft: true
base: main
head: Allow-error-details-in-header
created_at: 2025-09-22T13:20:46Z
updated_at: 2025-10-27T11:05:12Z
url: https://github.com/astral-sh/uv/pull/15979
synced_at: 2026-01-12T16:12:03Z
```

# Add X-Error-Message header for error context

---

_@doddi_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
HTTP1.1 [RFC 9112 - HTTP/1.1](https://www.rfc-editor.org/rfc/rfc9112.html#name-status-line) section 4 defines the response status code to optionally include a text description (human readable) of the reason for the status code.

[RFC9113 - HTTP/2](https://www.rfc-editor.org/rfc/rfc9113) is the HTTP2 protocol standard and the response status only considers the [status code](https://www.rfc-editor.org/rfc/rfc9113#name-response-pseudo-header-fiel) and not the reason phrase, as such important information can be lost in helping the client determine a route cause of a failure.

<!-- What's the purpose of the change? What does it do, and why? -->
## Purpose
Allow error header in a HEAD response

Similar to [huggingface](https://github.com/huggingface/huggingface_hub/blob/e5c84bcc04ab32f2b2d2f5733cbd2b54f759876a/src/huggingface_hub/utils/_http.py#L411) have a X-Error-Code and a X-Error-Message header.

X-Error-Code: defines a grouping of the error type
X-Error-Message: detailed message enhancing the reason for the failure

For this PR I opted to only consider the `X-Error-Message` header to avoid making the solution overly complicated for its need.

This will allow the following:
- Enhance the information displayed to the user when an error status is received. Both HEAD and GET requests will be able to provide context.
- This solution will allow error information to be transmitted over any http version.

## Test Plan

Created a server that additionally add a `X-Error-Message` header response when an error occurs. Observe in the cli that the message is displayed verbatim.

<!-- How was it tested? -->
<img width="3167" height="121" alt="image" src="https://github.com/user-attachments/assets/1b20f7a6-4021-4d90-a33b-f7c6acb45077" />

Note: There are 2 entries for the error in the screenshot above because this test was over http1.1 which additionally sends the same message in the 'traditional' errorPhrase section.


---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1586 on 2025-09-22 13:42_

This is a bit drive-by, as I haven't had time to look at the rest of the pull request in depth, but... it seems wrong to drop the rest of our messaging here. Can you explain the motivation for that? I'd expect the custom messaging to just be an addendum.

---

_@zanieb reviewed on 2025-09-22 13:42_

---

_@doddi reviewed on 2025-09-22 13:49_

---

_Review comment by @doddi on `crates/uv-resolver/src/pubgrub/report.rs`:1586 on 2025-09-22 13:49_

I can update to include the existing messaging (not drop) 👍 

---

_@doddi reviewed on 2025-09-22 13:58_

---

_Review comment by @doddi on `crates/uv-resolver/src/pubgrub/report.rs`:1586 on 2025-09-22 13:58_

https://github.com/astral-sh/uv/pull/15979/commits/f686147981142b710693b16c49af08a039db4ef7

---

_Comment by @konstin on 2025-09-25 09:05_

I see there's an example in the screenshot, but can you say a bit more about which server uses this header and how, so we get a complete picture of the user experience?

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:595 on 2025-09-25 09:06_

Why only for those status codes?

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:597 on 2025-09-25 09:08_

nit: inline this function

---

_Review comment by @konstin on `crates/uv-client/src/error.rs`:475 on 2025-09-25 09:09_

We need to show that this is something coming from the server and not uv or the os, e.g. `Server says:`

---

_Review comment by @konstin on `crates/uv-distribution-types/src/status_code_strategy.rs`:107 on 2025-09-25 09:14_

Do we need to recurse here or can we do an early return for `Self::IgnoreErrorCodes` and `status_codes.contains(&status_code)` instead?

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:588 on 2025-09-25 09:19_

I would make this a `WrappedReqwestError` method - it seems useful to have this universally available.

---

_Review comment by @konstin on `crates/uv-client/src/error.rs`:502 on 2025-09-25 09:20_

Can you add an integration test? There are some references with `MockServer` e.g. in `pip_install.rs`

---

_@konstin reviewed on 2025-09-25 09:21_

---

_Comment by @woodruffw on 2025-09-25 15:36_

> I see there's an example in the screenshot, but can you say a bit more about which server uses this header and how, so we get a complete picture of the user experience?

I agree with this -- a quick search online shows that there's a _lot_ of variance in third-party headers used for error messaging (`X-Error-Message`, `X-Error`, `X-Err-Msg`, etc.), and I think accepting one non-standard header would establish a precedent for accepting other non-standard headers (which presents a long term quality/maintenance risk IMO, since we can't assert that these headers are plain text and not JSON or some other serialized form).

@doddi Out of curiosity, have you engaged with the Python packaging standards processes for this use case? I think there would be an immense amount of value in having Python package indices align on error response behavior/encoding in index API (PEP 503/691) responses, and would make the integration story a lot simpler and more uniform here.

---

_@woodruffw reviewed on 2025-09-25 15:37_

---

_Review comment by @woodruffw on `crates/uv-client/src/cached_client.rs`:595 on 2025-09-25 15:37_

Yeah, if I understand correctly we should _probably_ check for this on `400..=599`, i.e. all client- and server- originated error responses?

---

_Comment by @doddi on 2025-09-26 13:18_

> > I see there's an example in the screenshot, but can you say a bit more about which server uses this header and how, so we get a complete picture of the user experience?
> 
> I agree with this -- a quick search online shows that there's a _lot_ of variance in third-party headers used for error messaging (`X-Error-Message`, `X-Error`, `X-Err-Msg`, etc.), and I think accepting one non-standard header would establish a precedent for accepting other non-standard headers (which presents a long term quality/maintenance risk IMO, since we can't assert that these headers are plain text and not JSON or some other serialized form).
> 
> @doddi Out of curiosity, have you engaged with the Python packaging standards processes for this use case? I think there would be an immense amount of value in having Python package indices align on error response behavior/encoding in index API (PEP 503/691) responses, and would make the integration story a lot simpler and more uniform here.

@woodruffw I have not engaged with the Python packaging standards process, I was not aware of such a thing :). Would you recommend this as a first step? if so could you suggest what would be the best approach?

As for the screenshot this is from Sonatype Nexus Repository Manager, we are currently working through all our supported formats and determining how we can support such error types when communicating using http2. For our use case we generate a 403 response when we determine that a download of a component should be blocked due to violating a policy, such as being malicious or perhaps licensing. The response usually includes a url link to a report on why the download was blocked. Moving to http2 (no reasonPhrase option) the end user is simply presented with a 403 and no context as to why a download 'failed'.

---

_Comment by @woodruffw on 2025-09-26 15:19_

> @woodruffw I have not engaged with the Python packaging standards process, I was not aware of such a thing :). Would you recommend this as a first step? if so could you suggest what would be the best approach?

Yeah, I think this would be a good starting point -- changes to standard interfaces (like the index) are IMO a *lot* easier to justify if they can be substantiated through the standards process 🙂 

In terms of starting: I think a discussion thread on the [packaging forum](https://discuss.python.org/c/packaging/14) is probably right starting point -- the community there is familiar with the rationale behind the original PEP 503/691 specifications, and will likely have useful feedback on whether making explicit accommodations for error states makes sense.

However, to take a step back, there *might* be an existing solution that works for you:

> For our use case we generate a 403 response when we determine that a download of a component should be blocked due to violating a policy, such as being malicious or perhaps licensing. The response usually includes a url link to a report on why the download was blocked. Moving to http2 (no reasonPhrase option) the end user is simply presented with a 403 and no context as to why a download 'failed'.

Hmm, have you looked at implementing [PEP 792](https://peps.python.org/pep-0792/) for this? That's a pre-existing standard mechanism that allows the index to communicate to clients whether a project has been quarantined (among other states). 

The main restriction with PEP 792 is that it operates at the *project* level, not the per-file level: you set the status marker in the index response itself. I'm not sure if that makes sense for your use case, but if it does you'll get compatibility with uv and other installers for "free" without having to make any kind of changes or standards efforts 🙂 

---

_Comment by @doddi on 2025-09-26 16:21_

> > @woodruffw I have not engaged with the Python packaging standards process, I was not aware of such a thing :). Would you recommend this as a first step? if so could you suggest what would be the best approach?
> 
> Yeah, I think this would be a good starting point -- changes to standard interfaces (like the index) are IMO a _lot_ easier to justify if they can be substantiated through the standards process 🙂
> 
> In terms of starting: I think a discussion thread on the [packaging forum](https://discuss.python.org/c/packaging/14) is probably right starting point -- the community there is familiar with the rationale behind the original PEP 503/691 specifications, and will likely have useful feedback on whether making explicit accommodations for error states makes sense.
> 
> However, to take a step back, there _might_ be an existing solution that works for you:
> 
> > For our use case we generate a 403 response when we determine that a download of a component should be blocked due to violating a policy, such as being malicious or perhaps licensing. The response usually includes a url link to a report on why the download was blocked. Moving to http2 (no reasonPhrase option) the end user is simply presented with a 403 and no context as to why a download 'failed'.
> 
> Hmm, have you looked at implementing [PEP 792](https://peps.python.org/pep-0792/) for this? That's a pre-existing standard mechanism that allows the index to communicate to clients whether a project has been quarantined (among other states).
> 
> The main restriction with PEP 792 is that it operates at the _project_ level, not the per-file level: you set the status marker in the index response itself. I'm not sure if that makes sense for your use case, but if it does you'll get compatibility with uv and other installers for "free" without having to make any kind of changes or standards efforts 🙂

That is really useful information re PEP792 but sadly we need to communicate policy violations on a per version basis. Thanks again for the information, I will reach out to the PEP standards 👍 

---

_Comment by @doddi on 2025-09-30 11:00_

I have opened up a thread for discussion https://discuss.python.org/t/block-download-of-components-when-violating-policy/104021. Probably best I close this PR for now and see what traction I get over there 👍 

---

_Converted to draft by @konstin on 2025-09-30 12:44_

---

_Closed by @doddi on 2025-10-27 11:05_

---
