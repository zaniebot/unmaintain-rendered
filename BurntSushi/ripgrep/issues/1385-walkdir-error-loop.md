```yaml
number: 1385
title: WalkDir error loop
type: issue
state: closed
author: forensicmatt
labels: []
assignees: []
created_at: 2019-09-23T17:58:38Z
updated_at: 2019-09-23T19:59:39Z
url: https://github.com/BurntSushi/ripgrep/issues/1385
synced_at: 2026-01-12T16:13:23Z
```

# WalkDir error loop

---

_@forensicmatt_

I have found a scenario where both WalkBuilder and WalkParallel get stuck on an error message and fall into an infinite loop, but using the std::fs::read_dir is able to move past the error:

Here is example code I am using for WalkBuilder.
```rust
let builder = WalkBuilder::new( 
    &source_location
)   .follow_links(true)
    .hidden(false)
    .ignore(false)
    .parents(false)
    .git_ignore(false)
    .git_exclude(false)
    .build();

for entry_result in builder {
    let dir_entry = match entry_result {
        Ok(entry) => entry,
        Err(e) => {
            eprintln!("{:?}", e);
            continue;
        }
    };

    match dir_entry.file_type() {
        Some(type_info) => {
            if !type_info.is_file() {
                continue;
            }
        },
        None => {continue}
    };

    let file_path_str = match dir_entry.path().to_str(){
        Some(s) => s,
        None => {continue}
    };

    println!("{}", file_path_str);
}
```

When I run a tool that uses the example snippit it gets stuck in an infinite loop:
```
> .\target\release\examples\iter_dir_ignore_single.exe -s D:\Windows\System32\winevt\Logs
D:\Windows\System32\winevt\Logs\Application.evtx
...
D:\Windows\System32\winevt\Logs\Microsoft-Windows-WindowsUpdateClient%4Operational.evtx
D:\Windows\System32\winevt\Logs\Microsoft-Windows-WinINet-Config%4ProxyConfigChanged.evtx
Io(Custom { kind: Other, error: Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } } })
Io(Custom { kind: Other, error: Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } } })
Io(Custom { kind: Other, error: Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } } })
Io(Custom { kind: Other, error: Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } } })
Io(Custom { kind: Other, error: Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } } })
Io(Custom { kind: Other, error: Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } } })
Io(Custom { kind: Other, error: Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } } })
...
```

However, if I use the std::fs::read_dir, I still get the error, but it does not get stuck in an infinite loop. I know its very difficult to pin point without having a way to reproduce. **Any idea where this could be happening within *ignore*?**

Here is an example function I am using to iterate files with fs::read_dir:
```rust
fn handle_directory(directory_path: &str) {
    for dir_reader in fs::read_dir(directory_path) {
        for entry_result in dir_reader {
            match entry_result {
                Ok(entry) => {
                    let path = entry.path();
                    if path.is_file() {
                        let path_string = path.into_os_string().into_string().unwrap();
                        println!("{}", &path_string);
                    } else if path.is_dir(){
                        let path_string = path.into_os_string().into_string().unwrap();
                        handle_directory(
                            &path_string
                        );
                    }
                },
                Err(error) => {
                    eprintln!("Error reading {} [{:?}]", directory_path, error);
                    break;
                }
            }
        }
    }
}
```

With the resulting error being:
```
> .\target\release\examples\iter_dir_std.exe -s D:\Windows\System32\winevt\Logs
D:\Windows\System32\winevt\Logs\Application.evtx
...
D:\Windows\System32\winevt\Logs\Microsoft-Windows-WindowsUpdateClient%4Operational.evtx
D:\Windows\System32\winevt\Logs\Microsoft-Windows-WinINet-Config%4ProxyConfigChanged.evtx
Error reading D:\Windows\System32\winevt\Logs [Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." }]
```


---

_Renamed from "WalkBuilder error loop" to "WalkDir error loop" by @forensicmatt on 2019-09-23 19:45_

---

_Comment by @forensicmatt on 2019-09-23 19:47_

This appears to be an issue with WalkDir. The following example infinitely loops on me as well.

```rust
let walk_iter = WalkDir::new(source_location).into_iter();

for entry_result in walk_iter {
    let entry = match entry_result {
        Ok(entry) => entry,
        Err(e) => {
            eprintln!("{:?}", e);
            continue;
        }
    };

    println!("{}", entry.path().display());
}
```

```
> .\target\release\examples\iter_dir_walkdir.exe -s D:\Windows\System32\winevt\Logs
D:\Windows\System32\winevt\Logs
D:\Windows\System32\winevt\Logs\Application.evtx
D:\Windows\System32\winevt\Logs\HardwareEvents.evtx
D:\Windows\System32\winevt\Logs\Microsoft-Windows-WindowsUpdateClient%4Operational.evtx
D:\Windows\System32\winevt\Logs\Microsoft-Windows-WinINet-Config%4ProxyConfigChanged.evtx
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
Error { depth: 1, inner: Io { path: None, err: Os { code: 1392, kind: Other, message: "The file or directory is corrupted and unreadable." } } }
...
```

---

_Comment by @BurntSushi on 2019-09-23 19:54_

walkdir and ignore correspond to two distinct recursive directory traversals, so they could both be suffering from the same bug.

Indeed, this is hard to do anything about without a reproduction. Perhaps you could investigate what's wrong with the directory and/or debug it from inside of walkdir.

---

_Comment by @forensicmatt on 2019-09-23 19:59_

Moving this to https://github.com/BurntSushi/walkdir/issues/128.

---

_Closed by @forensicmatt on 2019-09-23 19:59_

---
