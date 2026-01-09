---
number: 14768
title: hovering over variable in jupyter notebook causes ruff server to crash
type: issue
state: closed
author: TheOnlyWayUp
labels:
  - bug
  - notebook
  - server
assignees: []
created_at: 2024-12-04T09:51:30Z
updated_at: 2025-01-10T13:13:57Z
url: https://github.com/astral-sh/ruff/issues/14768
synced_at: 2026-01-07T13:12:16-06:00
---

# hovering over variable in jupyter notebook causes ruff server to crash

---

_Issue opened by @TheOnlyWayUp on 2024-12-04 09:51_

https://files.rambhat.la/s/ySXwKiXNSLwcyde

```py
from langchain_core.prompts import ChatPromptTemplate

system = """..."""

prompt = ChatPromptTemplate.from_messages([("system", system), ("human", "{input}")])

prompted_llm = prompt | llm
output = prompted_llm.invoke(texts[1])
print(output)
```

```
2024-12-04 09:50:35.199 [info] panicked at crates/ruff_server/src/server/api/requests/hover.rs:36:10:
hover should only be called on text documents or notebook cells

2024-12-04 09:50:35.199 [info]    0: ruff_server::server::Server::run::{{closure}}
   1: std::panicking::rust_panic_with_hook
   2: std::panicking::begin_panic_handler::{{closure}}

2024-12-04 09:50:35.200 [info]    3: std::sys::backtrace::__rust_end_short_backtrace
   4: rust_begin_unwind
   5: core::panicking::panic_fmt
   
2024-12-04 09:50:35.200 [info] 6: core::option::expect_failed
   7: <ruff_server::server::api::requests::hover::Hover
2024-12-04 09:50:35.200 [info]  as ruff_server::server::api::traits::BackgroundDocumentRequestHandler>::run_with_snapshot
   8: core::
2024-12-04 09:50:35.200 [info] ops::function::FnOnce::call_once{{vtable.shim}}
   9: core::ops
2024-12-04 09:50:35.200 [info] ::function::FnOnce::call_once{{vtable.shim}}
  10: std::sys::backtrace::__rust_begin_short_backtrace
 
2024-12-04 09:50:35.200 [info]  11: core::ops::function::FnOnce::call_once{{vtable.shim}}
  
2024-12-04 09:50:35.200 [info] 12: std::sys::pal::unix::thread::Thread::new::thread_start
  13: 
2024-12-04 09:50:35.200 [info] start_thread
  14: clone
```

---

_Comment by @dhruvmanila on 2024-12-04 11:06_

This is most likely same as https://github.com/astral-sh/ruff-vscode/issues/644. Can you make sure you're on the latest Ruff version and provide us the detailed logs by turning on server logs using the following config?

```json
{
	"ruff.server.trace": "verbose",
	"ruff.logLevel": "debug",
}
```

You can find more details here: https://github.com/astral-sh/ruff-vscode#troubleshooting. I'm specifically looking for a message that's been added in https://github.com/astral-sh/ruff/pull/14579.

---

_Label `bug` added by @dhruvmanila on 2024-12-04 11:10_

---

_Label `server` added by @dhruvmanila on 2024-12-04 11:10_

---

_Comment by @dhruvmanila on 2024-12-04 11:11_

I think even just upgrading the Ruff version should provide us a better message when the panic occurs.

---

_Comment by @TheOnlyWayUp on 2024-12-04 11:14_

> {
> 	"ruff.server.trace": "verbose",
> 	"ruff.logLevel": "debug",
> }

I've added this to my settings.json, don't see any debug messages in Output yet. I haven't been able to replicate the error (hovering doesn't cause a crash).

I'll leave the debugging enabled incase it pops up again.



---

_Comment by @dhruvmanila on 2024-12-04 11:17_

> I'll leave the debugging enabled incase it pops up again.

Thank you!

---

_Referenced in [astral-sh/ruff#15398](../../astral-sh/ruff/pulls/15398.md) on 2025-01-10 12:25_

---

_Closed by @dhruvmanila on 2025-01-10 13:11_

---

_Closed by @dhruvmanila on 2025-01-10 13:11_

---

_Label `notebook` added by @dhruvmanila on 2025-01-10 13:13_

---
