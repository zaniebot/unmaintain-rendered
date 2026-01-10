```yaml
number: 4722
title: "feat: re-enable std in uv-trampoline"
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: trampoline-std
created_at: 2024-07-02T04:44:18Z
updated_at: 2024-07-07T19:29:58Z
url: https://github.com/astral-sh/uv/pull/4722
synced_at: 2026-01-10T13:48:28Z
```

# feat: re-enable std in uv-trampoline

---

_Pull request opened by @samypr100 on 2024-07-02 04:44_

## Summary

Partially closes #1917

This PR picks up on some of the great work from #1864 and opted to keep `panic_immediate_abort` (for size reasons). I split the PR in different isolated commits in case we want to separate/cherry-pick them out.

1. The first commit ports mostly all std changes from that PR into this PR. Binary sizes stayed the same ~16kb.
2. The second commit migrates our existing usage of windows-sys to windows for a safer ffi calls with Results!. It also changes all large unsafe blocks to be isolated to the actual unsafe calls, and switches some areas to use std such as getenv port ( which seemed buggy! ) from launcher.c. In addition, this also adds more error checking in order to match some missing assertions from distlib's launcher.c. Note, due to the additional .text data, the binary sizes increased to ~20.5kb, but we can cut back on some of the added error msgs as needed.
3. The third commit switches to using xwin for building on all 3 supported trampoline targets for sanity, and adds a CI bloat check for core::fmt and panic as a precaution. Sadly, this will invalidate the xwin cache on the first run.

## Test Plan

Most changes were tested on a couple of local GUI apps and console apps, also tested some of the error states manually by using SetLastError at different points in the code and/or passing in invalid handles.

I'm not sure how far we can get with migrating some of the other calls without increasing binary size substantially. An initial attempt at using std::path didn't seem so bad size wise when I tried it (~1k). On other cases, such as std::process::exit added ~10k to the total binary size. 

---

_Review requested from @konstin by @zanieb on 2024-07-02 20:19_

---

_Review requested from @BurntSushi by @zanieb on 2024-07-02 20:19_

---

_Comment by @samypr100 on 2024-07-03 04:09_

Below are some extra logging I added that is not part of distlib's launcher.c which we could arguably remove, although all these errors (if they occur) can diminish the stability of the console/gui script.

<details><summary>Patch File Diff (in case we want to remove the added logging)</summary>
<p>

```patch
@@ -400,9 +400,7 @@
         print_last_error_and_exit("Failed to spawn the python child process.");
     });
     let child_process_info = unsafe { child_process_info.assume_init() };
-    unsafe { CloseHandle(child_process_info.hThread) }.unwrap_or_else(|_| {
-        print_last_error_and_exit("Failed to close handle to python child process main thread.");
-    });
+    let _ = unsafe { CloseHandle(child_process_info.hThread) };
     // Return handle to child process.
     child_process_info.hProcess
 }
@@ -415,12 +413,8 @@
     // See distlib/PC/launcher.c::cleanup_standard_io()
     for std_handle in [STD_INPUT_HANDLE, STD_OUTPUT_HANDLE, STD_ERROR_HANDLE] {
         if let Ok(handle) = unsafe { GetStdHandle(std_handle) } {
-            unsafe { CloseHandle(handle) }.unwrap_or_else(|_| {
-                eprintln!("Failed to close standard device handle {}.", handle.0);
-            });
-            unsafe { SetStdHandle(std_handle, INVALID_HANDLE_VALUE) }.unwrap_or_else(|_| {
-                eprintln!("Failed to modify standard device handle {}.", std_handle.0);
-            });
+            let _ = unsafe { CloseHandle(handle) };
+            let _ = unsafe { SetStdHandle(std_handle, INVALID_HANDLE_VALUE) };
         }
     }
 
@@ -434,9 +428,7 @@
     for i in 0..handle_count {
         let handle_ptr = unsafe { handle_start.offset(i).read_unaligned() } as *const HANDLE;
         // Close all fds inherited from the parent, except for the standard I/O fds.
-        unsafe { CloseHandle(*handle_ptr) }.unwrap_or_else(|_| {
-            eprintln!("Failed to close child file descriptors at {}.", i);
-        });
+        let _ = unsafe { CloseHandle(*handle_ptr) };
     }
 }
 
@@ -463,16 +455,10 @@
     let mut msg = MaybeUninit::<MSG>::uninit();
     unsafe {
         // End the launcher's "app starting" cursor state.
-        PostMessageA(None, 0, None, None).unwrap_or_else(|_| {
-            eprintln!("Failed to post a message to specified window.");
-        });
-        if GetMessageA(msg.as_mut_ptr(), None, 0, 0) != TRUE {
-            eprintln!("Failed to retrieve posted window message.");
-        }
+        let _ = PostMessageA(None, 0, None, None);
+        let _ = GetMessageA(msg.as_mut_ptr(), None, 0, 0);
         // Proxy the child's input idle event.
-        if WaitForInputIdle(child_handle, INFINITE) != 0 {
-            eprintln!("Failed to wait for input from window.");
-        }
+        let _ = WaitForInputIdle(child_handle, INFINITE);
         // Signal the process input idle event by creating a window and pumping
         // sent messages. The window class isn't important, so just use the
         // system "STATIC" class.
@@ -491,10 +477,8 @@
             None,
         );
         // Process all sent messages and signal input idle.
-        _ = PeekMessageA(msg.as_mut_ptr(), hwnd, 0, 0, PEEK_MESSAGE_REMOVE_TYPE(0));
-        DestroyWindow(hwnd).unwrap_or_else(|_| {
-            print_last_error_and_exit("Failed to destroy temporary window.");
-        });
+        let _ = PeekMessageA(msg.as_mut_ptr(), hwnd, 0, 0, PEEK_MESSAGE_REMOVE_TYPE(0));
+        let _ = DestroyWindow(hwnd);
     }
 }
 
@@ -517,9 +501,7 @@
 
     // (best effort) Switch to some innocuous directory, so we don't hold the original cwd open.
     // See distlib/PC/launcher.c::switch_working_directory
-    if std::env::set_current_dir(std::env::temp_dir()).is_err() {
-        eprintln!("Failed to set cwd to temp dir.");
-    }
+    let _ = std::env::set_current_dir(std::env::temp_dir());
 
     // We want to ignore control-C/control-Break/logout/etc.; the same event will
     // be delivered to the child, so we let them decide whether to exit or not.
@@ -535,7 +517,7 @@
         clear_app_starting_state(child_handle);
     }
 
-    _ = unsafe { WaitForSingleObject(child_handle, INFINITE) };
+    let _ = unsafe { WaitForSingleObject(child_handle, INFINITE) };
     let mut exit_code = 0u32;
     if unsafe { GetExitCodeProcess(child_handle, &mut exit_code) }.is_err() {
         print_last_error_and_exit("Failed to get exit code of child process.");
```

</p>
</details> 

---

_@samypr100 reviewed on 2024-07-03 04:13_

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:508 on 2024-07-03 04:13_

Note, the `c:\\` approach was removed as it's not part of [launcher.c](https://github.com/pypa/distlib/blob/master/PC/launcher.c#L736), plus the sysroot drive could vary on a system.

getenv(c"TEMP") was changed to use std instead as it had bugs when parsing the value of TEMP on my system probably due to W vs A calls but didn't dig too much, as a result `getenv` was deleted.

---

_Review comment by @konstin on `crates/uv-trampoline/Cargo.toml`:19 on 2024-07-03 07:36_

Nit: Can you remove the block alignment of the comments? Either move the to the previous line or one space after the item they are commenting on

---

_@konstin approved on 2024-07-03 07:43_

---

_Comment by @konstin on 2024-07-03 07:45_

> Leaving in draft for now to see if we want to invest more time on this.

This looks good to merge to me

---

_Comment by @konstin on 2024-07-03 07:45_

> I didn't commit the binaries, do we manually commit those on the PR?

A member of the team will make a follow-up where the build the binaries and commit them

---

_Comment by @samypr100 on 2024-07-03 12:18_

> A member of the team will make a follow-up where the build the binaries and commit them

Sounds good, I ended up commiting them to ensure tests pass, but from a security angle it makes sense someone from Astral builds them.

---

_@samypr100 reviewed on 2024-07-03 12:22_

---

_Review comment by @samypr100 on `crates/uv-trampoline/Cargo.toml`:19 on 2024-07-03 12:22_

Changed in 528593f4ea1b811b8c19a5dc6aaeb3701785b6d5

---

_Marked ready for review by @samypr100 on 2024-07-03 14:47_

---

_@konstin approved on 2024-07-06 20:30_

Thanks!

---

_Merged by @konstin on 2024-07-06 20:38_

---

_Closed by @konstin on 2024-07-06 20:38_

---

_Comment by @zanieb on 2024-07-07 16:52_

Awesome!

---

_Branch deleted on 2024-07-07 19:29_

---
