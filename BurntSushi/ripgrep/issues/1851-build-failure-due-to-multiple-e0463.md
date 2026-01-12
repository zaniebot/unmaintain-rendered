```yaml
number: 1851
title: Build failure due to multiple E0463
type: issue
state: closed
author: hsmyers
labels:
  - invalid
assignees: []
created_at: 2021-04-16T16:36:07Z
updated_at: 2021-04-16T17:52:23Z
url: https://github.com/BurntSushi/ripgrep/issues/1851
synced_at: 2026-01-12T16:13:24Z
```

# Build failure due to multiple E0463

---

_@hsmyers_

#### What operating system are you using ripgrep on?

Microsoft Windows [Version 10.0.21359.1]

#### Describe your bug.

Both the Git based install and the cargo-based method fail because of multiple E0463 errors.

```
hsmyers@LENNY C:\Users\hsmyers                                                                                                              
# cargo install ripgrep                                                                                                                     
    Updating crates.io index                                                                                                                
  Downloaded ripgrep v12.1.1                                                                                                                
  Downloaded 1 crate (256.5 KB) in 0.66s                                                                                                    
  Installing ripgrep v12.1.1                                                                                                                
  Downloaded base64 v0.12.3                                                                                                                 
  Downloaded byteorder v1.4.3                                                                                                               
  Downloaded bstr v0.2.15                                                                                                                   
  Downloaded encoding_rs v0.8.28                                                                                                            
  Downloaded proc-macro2 v1.0.26                                                                                                            
  Downloaded walkdir v2.3.2                                                                                                                 
  Downloaded serde_json v1.0.64                                                                                                             
  Downloaded ignore v0.4.17                                                                                                                 
  Downloaded memmap v0.7.0                                                                                                                  
  Downloaded regex v1.4.5                                                                                                                   
  Downloaded grep-matcher v0.1.4                                                                                                            
  Downloaded serde_derive v1.0.125                                                                                                          
  Downloaded serde v1.0.125                                                                                                                 
  Downloaded grep-cli v0.1.5                                                                                                                
  Downloaded grep-searcher v0.1.7                                                                                                           
  Downloaded log v0.4.14                                                                                                                    
  Downloaded once_cell v1.7.2                                                                                                               
  Downloaded syn v1.0.69                                                                                                                    
  Downloaded quote v1.0.9                                                                                                                   
  Downloaded thread_local v1.1.3                                                                                                            
  Downloaded crossbeam-utils v0.8.3                                                                                                         
  Downloaded globset v0.4.6                                                                                                                 
  Downloaded grep-printer v0.1.5                                                                                                            
  Downloaded grep-regex v0.1.8                                                                                                              
  Downloaded libc v0.2.93                                                                                                                   
  Downloaded grep v0.2.7                                                                                                                    
  Downloaded 26 crates (3.5 MB) in 3.17s (largest was `encoding_rs` at 1.4 MB)                                                              
   Compiling winapi-x86_64-pc-windows-gnu v0.4.0                                                                                            
   Compiling memchr v2.3.4                                                                                                                  
   Compiling winapi v0.3.9                                                                                                                  
   Compiling cfg-if v1.0.0                                                                                                                  
error[E0463]: can't find crate for `std`                                                                                                    
                                                                                                                                            
error: aborting due to previous error                                                                                                       
                                                                                                                                            
For more information about this error, try `rustc --explain E0463`.                                                                         
error: could not compile `winapi-x86_64-pc-windows-gnu`                                                                                     
                                                                                                                                            
To learn more, run the command again with --verbose.                                                                                        
warning: build failed, waiting for other jobs to finish...                                                                                  
error[E0463]: can't find crate for `std`                                                                                                    
                                                                                                                                            
error: aborting due to previous error                                                                                                       
                                                                                                                                            
For more information about this error, try `rustc --explain E0463`.                                                                         
error[E0463]: can't find crate for `std`                                                                                                    
                                                                                                                                            
error: aborting due to previous error                                                                                                       
                                                                                                                                            
For more information about this error, try `rustc --explain E0463`.                                                                         
error[E0463]: can't find crate for `core`                                                                                                   
                                                                                                                                            
error: aborting due to previous error                                                                                                       
                                                                                                                                            
For more information about this error, try `rustc --explain E0463`.                                                                         
error: failed to compile `ripgrep v12.1.1`, intermediate artifacts can be found at `C:\Users\hsmyers\AppData\Local\Temp\cargo-installBJWHQJ`
                                                                                                                                            
Caused by:                                                                                                                                  
  build failed                                                                                                                              
```

What do you think ripgrep should have done?
Should have worked. 

---

_Comment by @BurntSushi on 2021-04-16 16:37_

It looks like something is wrong with your Rust installation to me. I don't know what it is. I've never seen that error before, and I'm not a Windows user. This definitely isn't a problem with ripgrep.

Please also make sure you're using the latest version of Rust stable.

---

_Closed by @BurntSushi on 2021-04-16 16:37_

---

_Label `invalid` added by @BurntSushi on 2021-04-16 16:37_

---

_Comment by @BurntSushi on 2021-04-16 16:39_

A simple Google search seems to confirm that your installation is FUBAR:

* https://stackoverflow.com/questions/38041331/rust-compiler-cant-find-crate-for-std
* https://stackoverflow.com/questions/24417183/cant-find-crate-for-std-compiler-error-with-trivial-code
* https://github.com/rust-lang/cargo/issues/9149

---

_Comment by @hsmyers on 2021-04-16 16:51_

First I did a choco install rust which executed without error. then I did
the following:

mkdir rust
cd rust
git clone git://github.com/BurntSushi/ripgrep
cd ripgrep
cargo build --release

which failed as previously shown in the second attempt to build.

So this problem is because of choco's error-free install?

On Fri, Apr 16, 2021 at 10:40 AM Andrew Gallant ***@***.***>
wrote:

> A simple Google search seems to confirm that your installation is FUBAR:
>
>    -
>    https://stackoverflow.com/questions/38041331/rust-compiler-cant-find-crate-for-std
>    -
>    https://stackoverflow.com/questions/24417183/cant-find-crate-for-std-compiler-error-with-trivial-code
>    - rust-lang/cargo#9149 <https://github.com/rust-lang/cargo/issues/9149>
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1851#issuecomment-821300095>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAHE7PQVHYH4XLC46DJIB3DTJBR6DANCNFSM43B3ILHA>
> .
>


---

_Comment by @BurntSushi on 2021-04-16 16:55_

I don't know. Try compiling some other Rust projects. If you get the same error, then it's probably an installation problem that you should report to whoever packages Rust for Choco. You might also consider using rustup instead.

---

_Comment by @hsmyers on 2021-04-16 17:08_

Just did a choco uninstall rust followed by a rustup-init.exe
<https://win.rustup.rs/x86_64> and will try again. I'll let you know...

On Fri, Apr 16, 2021 at 10:55 AM Andrew Gallant ***@***.***>
wrote:

> I don't know. Try compiling some other Rust projects. If you get the same
> error, then it's probably an installation problem that you should report to
> whoever packages Rust for Choco. You might also consider using rustup
> instead.
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1851#issuecomment-821309187>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAHE7PXKDSYQ4U2Y3CZSMATTJBTXZANCNFSM43B3ILHA>
> .
>


---

_Comment by @hsmyers on 2021-04-16 17:33_

C:\Users\hsmyers\rust\ripgrep>target\release\rg --version
ripgrep 12.1.1 (rev 92286ad4d2)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

This leads me to believe that the problem with the choco install was no
toolchain selected? But rustup makes the connection with Visual C++ and all
works as intended. You can point to this message chain as a response to
similar issues. Thanks for your time and the info. Take care.

On Fri, Apr 16, 2021 at 11:08 AM Hugh S. Myers ***@***.***> wrote:

> Just did a choco uninstall rust followed by a rustup-init.exe
> <https://win.rustup.rs/x86_64> and will try again. I'll let you know...
>
> On Fri, Apr 16, 2021 at 10:55 AM Andrew Gallant ***@***.***>
> wrote:
>
>> I don't know. Try compiling some other Rust projects. If you get the same
>> error, then it's probably an installation problem that you should report to
>> whoever packages Rust for Choco. You might also consider using rustup
>> instead.
>>
>> —
>> You are receiving this because you authored the thread.
>> Reply to this email directly, view it on GitHub
>> <https://github.com/BurntSushi/ripgrep/issues/1851#issuecomment-821309187>,
>> or unsubscribe
>> <https://github.com/notifications/unsubscribe-auth/AAHE7PXKDSYQ4U2Y3CZSMATTJBTXZANCNFSM43B3ILHA>
>> .
>>
>


---

_Comment by @BurntSushi on 2021-04-16 17:36_

Glad you got it figured out! Thanks for responding. Might be good to file a bug with Choco if you can. They might want to know that their installation is borked.

With that said, generally most folks just use rustup directly to build end user applications like ripgrep. Compilers maintained by distros like Choco are typically used to build Rust software shipped by that distro.

---

_Comment by @hsmyers on 2021-04-16 17:52_

I'll look into it. I've mostly had luck with other problems that I've
discovered. I don't yell or scream and that seems to work well when
following up on questions and such. Of course, it helps that I've been
doing this since 1975!

On Fri, Apr 16, 2021 at 11:36 AM Andrew Gallant ***@***.***>
wrote:

> Glad you got it figured out! Thanks for responding. Might be good to file
> a bug with Choco if you can. They might want to know that their
> installation is borked.
>
> With that said, generally most folks just use rustup directly to build end
> user applications like ripgrep. Compilers maintained by distros like Choco
> are typically used to build Rust software shipped by that distro.
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1851#issuecomment-821330861>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAHE7PQVYIZN4AIEGJVMKLTTJBYSJANCNFSM43B3ILHA>
> .
>


---
