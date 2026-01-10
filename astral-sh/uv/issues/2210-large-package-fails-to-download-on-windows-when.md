---
number: 2210
title: Large package fails to download on Windows when paging file is small
type: issue
state: open
author: akx
labels:
  - bug
  - windows
assignees: []
created_at: 2024-03-05T18:07:55Z
updated_at: 2024-07-02T09:37:45Z
url: https://github.com/astral-sh/uv/issues/2210
synced_at: 2026-01-10T01:23:14Z
---

# Large package fails to download on Windows when paging file is small

---

_Issue opened by @akx on 2024-03-05 18:07_

... that's a new one! :grin:

I have 13.6 gigabytes of free memory, but my pagefile is set to 4 gigabytes since 32 GB of system memory should well be enough...

The file in question is 2454828770 bytes (2.3G).

```
(bitsandbytes) F:\build\bitsandbytes>ver

Microsoft Windows [Version 10.0.19045.4046]

(bitsandbytes) F:\build\bitsandbytes>uv --version
uv 0.1.14 (c525fdf2b 2024-03-04)

(bitsandbytes) F:\build\bitsandbytes>uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121 --verbose
 uv::requirements::from_source source=torch
 uv::requirements::from_source source=torchvision
 uv::requirements::from_source source=torchaudio
    0.003750s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: F:\venvs\bitsandbytes
    0.004546s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.11.4, skipping probing: \\?\F:\venvs\bitsandbytes\Scripts\python.exe
    0.005073s DEBUG uv::commands::pip_install Using Python 3.11.4 environment at F:\venvs\bitsandbytes\Scripts\python.exe
    0.006479s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.016671s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.4
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.018873s   0ms DEBUG uv_resolver::resolver Adding direct dependency: torch*
        0.019753s   1ms DEBUG uv_resolver::resolver Adding direct dependency: torchvision*
        0.020892s   2ms DEBUG uv_resolver::resolver Adding direct dependency: torchaudio*
   uv_resolver::resolver::choose_version package=torch
     uv_resolver::resolver::package_wait package_name=torch
 uv_resolver::resolver::process_request request=Versions torch
   uv_client::registry_client::simple_api package=torch
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=\\?\C:\Users\X\AppData\Local\uv\cache\simple-v3\105f2b6141aa1e84\torch.rkyv
 uv_resolver::resolver::process_request request=Versions torchvision
   uv_client::registry_client::simple_api package=torchvision
 uv_client::cached_client::from_path_sync path="\\\\?\\C:\\Users\\X\\AppData\\Local\\uv\\cache\\simple-v3\\105f2b6141aa1e84\\torch.rkyv"
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=\\?\C:\Users\X\AppData\Local\uv\cache\simple-v3\105f2b6141aa1e84\torchvision.rkyv
 uv_resolver::resolver::process_request request=Versions torchaudio
   uv_client::registry_client::simple_api package=torchaudio
 uv_client::cached_client::from_path_sync path="\\\\?\\C:\\Users\\X\\AppData\\Local\\uv\\cache\\simple-v3\\105f2b6141aa1e84\\torchvision.rkyv"
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=\\?\C:\Users\X\AppData\Local\uv\cache\simple-v3\105f2b6141aa1e84\torchaudio.rkyv
 uv_resolver::resolver::process_request request=Prefetch torchaudio *
 uv_client::cached_client::from_path_sync path="\\\\?\\C:\\Users\\X\\AppData\\Local\\uv\\cache\\simple-v3\\105f2b6141aa1e84\\torchaudio.rkyv"
 uv_resolver::resolver::process_request request=Prefetch torchvision *
 uv_resolver::resolver::process_request request=Prefetch torch *
          0.036451s  10ms DEBUG uv_client::cached_client No cache entry for: https://download.pytorch.org/whl/cu121/torch/
       uv_client::cached_client::fresh_request url="https://download.pytorch.org/whl/cu121/torch/"
          0.038076s   9ms DEBUG uv_client::cached_client No cache entry for: https://download.pytorch.org/whl/cu121/torchvision/
       uv_client::cached_client::fresh_request url="https://download.pytorch.org/whl/cu121/torchvision/"
          0.040335s   8ms DEBUG uv_client::cached_client No cache entry for: https://download.pytorch.org/whl/cu121/torchaudio/
       uv_client::cached_client::fresh_request url="https://download.pytorch.org/whl/cu121/torchaudio/"
       uv_client::cached_client::new_cache file=\\?\C:\Users\X\AppData\Local\uv\cache\simple-v3\105f2b6141aa1e84\torch.rkyv
       uv_client::registry_client::parse_simple_api package=torch
         uv_client::html::parse url=https://download.pytorch.org/whl/cu121/torch/
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=torch==2.2.1+cu121
     uv_client::registry_client::wheel_metadata built_dist=torch==2.2.1+cu121
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=\\?\C:\Users\X\AppData\Local\uv\cache\wheels-v0\index\105f2b6141aa1e84\torch\torch-2.2.1+cu121-cp311-cp311-win_amd64.msgpack
 uv_client::cached_client::from_path_sync path="\\\\?\\C:\\Users\\X\\AppData\\Local\\uv\\cache\\wheels-v0\\index\\105f2b6141aa1e84\\torch\\torch-2.2.1+cu121-cp311-cp311-win_amd64.msgpack"
        0.338085s 316ms DEBUG uv_resolver::resolver Searching for a compatible version of torch (*)
        0.339033s 317ms DEBUG uv_resolver::resolver Selecting: torch==2.2.1+cu121 (torch-2.2.1+cu121-cp311-cp311-win_amd64.whl)
   uv_resolver::resolver::get_dependencies package=torch, version=2.2.1+cu121
     uv_resolver::resolver::distributions_wait package_id=torch-2.2.1+cu121
              0.341270s   5ms DEBUG uv_client::cached_client No cache entry for: https://download.pytorch.org/whl/cu121/torch-2.2.1%2Bcu121-cp311-cp311-win_amd64.whl
           uv_client::cached_client::fresh_request url="https://download.pytorch.org/whl/cu121/torch-2.2.1%2Bcu121-cp311-cp311-win_amd64.whl"
           uv_client::cached_client::new_cache file=\\?\C:\Users\X\AppData\Local\uv\cache\wheels-v0\index\105f2b6141aa1e84\torch\torch-2.2.1+cu121-cp311-cp311-win_amd64.msgpack
           uv_client::registry_client::read_metadata_range_request wheel=torch-2.2.1+cu121-cp311-cp311-win_amd64.whl
error: Failed to download: torch==2.2.1+cu121
  Caused by: memory mapping the file failed
  Caused by: The paging file is too small for this operation to complete. (os error 1455)

(bitsandbytes) F:\build\bitsandbytes>
```

Manually downloading the same file and installing it works (though 21 seconds to "download" it, huh?)

```
(bitsandbytes) F:\build\bitsandbytes>wget https://download.pytorch.org/whl/cu121/torch-2.2.1%2Bcu121-cp311-cp311-win_amd64.whl
--2024-03-05 20:03:48--  https://download.pytorch.org/whl/cu121/torch-2.2.1%2Bcu121-cp311-cp311-win_amd64.whl
Resolving download.pytorch.org (download.pytorch.org)... 13.33.243.88, 13.33.243.23, 13.33.243.91, ...
Connecting to download.pytorch.org (download.pytorch.org)|13.33.243.88|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2454828770 (2.3G) [binary/octet-stream]
Saving to: 'torch-2.2.1+cu121-cp311-cp311-win_amd64.whl'

torch-2.2.1+cu121-cp311-cp311-win_amd64.whl   100%[================================================================================================>]   2.29G  40.7MB/s    in 60s

(bitsandbytes) F:\build\bitsandbytes>uv pip install torch@torch-2.2.1+cu121-cp311-cp311-win_amd64.whl
Resolved 9 packages in 19ms
Downloaded 1 package in 21.02s
Installed 1 package in 12.38s
 + torch==2.2.1+cu121 (from file:///F:/build/bitsandbytes/torch-2.2.1+cu121-cp311-cp311-win_amd64.whl)
```



---

_Label `bug` added by @zanieb on 2024-03-05 18:14_

---

_Label `windows` added by @konstin on 2024-07-02 09:37_

---
