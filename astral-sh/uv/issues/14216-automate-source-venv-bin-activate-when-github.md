```yaml
number: 14216
title: "Automate `source .venv/bin/activate` when github codesspaces starts"
type: issue
state: open
author: oliverangelil
labels:
  - question
assignees: []
created_at: 2025-06-23T14:48:26Z
updated_at: 2025-06-27T15:45:49Z
url: https://github.com/astral-sh/uv/issues/14216
synced_at: 2026-01-10T03:32:45Z
```

# Automate `source .venv/bin/activate` when github codesspaces starts

---

_Issue opened by @oliverangelil on 2025-06-23 14:48_

### Question

I am using github codespaces with UV. 
I don't want to have to type out:
```
uv venv
source .venv/bin/activate
uvsync

```
every time I start a new codespace.

I am using a `postCreateCommand.sh` script with the above 3 lines of code.

I also have a `devcontainer.json` file that looks like:

```
// For format details, see https://aka.ms/devcontainer.json. For config options, see the
// README at: https://github.com/devcontainers/templates/tree/main/src/python
{
	"name": "Python 3",
	// Or use a Dockerfile or Docker Compose file. More info: https://containers.dev/guide/dockerfile
	"image": "mcr.microsoft.com/devcontainers/base:debian",
	"features": {
		"ghcr.io/va-h/devcontainers-features/uv:1": {},
		"ghcr.io/dhoeric/features/google-cloud-cli:1": {},
		"ghcr.io/robbert229/devcontainer-features/opentofu:1": {}	
	},
	// Use 'forwardPorts' to make a list of ports inside the container available locally.
	// "forwardPorts": [],

	// Use 'postCreateCommand' to run commands after the container is created.
	"postCreateCommand": "./.devcontainer/postCreateCommand.sh",

	// Configure tool-specific properties.
	"customizations": {
		// Configure properties specific to VS Code.
		"vscode": {
	  	// Add the IDs of extensions you want installed when the container is created.
	  		"extensions": [
				"github.copilot",
				"charliermarsh.ruff",
				"github.vscode-github-actions",
				"ms-python.vscode-pylance"
	  		]
		}
  	}
}
```

However, it runs the `postCreateCommand.sh` command too soon which leads to some odd side effects. For example see #14215 . 

What is the recommended way to automate this in codespaces?

### Platform

Debian GNU/Linux 12 (bookworm)

### Version

uv 0.7.13

---

_Label `question` added by @oliverangelil on 2025-06-23 14:48_

---

_Comment by @konstin on 2025-06-23 14:57_

I'm not familiar with the details of codespaces, but activating a venv is setting `VIRTUAL_ENV`, adding the bin dir to `PATH` and (optionally) changing the shell prompt, so maybe the way the scripts are run interact badly with your shell, you may be able to fix this by applying the environment variable changes through a different devcontainer option.

---

_Comment by @nataziel on 2025-06-25 23:59_

I don't believe that is the correct way to use the `postCreateCommand`, as it runs separately to whatever terminal(s) you might be using.

I do a lot of work in devcontainers (not codespaces) but I think they work similarly. Assuming you have `pyproject.toml` setup correctly for `uv`, you can use the `postCreateCommand` to sync the venv like so:

``` json
{
  ...
  "postCreateCommand": "uv sync",
  ...
}
```

`uv sync` will handle creating the venv if it doesn't already exist so you don't need to manually create it & activate it yourself before running the command.

Then as long as you have selected that venv as the python interpreter, when you launch a new terminal it should automatically activate that venv. This behaviour is controlled by the  `python.terminal.activateEnvironment` setting in vscode (hopefully there's a similar setting in codespaces). 

---

_Comment by @oliverangelil on 2025-06-27 15:45_

I've added this line to devcontainer.json:
`"postCreateCommand": "uv sync && echo 'source .venv/bin/activate' >> ~/.bashrc",
`

You can see the full code in [this](https://github.com/Ishangoai/AIMS_course/blob/49ae94ec06c4f7efc5c0d9f0a5a8eaf5cb1d15d3/.devcontainer/devcontainer.json) commit.

---
