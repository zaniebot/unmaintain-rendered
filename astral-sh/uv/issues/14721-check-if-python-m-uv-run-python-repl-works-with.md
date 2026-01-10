---
number: 14721
title: "Check if `python -m uv run python` REPL works with Ctrl-C on Windows"
type: issue
state: open
author: zanieb
labels:
  - good first issue
  - windows
assignees: []
created_at: 2025-07-18T13:09:03Z
updated_at: 2025-07-20T03:04:54Z
url: https://github.com/astral-sh/uv/issues/14721
synced_at: 2026-01-10T01:25:48Z
---

# Check if `python -m uv run python` REPL works with Ctrl-C on Windows

---

_Issue opened by @zanieb on 2025-07-18 13:09_

If you run `python -m uv run python` to get a REPL on Windows. What happens when you use Ctrl-C?

_Originally posted by @geofft in https://github.com/astral-sh/uv/issues/14715#issuecomment-3089308034_
            

---

_Label `good first issue` added by @zanieb on 2025-07-18 13:09_

---

_Label `windows` added by @zanieb on 2025-07-18 13:09_

---

_Comment by @kfsone on 2025-07-20 03:00_

TLDR:
- Forward the stdio handles and the console to the child process,
- But create the ChildProcess suspended,
- If CreateProcess() fails, you still own the handles,
- If CreateProcess() succeeded, you no-longer own the handles, release them and ResumeThread() the child.

I recently ran into Window's variation of ctrl-c handling. Out of the box a process group is wired to send ctrl-c to the entire group in tree-descent order; the parent will get the ctrl-c first. If you're trying to emulate POSIX exec() this is a wee bit inconvenient.

Certainly, `uv run python` followed by a ctrl-c will result in:

<img width="198" height="144" alt="Image" src="https://github.com/user-attachments/assets/d73fa433-bd04-43c3-9bbc-7dadcaaf1e06" />

and now the shell and the python instance are sharing some kind of handle so:

<img width="247" height="329" alt="Image" src="https://github.com/user-attachments/assets/a747ce01-dcbd-4b5f-a3b2-c33ca369061b" />

I was dealing with this in C++ and the [MS documentation was leading me in circles](https://stackoverflow.com/questions/79696425/posix-exec-like-transfer-of-console-handles-to-createprocess-child).

- A signal handler that ignores Ctrl-Cs but still forwards breaks (so you can't get stuck),

```
// -----------------------------------------------------------------------------
//! Control C handler if we want to be authoritative rather than the child.
//
BOOL WINAPI consoleCtrlHandler(DWORD ctrlType)
{
	// If it's not an event we care about, or if the handler is not installed,
	// let the system handle it the normal way.
	if (ctrlType == CTRL_C_EVENT)
	{
		// Let Ctrl-C pass thru.
		return (TRUE);
	}
	if (ctrlType == CTRL_BREAK_EVENT)
		terminateProcess();  // <- my code is using a singleton, because `exec`.

	return (FALSE);
}
```

For the process launch itself, I'm doing these things:

```
	HANDLE hStdIn = GetStdHandle(STD_INPUT_HANDLE);
	HANDLE hStdOut = GetStdHandle(STD_OUTPUT_HANDLE);
	HANDLE hStdErr = GetStdHandle(STD_ERROR_HANDLE);

	// Setup process creation, explicitly forward the handles.
	STARTUPINFO si = {};
	si.cb = sizeof(si);
	si.dwFlags = STARTF_USESTDHANDLES;
	si.hStdInput = hStdIn;
	si.hStdOutput = hStdOut;
	si.hStdError = hStdErr;

	// Ensure our io activities are finished
	_flushall();

	// Create the child process in a new process group
	BOOL success = CreateProcess(
		nullptr,                    // No module name (use command line)
		const_cast<char*>(cmdline.c_str()),			// Command line
		nullptr,                    // Process handle not inheritable
		nullptr,                    // Thread handle not inheritable
		TRUE,                       // Set handle inheritance to TRUE
  // vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv
		0 | CREATE_SUSPENDED, 		// Creation flags - normal priority
  // ^^^^^^^^^^^^^^^^^^^^^^
		nullptr,                    // Use parent's environment block
		nullptr,                    // Use parent's starting directory
		&si,                        // Pointer to STARTUPINFO structure
		gProcessInfo               	// Pointer to PROCESS_INFORMATION structure
	);
```

At this point, you still have access to the `Console` and stdio descriptors, so if `CreateProcess` fails you can back out/log to the user.

On the other hand, if `CreateProcess` suceeded, the child process is in a suspended state, allowing you to free those handles AND the console before retiring.

```

	DWORD exitCode = 0;
	if (!success)
	{
		DWORD errorCode = GetLastError();
		char* errorMessage = nullptr;
		FormatMessageA(FORMAT_MESSAGE_ALLOCATE_BUFFER | FORMAT_MESSAGE_FROM_SYSTEM,
		                 nullptr, errorCode, 0, (LPSTR)&errorMessage, 0, nullptr);
		error = "Failed to create process: " + Base::String(errorMessage);
		LocalFree(errorMessage);
		return static_cast<int>(exitCode);
	}

	// Let the child determine what to do with Ctrl-C; not our responsibility any more.
	SetConsoleCtrlHandler(consoleCtrlHandler, TRUE);

	// Surrender all of the handles to prevent mux/switching of receipients
	// which can manifest as weird behavior when hitting control keys, e.g
	CloseHandle(hStdIn);
	CloseHandle(hStdOut);
	CloseHandle(hStdErr);
	// Release the layer that does input translations too
	FreeConsole();

	// Now if the thread accesses the console or stdio handles, they will appear to
	// belong to it and it be as interactive/streamed as we are.
	ResumeThread(gProcessInfo->hThread);
	CloseHandle(gProcessInfo->hThread);  // Avoid resource leak.

	// And now we just need to wait to exit.
	WaitForSingleObject(gProcessInfo->hProcess, INFINITE);
	GetExitCodeProcess(gProcessInfo->hProcess, &exitCode);
	CloseHandle(gProcessInfo->hProcess);
	::exit(exitCode);
```

I'm using this for an in-house shim for wrapping uv to maintain workspace/branch-specific venvs but then launch the venv python directly because using uv doesn't otherwise do this:

<img width="393" height="253" alt="Image" src="https://github.com/user-attachments/assets/2efcabc1-f044-409d-bcf3-8321ea4b97a0" />



---
