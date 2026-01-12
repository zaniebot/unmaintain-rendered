```yaml
number: 13788
title: "Interpreter can't open server"
type: issue
state: closed
author: FelixSoderstrom
labels:
  - question
  - server
assignees: []
created_at: 2024-10-17T07:40:15Z
updated_at: 2024-10-17T09:13:18Z
url: https://github.com/astral-sh/ruff/issues/13788
synced_at: 2026-01-12T15:54:53Z
```

# Interpreter can't open server

---

_@FelixSoderstrom_

### Issue
Ruff doesn't find server because it doesn't exist.
This issue arose when I moved my base interpreter to a different directory.
Other extensions work just fine.
I don't know why it's looking for server in "../Desktop/Code". I never had python installed here.


### Log TLDR
path\to\interpreter: can't open file '...Desktop\Code\server': [Errno 2] no such file or directory.

### Full log
[message.txt](https://github.com/user-attachments/files/17408926/message.txt)




---

_Label `question` added by @MichaReiser on 2024-10-17 08:02_

---

_Label `server` added by @MichaReiser on 2024-10-17 08:02_

---

_Comment by @MichaReiser on 2024-10-17 08:03_

I'm sorry you're running into this. 

Hmm, I can't download the `message.txt`. I get some request error. Could you try reuploading the file or pasting its content in a code block?

Could you share your editor config with us. I wonder if you set `ruff.path` or similar. 

---

_Label `needs-info` added by @MichaReiser on 2024-10-17 08:03_

---

_Comment by @FelixSoderstrom on 2024-10-17 08:37_

Hi thanks for looking into this. It's really appreciated.
I reuploaded the file but will send you a block just in case.

```
2024-10-17 10:34:57.009 [info] Name: Ruff
2024-10-17 10:34:57.009 [info] Module: ruff
2024-10-17 10:34:57.009 [info] Using interpreter: C:\Python\python.exe
2024-10-17 10:34:57.009 [info] Using 'path' setting: C:\Python\python.exe
2024-10-17 10:34:57.009 [info] Resolved 'ruff.nativeServer: auto' to use the native server
2024-10-17 10:34:57.009 [info] Found Ruff 3.13.0 at C:\Python\python.exe
2024-10-17 10:34:57.009 [info] Server run command: C:\Python\python.exe server
2024-10-17 10:34:57.009 [info] Server: Start requested.
2024-10-17 10:34:57.009 [info] C:\Python\python.exe: can't open file 'c:\\Users\\Felix\\Desktop\\Code\\server': [Errno 2] No such file or directory

2024-10-17 10:34:57.010 [info] [Error - 10:34:56 AM] Server initialization failed.
2024-10-17 10:34:57.010 [info]   Message: Pending response rejected since connection got disposed
  Code: -32097 
2024-10-17 10:34:57.010 [info] [Info  - 10:34:56 AM] Connection to server got closed. Server will restart.
2024-10-17 10:34:57.010 [info] true
2024-10-17 10:34:57.010 [info] [Error - 10:34:56 AM] Ruff client: couldn't create connection to server.
2024-10-17 10:34:57.010 [info]   Message: Pending response rejected since connection got disposed
  Code: -32097 
2024-10-17 10:34:57.178 [info] [Error - 10:34:57 AM] Client Ruff: connection to server is erroring. Shutting down server.
2024-10-17 10:34:57.178 [info] [Error - 10:34:57 AM] Stopping server failed
2024-10-17 10:34:57.179 [info] Error: Client is not running and can't be stopped. It's current state is: startFailed
    at D.shutdown (c:\Users\Felix\.vscode\extensions\charliermarsh.ruff-2024.50.0-win32-x64\dist\webpack:\ruff\node_modules\vscode-languageclient\lib\common\client.js:914:19)
    at D.stop (c:\Users\Felix\.vscode\extensions\charliermarsh.ruff-2024.50.0-win32-x64\dist\webpack:\ruff\node_modules\vscode-languageclient\lib\common\client.js:885:21)
    at D.stop (c:\Users\Felix\.vscode\extensions\charliermarsh.ruff-2024.50.0-win32-x64\dist\webpack:\ruff\node_modules\vscode-languageclient\lib\node\main.js:150:22)
    at D.handleConnectionError (c:\Users\Felix\.vscode\extensions\charliermarsh.ruff-2024.50.0-win32-x64\dist\webpack:\ruff\node_modules\vscode-languageclient\lib\common\client.js:1146:18)
    at processTicksAndRejections (node:internal/process/task_queues:95:5)
2024-10-17 10:34:57.179 [info] [Error - 10:34:57 AM] Server initialization failed.
2024-10-17 10:34:57.179 [info]   Message: write EPIPE
  Code: -32099 
2024-10-17 10:34:57.179 [info] [Error - 10:34:57 AM] Ruff client: couldn't create connection to server.
2024-10-17 10:34:57.179 [info]   Message: write EPIPE
  Code: -32099 
2024-10-17 10:34:57.179 [error] Server: Start failed: Error: write EPIPE
2024-10-17 10:34:57.179 [info] [Error - 10:34:57 AM] Restarting server failed
2024-10-17 10:34:57.179 [info]   Message: write EPIPE
  Code: -32099 
2024-10-17 10:34:57.179 [info] C:\Python\python.exe: can't open file 'c:\\Users\\Felix\\Desktop\\Code\\server': [Errno 2] No such file or directory

2024-10-17 10:34:57.190 [info] [Info  - 10:34:57 AM] Connection to server got closed. Server will restart.
2024-10-17 10:34:57.190 [info] true
2024-10-17 10:34:57.357 [info] [Error - 10:34:57 AM] Client Ruff: connection to server is erroring. Shutting down server.
2024-10-17 10:34:57.357 [info] [Error - 10:34:57 AM] Stopping server failed
2024-10-17 10:34:57.357 [info] Error: Client is not running and can't be stopped. It's current state is: starting
    at D.shutdown (c:\Users\Felix\.vscode\extensions\charliermarsh.ruff-2024.50.0-win32-x64\dist\webpack:\ruff\node_modules\vscode-languageclient\lib\common\client.js:914:19)
    at D.stop (c:\Users\Felix\.vscode\extensions\charliermarsh.ruff-2024.50.0-win32-x64\dist\webpack:\ruff\node_modules\vscode-languageclient\lib\common\client.js:885:21)
    at D.stop (c:\Users\Felix\.vscode\extensions\charliermarsh.ruff-2024.50.0-win32-x64\dist\webpack:\ruff\node_modules\vscode-languageclient\lib\node\main.js:150:22)
    at D.handleConnectionError (c:\Users\Felix\.vscode\extensions\charliermarsh.ruff-2024.50.0-win32-x64\dist\webpack:\ruff\node_modules\vscode-languageclient\lib\common\client.js:1146:18)
    at processTicksAndRejections (node:internal/process/task_queues:95:5)
2024-10-17 10:34:57.357 [info] [Error - 10:34:57 AM] Server initialization failed.
2024-10-17 10:34:57.358 [info]   Message: write EPIPE
  Code: -32099 
2024-10-17 10:34:57.358 [info] [Error - 10:34:57 AM] Ruff client: couldn't create connection to server.
2024-10-17 10:34:57.358 [info]   Message: write EPIPE
  Code: -32099 
2024-10-17 10:34:57.358 [info] [Error - 10:34:57 AM] Restarting server failed
2024-10-17 10:34:57.358 [info]   Message: write EPIPE
  Code: -32099 
2024-10-17 10:34:57.359 [info] C:\Python\python.exe: can't open file 'c:\\Users\\Felix\\Desktop\\Code\\server': [Errno 2] No such file or directory

2024-10-17 10:34:57.367 [info] [Info  - 10:34:57 AM] Connection to server got closed. Server will restart.
2024-10-17 10:34:57.367 [info] true
2024-10-17 10:34:57.502 [info] C:\Python\python.exe: can't open file 'c:\\Users\\Felix\\Desktop\\Code\\server': [Errno 2] No such file or directory

2024-10-17 10:34:57.518 [info] [Error - 10:34:57 AM] Server initialization failed.
2024-10-17 10:34:57.518 [info]   Message: Pending response rejected since connection got disposed
  Code: -32097 
2024-10-17 10:34:57.518 [info] [Info  - 10:34:57 AM] Connection to server got closed. Server will restart.
2024-10-17 10:34:57.518 [info] true
2024-10-17 10:34:57.518 [info] [Error - 10:34:57 AM] Ruff client: couldn't create connection to server.
2024-10-17 10:34:57.518 [info]   Message: Pending response rejected since connection got disposed
  Code: -32097 
2024-10-17 10:34:57.624 [info] C:\Python\python.exe: can't open file 'c:\\Users\\Felix\\Desktop\\Code\\server': [Errno 2] No such file or directory

2024-10-17 10:34:57.690 [info] [Error - 10:34:57 AM] Server initialization failed.
2024-10-17 10:34:57.691 [info]   Message: Pending response rejected since connection got disposed
  Code: -32097 
2024-10-17 10:34:57.691 [info] [Error - 10:34:57 AM] The Ruff server crashed 5 times in the last 3 minutes. The server will not be restarted. See the output for more information.
2024-10-17 10:34:57.691 [info] [Error - 10:34:57 AM] Ruff client: couldn't create connection to server.
2024-10-17 10:34:57.691 [info]   Message: Pending response rejected since connection got disposed
  Code: -32097 
2024-10-17 10:34:57.691 [info] [Error - 10:34:57 AM] Restarting server failed
2024-10-17 10:34:57.691 [info]   Message: Pending response rejected since connection got disposed
  Code: -32097
```

---

_Comment by @dhruvmanila on 2024-10-17 08:41_

I think you've set the `ruff.path` to the Python interpreter while the value should be a path to the `ruff` executable: https://docs.astral.sh/ruff/editors/settings/#path

---

_Comment by @FelixSoderstrom on 2024-10-17 08:46_

Very silly!
I got them mixed up in my 2AM shenanigans trying to fix this yesterday.
Thank you for helping me solve my upside down usb-problem!

---

_Closed by @FelixSoderstrom on 2024-10-17 08:48_

---

_Label `needs-info` removed by @dhruvmanila on 2024-10-17 09:13_

---
