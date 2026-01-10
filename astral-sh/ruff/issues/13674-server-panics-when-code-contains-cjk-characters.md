```yaml
number: 13674
title: Server panics when code contains CJK characters
type: issue
state: closed
author: HusuSama
labels:
  - bug
  - server
assignees: []
created_at: 2024-10-08T06:39:41Z
updated_at: 2024-11-01T07:16:55Z
url: https://github.com/astral-sh/ruff/issues/13674
synced_at: 2026-01-10T11:09:55Z
```

# Server panics when code contains CJK characters

---

_Issue opened by @HusuSama on 2024-10-08 06:39_

When I use Click, errors occur. I'm not sure if it's because of my Chinese comments or the decorators.

log:
![图片](https://github.com/user-attachments/assets/92167a53-08c6-4ee0-96d6-e43c2bbf6b91)

code:
![图片](https://github.com/user-attachments/assets/14b691ad-5448-4a70-abe3-06f9f2470e29)


---

_Label `bug` added by @MichaReiser on 2024-10-08 07:09_

---

_Label `server` added by @MichaReiser on 2024-10-08 07:09_

---

_Comment by @dhruvmanila on 2024-10-25 05:04_

@HusuSama Sorry, somehow I missed this. Can you provide the code content as text instead of an image? I can copy paste that and try to reproduce it. Additionally, can you provide the Ruff config if it's being used here (both server settings and Ruff configuration)?

---

_Renamed from "When I use click, it causes a panic." to "Server panics when code contains CJK characters" by @dhruvmanila on 2024-10-25 05:05_

---

_Comment by @HusuSama on 2024-10-28 01:35_

Thank you for noticing the issue I raised. I haven't made any additional configurations for Ruff, and I'm using the AstroNvim configuration. Here is the configuration content: [AstroNvim's Ruff Configuration](https://github.com/AstroNvim/astrocommunity/tree/main/lua/astrocommunity/pack/python-ruff).

I can't fully pinpoint this issue; it seems to be random. It tends to occur more frequently when I add Chinese comments at the top, and the usage is very unstable. I'm also not quite sure if it's an issue with my Neovim configuration. By the way, I use `Tab` for code completion.

my code:

```python
"""
测试comment
一些测试内容
"""

import click


@click.group()
def interface():
    pass


@interface.command()
def test():
    click.echo("hello")


if __name__ == "__main__":
    interface()

```


---

_Comment by @dhruvmanila on 2024-10-28 04:50_

Thanks! I've a couple of questions:
1. Can you describe the scenario which produces the panic? It could be like something you clicked, requested the server for something (like hover request), while typing etc.
2. What's the Ruff version that is being used here?
3. Can you provide debug logs for the server? I'm not sure how to set the config for AstroNvim but for `lsp-config` you can refer to the end of [this section in the docs](https://docs.astral.sh/ruff/editors/setup/#neovim).

---

_Label `bug` removed by @dhruvmanila on 2024-10-28 04:50_

---

_Label `needs-mre` added by @dhruvmanila on 2024-10-28 04:50_

---

_Comment by @HusuSama on 2024-10-29 01:28_

1. I suspect that the issue might be related to my code completion settings. When I use TAB to automatically select the first code suggestion, this error occurs. However, if I first move the cursor and then use TAB to select, the error does not appear. 
![图片](https://github.com/user-attachments/assets/50837288-88fe-40cb-8a1b-4a0c528ded5f)
![图片](https://github.com/user-attachments/assets/959a8f06-a136-404e-a2e6-6e52c7042ae3)

2. ruff version
![图片](https://github.com/user-attachments/assets/9692ce28-ab14-4e88-a9dd-56084463cdd7)

3. I only have the LSP error log
```log
[START][2024-10-29 09:10:40] LSP logging initiated
[ERROR][2024-10-29 09:10:40] ...lsp/handlers.lua:623    "Notification workspace/didChangeConfiguration defines parameters by name but received parameters by position"
[ERROR][2024-10-29 09:10:40] ...lsp/handlers.lua:623    "Notification workspace/didChangeConfiguration defines 1 params but received 0 arguments"
[ERROR][2024-10-29 09:11:05] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        'panicked at crates/ruff_server/src/edit/range.rs:68:71:\nbyte index 96 is out of bounds of `"""\n测试comment\n一些测试内容\n"""\nimport click\n\n@click.group()\ndef interface():\n    pas\n`\n'
[ERROR][2024-10-29 09:11:05] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   "
[ERROR][2024-10-29 09:11:05] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "0: ruff_server::server::Server::run::{{closure}}\n   1: std::panicking::rust_panic_with_hook\n   2: std::panicking::begin_panic_handler::{{closure}}\n   3: std::sys::backtrace::__rust_end_short_backtrace\n   4: rust_begin_unwind\n   5: core::panicking::panic_fmt\n   6: core"
[ERROR][2024-10-29 09:11:05] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "::str::slice_error_fail_rt\n   7: core::str::slice_error_fail\n   8: <lsp_types::Range as ruff_server::edit::range::RangeExt>::to_text_range\n   9: ruff_server::edit::text_document::TextDocument::apply_changes\n  10: ruff_server::session::index::Index::update_text_document\n  11: <ruff_server::server::api::notifications::did_change::DidChange as ruff_server::server::api::traits::SyncNotificationHandler>::run\n  12: core::ops::function::FnOnce::call_once{{vtable.shim}}\n  13: std::sys::backtrace::__rust_begin_short_backtrace\n  14: core::ops::function::FnOnce::call_once{{vtable.shim}}\n  15: std::sys::pal::unix::thread::Thread::new::thread_start\n  16: <unknown>\n  17: <unknown>\n\n"
[ERROR][2024-10-29 09:11:05] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/jod-thread-0.1.2/src/lib.rs:33:22:\ncalled `Result::unwrap()` on an `Err` value: Any { .. }\n"
[ERROR][2024-10-29 09:11:05] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   0: "
[ERROR][2024-10-29 09:11:05] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "ruff_server::server::Server::run::{{closure}}\n   1: std::panicking::rust_panic_with_hook\n   2: std::panicking::begin_panic_handler::{{closure}}\n   3: std::sys::backtrace::__rust_end_short_backtrace\n   4: rust_begin_unwind\n   5: core::panicking::panic_fmt\n   6: core::result::unwrap_failed\n   7: ruff_server::server::Server::run\n "
[ERROR][2024-10-29 09:11:05] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  8: ruff::run\n   9: ruff::main\n  10: std::sys::backtrace::__rust_begin_short_backtrace\n  11: std::rt::lang_start::{{closure}}\n  12: std::rt::lang_start_internal\n  13: main\n  14: <unknown>\n  15: __libc_start_main\n  16: <unknown>\n\n"
```

---

_Comment by @HusuSama on 2024-10-29 01:32_

Do I need to make the following settings?
![图片](https://github.com/user-attachments/assets/24ac2961-5a05-4375-810d-7ca89bc3fb3a)

The log content after I made the settings.

```log
[START][2024-10-29 09:39:45] LSP logging initiated
[ERROR][2024-10-29 09:39:45] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   0.000021229s  INFO main ruff_server::server: No workspace settings found for file:///home/husu/PythonProjects/ZhiWei/ZcloudInterface, using default settings\n   0.004739909s DEBUG ThreadId(04) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/.ruff_cache\n   0.004769454s DEBUG ThreadId(04) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/.git\n   0.004944608s DEBUG ThreadId(09) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/.venv\n   0.004984051s DEBUG ThreadId(10) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/.pytest_cache\n   0.006152245s  INFO main ruff_server::session::index: Registering workspace: /home/husu/PythonProjects/ZhiWei/ZcloudInterface\n   0.006695351s  WARN ruff:main ruff_server::server: LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.\n"
[ERROR][2024-10-29 09:39:45] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   0.020612467s DEBUG ruff:worker:0 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:46] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   0.557364854s DEBUG ruff:worker:1 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:49] ...lsp/handlers.lua:623    "Notification workspace/didChangeConfiguration defines parameters by name but received parameters by position"
[ERROR][2024-10-29 09:39:49] ...lsp/handlers.lua:623    "Notification workspace/didChangeConfiguration defines 1 params but received 0 arguments"
[ERROR][2024-10-29 09:39:50] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   4.661554269s DEBUG ruff:worker:3 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:50] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   4.866011638s DEBUG ruff:worker:4 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:50] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   5.086144569s DEBUG ruff:worker:5 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:50] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   5.260883897s DEBUG ruff:worker:6 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:50] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   5.341480461s DEBUG ruff:worker:7 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:51] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   5.449400862s DEBUG ruff:worker:8 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:51] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   5.623161907s DEBUG ruff:worker:9 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:52] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   6.918943214s DEBUG ruff:worker:10 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:52] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   7.064222808s DEBUG ruff:worker:11 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:53] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   7.716647234s DEBUG ruff:worker:12 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:53] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   7.717617050s DEBUG ruff:worker:15 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:54] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   8.628225531s DEBUG ruff:worker:13 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:54] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   8.896196725s DEBUG  ruff:worker:0 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:55] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   9.505942274s DEBUG  ruff:worker:1 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:55] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   9.787189927s DEBUG  ruff:worker:2 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:55] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "   9.919957468s DEBUG  ruff:worker:3 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:55] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  10.109705453s DEBUG  ruff:worker:4 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:55] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  10.191422562s DEBUG  ruff:worker:5 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:56] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  10.990491409s DEBUG  ruff:worker:6 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:56] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  11.281379026s DEBUG  ruff:worker:7 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:57] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  11.556823721s DEBUG  ruff:worker:8 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:57] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  11.667960930s DEBUG  ruff:worker:9 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:58] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  13.394465046s DEBUG ruff:worker:10 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:59] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  14.027106711s DEBUG ruff:worker:11 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:39:59] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  14.312729748s DEBUG ruff:worker:12 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:40:00] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  14.535848280s DEBUG ruff:worker:15 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:40:00] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  14.655770138s DEBUG ruff:worker:14 ruff_server::resolve: Included path via `include`: /home/husu/PythonProjects/ZhiWei/ZcloudInterface/test.py\n"
[ERROR][2024-10-29 09:40:00] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        '  15.379339018s ERROR      ruff:main ruff_server::server: panicked at crates/ruff_server/src/edit/range.rs:68:71:\nbyte index 68 is out of bounds of `"""\n测试comment\n一些测试内容\n"""\nimport click\n\n@click.go\n`\n   0: ruff_server::server::Server::run::{{closure}}\n   1: std::panicking::rust_panic_with_hook\n   2: std::panicking::begin_panic_handler::{{closure}}\n   3: std::sys::backtrace::__rust_end_short_backtrace\n   4: rust_begin_unwind\n   5: core::panicking::panic_fmt\n   6: core::str::slice_error_fail_rt\n   7: core::str::slice_error_fail\n   8: <lsp_types::Range as ruff_server::edit::range::RangeExt>::to_text_range\n   9: ruff_server::edit::text_document::TextDocument::apply_changes\n  10: ruff_server::session::index::Index::update_text_document\n  11: <ruff_server::server::api::notifications::did_change::DidChange as ruff_server::server::api::traits::SyncNotificationHandler>::run\n  12: core::ops::function::FnOnce::call_once{{vtable.shim}}\n  13: std::sys::backtrace::__rust_begin_short_backtrace\n  14: core::ops::function::FnOnce::call_once{{vtable.shim}}\n  15: std::sys::pal::unix::thread::Thread::new::thread_start\n  16: <unknown>\n  17: <unknown>\n\npanicked at crates/ruff_server/src/edit/range.rs:68:71:\nbyte index 68 is out of bounds of `"""\n测试comment\n一些测试内容\n"""\nimport click\n\n@click.go\n`\n   0: ruff_server::server::Server::run::{{closure}}\n   1: std::panicking::rust_panic_with_hook\n   2: std::panicking::begin_panic_handler::{{closure}}\n   3: std::sys::backtrace::__rust_end_short_backtrace\n   4: rust_begin_unwind\n   5: core::panicking::panic_fmt\n   6: core::str::slice_error_fail_rt\n   7: core::str::slice_error_fail\n   8: <lsp_types::Range as ruff_server::edit::range::RangeExt>::to_text_range\n   9: ruff_server::edit::text_document::TextDocument::apply_changes\n  10: ruff_server::session::index::Index::update_text_document\n  11: <ruff_server::server::api::notifications::did_change::DidChange as ruff_server::server::api::traits::SyncNotificationHandler>::run\n  12: core::ops::function::FnOnce::call_once{{vtable.shim}}\n  13: std::sys::backtrace::__rust_begin_short_backtrace\n  14: core::ops::function::FnOnce::call_once{{vtable.shim}}\n  15: std::sys::pal::unix::thread::Thread::new::thread_start\n  16: <unknown>\n  17: <unknown>\n\n'
[ERROR][2024-10-29 09:40:00] .../vim/lsp/rpc.lua:770    "rpc"   "/home/husu/.local/share/nvim/mason/bin/ruff"   "stderr"        "  15.384631891s ERROR           main ruff_server::server: panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/jod-thread-0.1.2/src/lib.rs:33:22:\ncalled `Result::unwrap()` on an `Err` value: Any { .. }\n   0: ruff_server::server::Server::run::{{closure}}\n   1: std::panicking::rust_panic_with_hook\n   2: std::panicking::begin_panic_handler::{{closure}}\n   3: std::sys::backtrace::__rust_end_short_backtrace\n   4: rust_begin_unwind\n   5: core::panicking::panic_fmt\n   6: core::result::unwrap_failed\n   7: ruff_server::server::Server::run\n   8: ruff::run\n   9: ruff::main\n  10: std::sys::backtrace::__rust_begin_short_backtrace\n  11: std::rt::lang_start::{{closure}}\n  12: std::rt::lang_start_internal\n  13: main\n  14: <unknown>\n  15: __libc_start_main\n  16: <unknown>\n\npanicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/jod-thread-0.1.2/src/lib.rs:33:22:\ncalled `Result::unwrap()` on an `Err` value: Any { .. }\n   0: ruff_server::server::Server::run::{{closure}}\n   1: std::panicking::rust_panic_with_hook\n   2: std::panicking::begin_panic_handler::{{closure}}\n   3: std::sys::backtrace::__rust_end_short_backtrace\n   4: rust_begin_unwind\n   5: core::panicking::panic_fmt\n   6: core::result::unwrap_failed\n   7: ruff_server::server::Server::run\n   8: ruff::run\n   9: ruff::main\n  10: std::sys::backtrace::__rust_begin_short_backtrace\n  11: std::rt::lang_start::{{closure}}\n  12: std::rt::lang_start_internal\n  13: main\n  14: <unknown>\n  15: __libc_start_main\n  16: <unknown>\n\n"
```

---

_Comment by @MichaReiser on 2024-10-29 07:27_

Thanks for the additional logs. 

Looking at the stack trace, I suspect that the panic happens here because it's the only place where we slice into the string in `to_text_range`:

https://github.com/astral-sh/ruff/blob/b0731ef9cbbfcad82144c2fc33dcbad1cd99de7b/crates/ruff_server/src/edit/range.rs#L64-L70

It would suggest that the line-ranges are incorrect but it's unclear to me how it comes to this. The LSP messages would be helpful for debugging. @dhruvmanila could you share the instructions on how to get the LSP messages in neovim? 

---

_Comment by @dhruvmanila on 2024-10-29 07:33_

> could you share the instructions on how to get the LSP messages in neovim?

Yeah, you'd need to add the following lines in your Neovim config and then restart Neovim:
```lua
vim.lsp.set_log_level("debug")
-- This is mainly to make sure the messages are logged in a pretty way
require('vim.lsp.log').set_format_func(vim.inspect)
```
This will then log the request - response messages between the server and the client.

---

_Comment by @dhruvmanila on 2024-10-29 14:13_

I tried reproducing it using the provided source code and trying to perform the same set of actions as close as possible but unable to get the panic:

https://github.com/user-attachments/assets/c830386c-590f-4ac6-980d-86a664e575f2

I think having detailed logs involving the request - response messages would be very useful.

---

_Comment by @MichaReiser on 2024-10-29 14:16_

> I think having detailed logs involving the request - response messages would be very useful.

What positional encoding does your neovim setup use? Is it UTF8 or UTF16?

---

_Comment by @HusuSama on 2024-10-30 02:26_

[error.webm](https://github.com/user-attachments/assets/23c54370-04d9-46b9-825e-ec2939824174)

This issue seems to occur randomly, and I'm not quite sure if it's related to my nvim configuration or the system.

os:
![图片](https://github.com/user-attachments/assets/1bf7600b-fff4-4cfd-b9b0-a9c5b878a88a)

Most of the time when problems occur, I automatically select the first code suggestion rather than manually choosing the first one.

`nvim-cmp` config:

```lua
return {
  {
    "hrsh7th/nvim-cmp",
    opts = function(_, opts)
      local cmp = require("cmp")
      local utils = require("astrocore")
      local lspkind = require("lspkind")
      opts.sources = cmp.config.sources {
        {
          name = "nvim_lsp",
          priority = 1000,
          entry_filter = function(entry, ctx)
            return require("cmp.types").lsp.CompletionItemKind[entry:get_kind()] ~= "Text"
          end
        },
        { name = "luasnip", priority = 750 },
        { name = "path",    priority = 250 },
        { name = "codeium" }
      }
      return utils.extend_tbl(opts, {
        formatting = {
          -- fields = {"abbr", "kind", "menu"},
          format = lspkind.cmp_format({
            -- mode = "symbol",
            mode = "symbol_text",
            -- mode = "text_symbol",
            minwidth = 60,
            maxwidth = 80,
            ellipsis_char = "..."
          })
        },
        mapping = {
          ["<Up>"] = cmp.mapping.select_prev_item { behavior = cmp.SelectBehavior.Select },
          ["<Down>"] = cmp.mapping.select_next_item { behavior = cmp.SelectBehavior.Select },
          ["<C-p>"] = cmp.mapping.select_prev_item { behavior = cmp.SelectBehavior.Insert },
          ["<C-n>"] = cmp.mapping.select_next_item { behavior = cmp.SelectBehavior.Insert },
          ["<C-k>"] = cmp.mapping.select_prev_item { behavior = cmp.SelectBehavior.Insert },
          ["<C-j>"] = cmp.mapping.select_next_item { behavior = cmp.SelectBehavior.Insert },
          ["<C-u>"] = cmp.mapping(cmp.mapping.scroll_docs(-4), { "i", "c" }),
          ["<C-d>"] = cmp.mapping(cmp.mapping.scroll_docs(4), { "i", "c" }),
          ["<C-Space>"] = cmp.mapping(cmp.mapping.complete(), { "i", "c" }),
          ["<C-y>"] = cmp.config.disable,
          -- ["<C-z>"] = cmp.mapping { i = cmp.mapping.abort(), c = cmp.mapping.close() },
          ["<C-e>"] = cmp.mapping { i = cmp.mapping.abort(), c = cmp.mapping.close() },
          ["<CR>"] = cmp.mapping.confirm { select = false },
          ["<Tab>"] = cmp.mapping.confirm { select = true },
        },
        sorting = {
          comparators = {
            cmp.config.compare.offset,
            cmp.config.compare.exact,
            cmp.config.compare.score,
            cmp.config.compare.recently_used,
            function(entry1, entry2)
              local _, entry1_under = entry1.completion_item.label:find "^_+"
              local _, entry2_under = entry2.completion_item.label:find "^_+"
              entry1_under = entry1_under or 0
              entry2_under = entry2_under or 0
              if entry1_under > entry2_under then
                return false
              elseif entry1_under < entry2_under then
                return true
              end
            end,
            -- 提升关键字的优先级
            function(entry1, entry2)
              local kind1 = entry1.completion_item.kind
              local kind2 = entry2.completion_item.kind
              kind1 = kind1 or 0
              kind2 = kind2 or 0
              if kind1 ~= kind2 then
                if require("cmp.types").lsp.CompletionItemKind[kind1] == "Keyword" then
                  -- if kind1 == 14 then
                  return true
                elseif require("cmp.types").lsp.CompletionItemKind[kind1] == "Snippet" then
                  return false
                else
                  return false
                end
              end
            end,
            cmp.config.compare.kind,
            cmp.config.compare.sort_text,
            cmp.config.compare.length,
            cmp.config.compare.order,
          },
        },
      })
    end
  }
}

```

@dhruvmanila 

---

_Comment by @dhruvmanila on 2024-10-30 03:03_

> What positional encoding does your neovim setup use? Is it UTF8 or UTF16?

UTF-16. I think Neovim's LSP client only support UTF-16 as per their client capabilities.

---

_Comment by @dhruvmanila on 2024-10-30 03:05_

@HusuSama Can you provide detailed logs? I've mentioned the change in your config to enable debug logs at https://github.com/astral-sh/ruff/issues/13674#issuecomment-2443456417.

---

_Comment by @HusuSama on 2024-10-30 05:43_

This data might be quite extensive; is it okay if I send it to you in a file? @dhruvmanila 
[lsp.log](https://github.com/user-attachments/files/17566722/lsp.log)


---

_Comment by @MichaReiser on 2024-10-31 08:25_

Thanks. The logs are very useful. I've been able to reproduce the bug with the following unit test:

```rust
mod tests {
    use crate::edit::DocumentVersion;
    use crate::{PositionEncoding, TextDocument};
    use lsp_types::{Position, TextDocumentContentChangeEvent};

    #[test]
    fn regression() {
        let mut document = TextDocument::new(
            r#""""
测试comment
一些测试内容
"""
import click


@click.group()
def interface():
    pas
"#
            .to_string(),
            0,
        );

        document.apply_changes(
            vec![
                TextDocumentContentChangeEvent {
                    range: Some(lsp_types::Range::new(
                        Position::new(9, 7),
                        Position::new(9, 7),
                    )),
                    range_length: Some(0),
                    text: "s".to_string(),
                },
                TextDocumentContentChangeEvent {
                    range: Some(lsp_types::Range::new(
                        Position::new(9, 7),
                        Position::new(9, 8),
                    )),
                    range_length: Some(1),
                    text: "".to_string(),
                },
                TextDocumentContentChangeEvent {
                    range: Some(lsp_types::Range::new(
                        Position::new(9, 7),
                        Position::new(9, 7),
                    )),
                    range_length: Some(0),
                    text: "s".to_string(),
                },
            ],
            1,
            PositionEncoding::UTF16,
        );

        dbg!(document);
    }
}
```

---

_Label `needs-mre` removed by @MichaReiser on 2024-10-31 08:29_

---

_Label `bug` added by @MichaReiser on 2024-10-31 08:29_

---

_Comment by @HusuSama on 2024-10-31 08:47_

I'm very happy to be able to assist you, and I hope to use the fixed version as soon as possible. 😄

---

_Closed by @MichaReiser on 2024-11-01 07:16_

---
