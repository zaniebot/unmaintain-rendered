```yaml
number: 986
title: rg miss some file in windows 
type: issue
state: closed
author: fcying
labels:
  - bug
assignees: []
created_at: 2018-07-20T03:12:39Z
updated_at: 2018-07-22T06:14:25Z
url: https://github.com/BurntSushi/ripgrep/issues/986
synced_at: 2026-01-12T16:13:22Z
```

# rg miss some file in windows 

---

_@fcying_

#### What version of ripgrep are you using?
ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX

#### How did you install ripgrep?
Github binary releases `ripgrep-0.8.1-x86_64-pc-windows-msvc.zip`


#### What operating system are you using ripgrep on?
windows 10

#### Describe your question, feature request, or bug.
in the test file, rg can't search all result in win10(sometimes work fine), but it work fine in linux.

#### If this is a bug, what are the steps to reproduce the behavior?
```
cd test_rg
rg -i test_rg
rg -i test_rg
```
![image](https://user-images.githubusercontent.com/106934/42981188-892496a6-8c0d-11e8-8142-301b97a570d5.png)


#### If this is a bug, what is the actual behavior?

Show the command you ran and the actual output. Include the `--debug` flag in
```
 rg -i test_rg --debug
DEBUG/grep::search/grep\src\search.rs:195: regex ast:
Literal {
    chars: [
        't',
        'e',
        's',
        't',
        '_',
        'r',
        'g'
    ],
    casei: true
}
DEBUG/grep::literals/grep\src\literals.rs:79: required literals found: [Cut(TEST_), Cut(tEST_), Cut(TeST_), Cut(teST_), Cut(TEsT_), Cut(tEsT_), Cut(TesT_), Cut(tesT_), Cut(TEſT_), Cut(tEſT_), Cut(TeſT_), Cut(teſT_), Cut(TESt_), Cut(tESt_), Cut(TeSt_), Cut(teSt_), Cut(TEst_), Cut(tEst_), Cut(Test_), Cut(test_), Cut(TEſt_), Cut(tEſt_), Cut(Teſt_), Cut(teſt_)]
```

#### If this is a bug, what is the expected behavior?
search correct

[test_rg.zip](https://github.com/BurntSushi/ripgrep/files/2212316/test_rg.zip)


---

_Comment by @BurntSushi on 2018-07-21 17:11_

Thanks for the bug report! This actually appears to have been fixed _accidentally_ in d5139228.

The underlying issue here is that when using the Windows console (which is a critical piece of information you left out in your bug report), it will completely choke if you feed it arbitrary binary data. In particular, your `test.txt` file contains what appears to be latin-1 bytes:

```
test_rg\xb7\xc5\r\n
```

where `\xB7\xC5` is not valid UTF-8. ripgrep passes this through to the console as-is, which Windows then chokes on. Now, ripgrep does actually account for this and will automatically replace invalid UTF-8 sequences in its output with the Unicode replacement character, which makes Windows happy. The issue here is that you were hitting a particular code path where this lossy transcoding wasn't getting trigger.

The reason why this appeared non-deterministic to you is because ripgrep dies (apparently there is no error from Windows?) as soon as it tries to write invalid UTF-8, which may or may not come before your `aaa/aaa.txt` file, depending on the order of directory traversal, which is itself non-deterministic.

Commit d5139228 seems to have straightened out the use of the lossy transcoding, so your example works fine on master.

For a more immediate work-around, if you use `-j1` to ripgrep, then it should always perform lossy transcoding (although it will limit execution to a single thread).

---

_Closed by @BurntSushi on 2018-07-21 17:11_

---

_Label `bug` added by @BurntSushi on 2018-07-21 17:11_

---

_Comment by @fcying on 2018-07-22 04:57_

I build the last version, it work fine, thanks a lot.

BTW: 
Why I can't build a msvc version, build gnu version ok.
```
cargo 1.27.0 (1e95190e5 2018-05-27)
d:\tool\source\ripgrep>cargo build --release --verbose
   Compiling winapi v0.3.5
       Fresh libc v0.2.42
       Fresh void v1.0.2
       Fresh ucd-util v0.1.1
     Running `rustc --crate-name build_script_build d:\workspace\cargo\registry\src\github.com-1ecc6299db9ec823\winapi-0.3.5\build.rs --crate-type bin --emit=dep-info,link -C opt-level=3 -C debuginfo=2 --cfg "feature=\"basetsd\"" --cfg "feature=\"consoleapi\"" --cfg "feature=\"errhandlingapi\"" --cfg "feature=\"fileapi\"" --cfg "feature=\"handleapi\"" --cfg "feature=\"memoryapi\"" --cfg "feature=\"minwinbase\"" --cfg "feature=\"minwindef\"" --cfg "feature=\"processenv\"" --cfg "feature=\"std\"" --cfg "feature=\"sysinfoapi\"" --cfg "feature=\"winbase\"" --cfg "feature=\"wincon\"" --cfg "feature=\"winnt\"" -C metadata=22a1e8202eb73949 -C extra-filename=-22a1e8202eb73949 --out-dir d:\tool\source\ripgrep\target\release\build\winapi-22a1e8202eb73949 -L dependency=d:\tool\source\ripgrep\target\release\deps --cap-lints allow`
   Compiling regex v1.0.2
       Fresh unicode-width v0.1.5
       Fresh lazy_static v1.0.2
     Running `rustc --crate-name build_script_build d:\workspace\cargo\registry\src\github.com-1ecc6299db9ec823\regex-1.0.2\build.rs --crate-type bin --emit=dep-info,link -C opt-level=3 -C debuginfo=2 --cfg "feature=\"default\"" --cfg "feature=\"use_std\"" -C metadata=eebceb7273847fb6 -C extra-filename=-eebceb7273847fb6 --out-dir d:\tool\source\ripgrep\target\release\build\regex-eebceb7273847fb6 -L dependency=d:\tool\source\ripgrep\target\release\deps --cap-lints allow`
       Fresh utf8-ranges v1.0.0
       Fresh bitflags v1.0.3
       Fresh strsim v0.7.0
       Fresh cfg-if v0.1.4
       Fresh fnv v1.0.6
       Fresh crossbeam v0.3.2
       Fresh bytecount v0.3.1
       Fresh memchr v2.0.1
       Fresh num_cpus v1.8.0
       Fresh unreachable v1.0.0
       Fresh regex-syntax v0.6.2
       Fresh textwrap v0.10.0
       Fresh log v0.4.3
       Fresh encoding_rs v0.8.4
       Fresh aho-corasick v0.6.6
       Fresh thread_local v0.3.5
       Fresh encoding_rs_io v0.1.0
error: linking with `C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\VC\Tools\MSVC\14.14.26428\bin\HostX86\x86\link.exe` failed: exit code: 1181
  |
  = note: "C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\BuildTools\\VC\\Tools\\MSVC\\14.14.26428\\bin\\HostX86\\x86\\link.exe" "/NOLOGO" "/NXCOMPAT" "/LIBPATH:D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build0-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build1-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build10-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build11-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build12-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build13-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build14-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build15-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build2-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build3-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build4-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build5-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build6-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build7-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build8-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.build_script_build9-c9f0661f2293fff6a17a81700813ad07.rs.rcgu.o" "/OUT:d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.exe" "d:\\tool\\source\\ripgrep\\target\\release\\build\\regex-eebceb7273847fb6\\build_script_build-eebceb7273847fb6.crate.allocator.rcgu.o" "/OPT:REF,ICF" "/DEBUG" "/NATVIS:D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\etc\\intrinsic.natvis" "/NATVIS:D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\etc\\liballoc.natvis" "/NATVIS:D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\etc\\libcore.natvis" "/LIBPATH:d:\\tool\\source\\ripgrep\\target\\release\\deps" "/LIBPATH:D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\libstd-76a5f3e41ba6c505.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\libpanic_unwind-34df5d829dd859de.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\libunwind-59e47e91e69cda4a.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\liblibc-8cda3d10ee6838de.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\liballoc_system-43af5dd0fb31ce12.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\liballoc-d398901ac58ea1e0.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\libcore-d27467c8bff46509.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\libcompiler_builtins-bd7b37332e08a54d.rlib" "advapi32.lib" "ws2_32.lib" "userenv.lib" "shell32.lib" "msvcrt.lib"
  = note: Non-UTF-8 output: LINK : fatal error LNK1181: \xce\xde\xb7\xa8\xb4\xf2\xbf\xaa\xca\xe4\xc8\xeb\xce\xc4\xbc\xfe\xa1\xb0advapi32.lib\xa1\xb1\r\n

error: aborting due to previous error

error: Could not compile `regex`.

Caused by:
  process didn't exit successfully: `rustc --crate-name build_script_build d:\workspace\cargo\registry\src\github.com-1ecc6299db9ec823\regex-1.0.2\build.rs --crate-type bin --emit=dep-info,link -C opt-level=3 -C debuginfo=2 --cfg feature="default" --cfg feature="use_std" -C metadata=eebceb7273847fb6 -C extra-filename=-eebceb7273847fb6 --out-dir d:\tool\source\ripgrep\target\release\build\regex-eebceb7273847fb6 -L dependency=d:\tool\source\ripgrep\target\release\deps --cap-lints allow` (exit code: 101)
warning: build failed, waiting for other jobs to finish...
error: linking with `C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\VC\Tools\MSVC\14.14.26428\bin\HostX86\x86\link.exe` failed: exit code: 1181
  |
  = note: "C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\BuildTools\\VC\\Tools\\MSVC\\14.14.26428\\bin\\HostX86\\x86\\link.exe" "/NOLOGO" "/NXCOMPAT" "/LIBPATH:D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build0-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build1-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build10-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build11-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build12-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build13-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build14-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build15-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build2-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build3-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build4-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build5-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build6-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build7-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build8-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.build_script_build9-5331de561096bfdc12b4afaa7b30e3f1.rs.rcgu.o" "/OUT:d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.exe" "d:\\tool\\source\\ripgrep\\target\\release\\build\\winapi-22a1e8202eb73949\\build_script_build-22a1e8202eb73949.crate.allocator.rcgu.o" "/OPT:REF,ICF" "/DEBUG" "/NATVIS:D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\etc\\intrinsic.natvis" "/NATVIS:D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\etc\\liballoc.natvis" "/NATVIS:D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\etc\\libcore.natvis" "/LIBPATH:d:\\tool\\source\\ripgrep\\target\\release\\deps" "/LIBPATH:D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\libstd-76a5f3e41ba6c505.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\libpanic_unwind-34df5d829dd859de.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\libunwind-59e47e91e69cda4a.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\liblibc-8cda3d10ee6838de.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\liballoc_system-43af5dd0fb31ce12.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\liballoc-d398901ac58ea1e0.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\libcore-d27467c8bff46509.rlib" "D:\\jason\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\lib\\rustlib\\x86_64-pc-windows-msvc\\lib\\libcompiler_builtins-bd7b37332e08a54d.rlib" "advapi32.lib" "ws2_32.lib" "userenv.lib" "shell32.lib" "msvcrt.lib"
  = note: Non-UTF-8 output: LINK : fatal error LNK1181: \xce\xde\xb7\xa8\xb4\xf2\xbf\xaa\xca\xe4\xc8\xeb\xce\xc4\xbc\xfe\xa1\xb0advapi32.lib\xa1\xb1\r\n

error: aborting due to previous error

error: Could not compile `winapi`.

Caused by:
  process didn't exit successfully: `rustc --crate-name build_script_build d:\workspace\cargo\registry\src\github.com-1ecc6299db9ec823\winapi-0.3.5\build.rs --crate-type bin --emit=dep-info,link -C opt-level=3 -C debuginfo=2 --cfg feature="basetsd" --cfg feature="consoleapi" --cfg feature="errhandlingapi" --cfg feature="fileapi" --cfg feature="handleapi" --cfg feature="memoryapi" --cfg feature="minwinbase" --cfg feature="minwindef" --cfg feature="processenv" --cfg feature="std" --cfg feature="sysinfoapi" --cfg feature="winbase" --cfg feature="wincon" --cfg feature="winnt" -C metadata=22a1e8202eb73949 -C extra-filename=-22a1e8202eb73949 --out-dir d:\tool\source\ripgrep\target\release\build\winapi-22a1e8202eb73949 -L dependency=d:\tool\source\ripgrep\target\release\deps --cap-lints allow` (exit code: 101)
```

---

_Comment by @fcying on 2018-07-22 06:14_

My fault, after install win10 sdk, it can be build with msvc

---
