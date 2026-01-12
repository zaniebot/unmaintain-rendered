```yaml
number: 6029
title: uv should be retrying on transient error of 429
type: issue
state: open
author: chrisirhc
labels:
  - needs-mre
  - network
assignees: []
created_at: 2024-08-12T09:02:29Z
updated_at: 2024-08-15T04:33:26Z
url: https://github.com/astral-sh/uv/issues/6029
synced_at: 2026-01-12T15:59:00Z
```

# uv should be retrying on transient error of 429

---

_@chrisirhc_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

This is on uv 0.2.34.

I'd expect 429 errors to be retried, but it seems to be not being retried, despite being specified in https://github.com/astral-sh/reqwest-middleware/blob/21ceec9a5fd2e8d6f71c3ea2999078fecbd13cbe/reqwest-retry/src/retryable_strategy.rs#L125 

Sample trace logs:
```
           44.515293s   1s  TRACE hyper_util::client::legacy::pool put; add idle connection for ("https", artifacts.uberinternal.com)                                                                          
           44.515313s   1s  DEBUG hyper_util::client::legacy::pool pooling idle connection for ("https", artifacts.uberinternal.com)                                                                           
           44.515350s   1s  DEBUG uv_client::base_client Transient request failure for: …/46/b3/d906c67382b8711d9612a8aff1e685f6b04
d97bd18991e21fabc197660eb/pydantic_core-2.14.4-cp311-cp311-macosx_11_0_arm64.whl#sha256=bccd5af7f05290da715623ec3e71f25d70ddb09da7925a4c2eea883f2610bad2                                                       
error: Failed to download `pydantic-core==2.14.4`
  Caused by: HTTP status client error (429 Too Many Requests) for url (…/46/b3/d906c67382b8711d9612a8aff1e685f6b04d97bd18991e21fabc
197660eb/pydantic_core-2.14.4-cp311-cp311-macosx_11_0_arm64.whl#sha256=bccd5af7f05290da715623ec3e71f25d70ddb09da7925a4c2eea883f2610bad2) 
```

I'm not seeing the "retrying" logs that were added in https://github.com/astral-sh/uv/commit/63c84ed4a630aaedea9061eedd2f489ce0f9aefa .

---

_Renamed from "`uv` should not retrying on transient error of 429" to "`uv` should be retrying on transient error of 429" by @chrisirhc on 2024-08-12 09:28_

---

_Renamed from "`uv` should be retrying on transient error of 429" to "uv should be retrying on transient error of 429" by @chrisirhc on 2024-08-12 09:28_

---

_Label `bug` added by @zanieb on 2024-08-12 12:16_

---

_Label `network` added by @zanieb on 2024-08-12 12:16_

---

_Comment by @zanieb on 2024-08-12 12:16_

Thanks for the report! Not sure why this would be happening yet.

---

_Comment by @zanieb on 2024-08-12 12:17_

What happens after that?

---

_Comment by @charliermarsh on 2024-08-12 14:44_

```rust
impl RetryableStrategy for UvRetryableStrategy {
    fn handle(&self, res: &Result<Response, reqwest_middleware::Error>) -> Option<Retryable> {
        // Use the default strategy and check for additional transient error cases.
        let retryable = match DefaultRetryableStrategy.handle(res) {
            None | Some(Retryable::Fatal) if is_extended_transient_error(res) => {
                Some(Retryable::Transient)
            }
            default => default,
        };

        // Log on transient errors
        if retryable == Some(Retryable::Transient) {
            match res {
                Ok(response) => {
                    debug!("Transient request failure for: {}", response.url());
                }
                Err(err) => {
                    let context = iter::successors(err.source(), |&err| err.source())
                        .map(|err| format!("  Caused by: {err}"))
                        .join("\n");
                    debug!(
                        "Transient request failure for {}, retrying: {err}\n{context}",
                        err.url().map(reqwest::Url::as_str).unwrap_or("unknown URL")
                    );
                }
            }
        }
        retryable
    }
}
```

I do see the `Transient request failure` logging above though.

---

_Comment by @charliermarsh on 2024-08-12 14:46_

I think we're probably missing a retry case here. It's going to be hard to test and reproduce though.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-12 14:46_

---

_Comment by @charliermarsh on 2024-08-12 14:56_

If you have any tips for successfully triggering this, LMK.

---

_Comment by @zanieb on 2024-08-12 16:53_

Yeah weird this pretty clearly looks like it's being marked as a transient failure. Can you share more logs?

---

_Comment by @charliermarsh on 2024-08-12 16:59_

My guess is we aren’t handling this properly because it’s a download failure (as opposed to any other HTTP request).

---

_Comment by @charliermarsh on 2024-08-12 18:16_

Ok, so I've actually confirmed that we do seem to retry on 429:

```
...
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/31/80/3a54838c3fb461f6fec263ebf3a3a41771bd05190238de3486aae8540c36/jinja2-3.1.4-py3-none-any.whl.metadata HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl.metadata HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl.metadata HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/bb/2a/10164ed1f31196a2f7f3799368a821765c62851ead0e630ab52b8e14b4d0/blinker-1.8.2-py3-none-any.whl.metadata HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/6a/4a/a4d49415e600bacae038c67f9fecc1d5433b9d3c71a4de6f33537b89654c/MarkupSafe-2.1.5-cp310-cp310-macosx_10_9_x86_64.whl.metadata HTTP/1.1" 429 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/6a/4a/a4d49415e600bacae038c67f9fecc1d5433b9d3c71a4de6f33537b89654c/MarkupSafe-2.1.5-cp310-cp310-macosx_10_9_x86_64.whl.metadata HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/6a/4a/a4d49415e600bacae038c67f9fecc1d5433b9d3c71a4de6f33537b89654c/MarkupSafe-2.1.5-cp310-cp310-macosx_10_9_x86_64.whl HTTP/1.1" 429 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/bb/2a/10164ed1f31196a2f7f3799368a821765c62851ead0e630ab52b8e14b4d0/blinker-1.8.2-py3-none-any.whl HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/61/80/ffe1da13ad9300f87c93af113edd0638c75138c42a0994becfacac078c06/flask-3.0.3-py3-none-any.whl HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/31/80/3a54838c3fb461f6fec263ebf3a3a41771bd05190238de3486aae8540c36/jinja2-3.1.4-py3-none-any.whl HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/9d/6e/e792999e816d19d7fcbfa94c730936750036d65656a76a5a688b57a656c4/werkzeug-3.0.3-py3-none-any.whl HTTP/1.1" 200 -
127.0.0.1 - - [12/Aug/2024 14:15:53] "GET /packages/6a/4a/a4d49415e600bacae038c67f9fecc1d5433b9d3c71a4de6f33537b89654c/MarkupSafe-2.1.5-cp310-cp310-macosx_10_9_x86_64.whl HTTP/1.1" 200 -
```

Is it possible that you hit the retry limit? Do you see multiple of those "Transient request failure for..." messages?

---

_Label `bug` removed by @charliermarsh on 2024-08-12 18:16_

---

_Label `needs-mre` added by @charliermarsh on 2024-08-12 18:16_

---

_Comment by @chrisirhc on 2024-08-13 04:48_

Sorry for the slow response, the logs actually just end there.
I’ll try reproducing later to see if I see anything else.
I also didn’t find any prior failures. 

Another thought. Does the retry count get incremented for every .netrc credential failure (the 401 before the credential is applied)? If so it might reach a retry limit just from installing many packages. I wouldn’t think so since I’d imagine the retry count is per package/request. 

---

_Comment by @chrisirhc on 2024-08-15 04:33_

I think figured out the issue. It was due to running multiple `uv` processes in parallel, causing them to compete with each other and the retry attempts to run out.

I'm not sure if there is a "fix" for this beyond adding some kind of lock between uv processes so that `UV_CONCURRENT_DOWNLOAD` doesn't exceed `UV_CONCURRENT_DOWNLOAD x uv processes`.

---
