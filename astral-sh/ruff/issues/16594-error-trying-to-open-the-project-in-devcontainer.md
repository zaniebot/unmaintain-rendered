```yaml
number: 16594
title: Error trying to open the project in devcontainer in vscode
type: issue
state: open
author: OfekShilon
labels:
  - help wanted
assignees: []
created_at: 2025-03-08T20:59:20Z
updated_at: 2025-03-18T11:06:11Z
url: https://github.com/astral-sh/ruff/issues/16594
synced_at: 2026-01-12T15:54:55Z
```

# Error trying to open the project in devcontainer in vscode

---

_@OfekShilon_

### Summary

Running vscode over WSL.
The first error seems to be:
```
ERROR: failed to solve: failed to resolve source metadata for docker.io/docker/dockerfile:1.4: error getting credentials - err: exec: "docker-credential-dev-containers-f552cfcd-cc6f-4076-a95f-f48fe481eb28": executable file not found in $PATH, out: ``
```


Full log:
```
[2025-03-08T20:54:16.631Z] Dev Containers 0.401.0 in VS Code 1.98.0 (6609ac3d66f4eade5cf376d1cb76f13985724bcb).
[2025-03-08T20:54:16.631Z] Start: Run: wsl -d Ubuntu-24.04 -e wslpath -u \\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff
[2025-03-08T20:54:16.800Z] Stop (169 ms): Run: wsl -d Ubuntu-24.04 -e wslpath -u \\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff
[2025-03-08T20:54:16.805Z] Start: Resolving Remote
[2025-03-08T20:54:16.819Z] Start: Run: wsl -d Ubuntu-24.04 -e wslpath -u \\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff
[2025-03-08T20:54:16.972Z] Stop (153 ms): Run: wsl -d Ubuntu-24.04 -e wslpath -u \\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff
[2025-03-08T20:54:16.975Z] Start: Run: wsl -d Ubuntu-24.04 -e /bin/sh -c cd '/home/ofeks/src/ruff' && /bin/sh
[2025-03-08T20:54:16.988Z] Start: Run in host: id -un
[2025-03-08T20:54:17.066Z] ofeks
[2025-03-08T20:54:17.066Z] 
[2025-03-08T20:54:17.066Z] Stop (78 ms): Run in host: id -un
[2025-03-08T20:54:17.066Z] Start: Run in host:  (command -v getent >/dev/null 2>&1 && getent passwd 'ofeks' || grep -E '^ofeks|^[^:]*:[^:]*:ofeks:' /etc/passwd || true)
[2025-03-08T20:54:17.068Z] Stop (2 ms): Run in host:  (command -v getent >/dev/null 2>&1 && getent passwd 'ofeks' || grep -E '^ofeks|^[^:]*:[^:]*:ofeks:' /etc/passwd || true)
[2025-03-08T20:54:17.068Z] Start: Run in host: echo ~
[2025-03-08T20:54:17.069Z] /home/ofeks
[2025-03-08T20:54:17.069Z] 
[2025-03-08T20:54:17.069Z] Stop (1 ms): Run in host: echo ~
[2025-03-08T20:54:17.069Z] Start: Run in host: test -f '/home/ofeks/.vscode-server/cli/servers/Stable-6609ac3d66f4eade5cf376d1cb76f13985724bcb/server/node'
[2025-03-08T20:54:17.070Z] 
[2025-03-08T20:54:17.070Z] 
[2025-03-08T20:54:17.070Z] Exit code 1
[2025-03-08T20:54:17.071Z] Stop (2 ms): Run in host: test -f '/home/ofeks/.vscode-server/cli/servers/Stable-6609ac3d66f4eade5cf376d1cb76f13985724bcb/server/node'
[2025-03-08T20:54:17.071Z] Start: Run in host: test -f '/home/ofeks/.vscode/cli/servers/Stable-6609ac3d66f4eade5cf376d1cb76f13985724bcb/server/node'
[2025-03-08T20:54:17.072Z] 
[2025-03-08T20:54:17.073Z] 
[2025-03-08T20:54:17.073Z] Exit code 1
[2025-03-08T20:54:17.073Z] Stop (2 ms): Run in host: test -f '/home/ofeks/.vscode/cli/servers/Stable-6609ac3d66f4eade5cf376d1cb76f13985724bcb/server/node'
[2025-03-08T20:54:17.073Z] Start: Run in host: test -f '/home/ofeks/.vscode-server/bin/6609ac3d66f4eade5cf376d1cb76f13985724bcb/node'
[2025-03-08T20:54:17.074Z] 
[2025-03-08T20:54:17.074Z] 
[2025-03-08T20:54:17.075Z] Stop (2 ms): Run in host: test -f '/home/ofeks/.vscode-server/bin/6609ac3d66f4eade5cf376d1cb76f13985724bcb/node'
[2025-03-08T20:54:17.075Z] Start: Run in host: test -f '/home/ofeks/.vscode-server/bin/6609ac3d66f4eade5cf376d1cb76f13985724bcb/node_modules/node-pty/package.json'
[2025-03-08T20:54:17.076Z] 
[2025-03-08T20:54:17.076Z] 
[2025-03-08T20:54:17.076Z] Stop (1 ms): Run in host: test -f '/home/ofeks/.vscode-server/bin/6609ac3d66f4eade5cf376d1cb76f13985724bcb/node_modules/node-pty/package.json'
[2025-03-08T20:54:17.076Z] Start: Run in host: test -f '/home/ofeks/.vscode-remote-containers/dist/vscode-remote-containers-server-0.401.0.js'
[2025-03-08T20:54:17.077Z] 
[2025-03-08T20:54:17.077Z] 
[2025-03-08T20:54:17.077Z] Stop (1 ms): Run in host: test -f '/home/ofeks/.vscode-remote-containers/dist/vscode-remote-containers-server-0.401.0.js'
[2025-03-08T20:54:17.081Z] userEnvProbe: loginInteractiveShell (default)
[2025-03-08T20:54:17.081Z] userEnvProbe: not found in cache
[2025-03-08T20:54:17.081Z] userEnvProbe shell: /bin/bash
[2025-03-08T20:54:17.487Z] userEnvProbe PATHs:
Probe:     '/home/ofeks/.cargo/bin:/home/ofeks/.nvm/versions/node/v22.11.0/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/wsl/lib:/mnt/c/WINDOWS/system32:/mnt/c/WINDOWS:/mnt/c/WINDOWS/System32/Wbem:/mnt/c/WINDOWS/System32/WindowsPowerShell/v1.0/:/mnt/c/WINDOWS/System32/OpenSSH/:/mnt/c/Program Files/dotnet/:/mnt/c/Program Files/Git/cmd:/mnt/c/Users/OfekShilon/AppData/Local/UniGetUI/Chocolatey/bin:/mnt/c/Users/OfekShilon/AppData/Local/Microsoft/WindowsApps:/mnt/c/Users/OfekShilon/AppData/Local/Programs/Microsoft VS Code/bin:/mnt/c/Users/OfekShilon/AppData/Local/JetBrains/Toolbox/scripts:/snap/bin:/project/local/bin:/snap/bin'
Container: None
[2025-03-08T20:54:17.488Z] Setting up container for folder or workspace: /home/ofeks/src/ruff
[2025-03-08T20:54:17.510Z] Start: Check Docker is running
[2025-03-08T20:54:17.510Z] Start: Run in Host: docker version
[2025-03-08T20:54:17.530Z] Client: Docker Engine - Community
 Version:           27.5.1
 API version:       1.47
 Go version:        go1.22.11
 Git commit:        9f9e405
 Built:             Wed Jan 22 13:41:48 2025
 OS/Arch:           linux/amd64
 Context:
[2025-03-08T20:54:17.530Z]            default

Server: Docker Engine - Community
 Engine:
  Version:          27.5.1
  API version:      1.47 (minimum version 1.24)
  Go version:       go1.22.11
  Git commit:       4c9b3b0
  Built:            Wed Jan 22 13:41:48 2025
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.25
  GitCommit:        bcc810d6b9066471b0b6fa75f557a15a1cbf31bb
 runc:
  Version:          1.2.4
  GitCommit:        v1.2.4-0-g6c52b3f
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
[2025-03-08T20:54:17.533Z] Stop (23 ms): Run in Host: docker version
[2025-03-08T20:54:17.533Z] Stop (23 ms): Check Docker is running
[2025-03-08T20:54:17.533Z] Start: Run in Host: docker volume ls -q
[2025-03-08T20:54:17.548Z] Stop (15 ms): Run in Host: docker volume ls -q
[2025-03-08T20:54:17.549Z] Start: Run in Host: docker ps -q -a --filter label=vsch.local.folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff --filter label=vsch.quality=stable
[2025-03-08T20:54:17.565Z] Stop (16 ms): Run in Host: docker ps -q -a --filter label=vsch.local.folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff --filter label=vsch.quality=stable
[2025-03-08T20:54:17.565Z] Start: Run in Host: docker ps -q -a --filter label=devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff --filter label=devcontainer.config_file=/home/ofeks/src/ruff/.devcontainer/devcontainer.json
[2025-03-08T20:54:17.579Z] Stop (14 ms): Run in Host: docker ps -q -a --filter label=devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff --filter label=devcontainer.config_file=/home/ofeks/src/ruff/.devcontainer/devcontainer.json
[2025-03-08T20:54:17.579Z] Start: Run in Host: docker ps -q -a --filter label=devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff
[2025-03-08T20:54:17.592Z] Stop (13 ms): Run in Host: docker ps -q -a --filter label=devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff
[2025-03-08T20:54:17.593Z] Start: Run in Host: docker ps -q -a --filter label=devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff
[2025-03-08T20:54:17.606Z] Stop (13 ms): Run in Host: docker ps -q -a --filter label=devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff
[2025-03-08T20:54:17.607Z] Running Dev Containers CLI:   up --container-session-data-folder /tmp/devcontainers-3d142100-c338-4c6a-afea-59fec3398ecd1741467255670 --workspace-folder /home/ofeks/src/ruff --workspace-mount-consistency cached --gpu-availability detect --id-label devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff --id-label devcontainer.config_file=/home/ofeks/src/ruff/.devcontainer/devcontainer.json --log-level debug --log-format json --config /home/ofeks/src/ruff/.devcontainer/devcontainer.json --default-user-env-probe loginInteractiveShell --mount type=volume,source=vscode,target=/vscode,external=true --skip-post-create --update-remote-user-uid-default on --mount-workspace-git-root --include-configuration --include-merged-configuration
[2025-03-08T20:54:17.607Z] Start: Checking for Dev Containers CLI
[2025-03-08T20:54:17.611Z] Stop (4 ms): Checking for Dev Containers CLI
[2025-03-08T20:54:17.614Z] Start: Run in Host: /home/ofeks/.vscode-server/bin/6609ac3d66f4eade5cf376d1cb76f13985724bcb/node /home/ofeks/.vscode-remote-containers/dist/dev-containers-cli-0.401.0/dist/spec-node/devContainersSpecCLI.js up --container-session-data-folder /tmp/devcontainers-3d142100-c338-4c6a-afea-59fec3398ecd1741467255670 --workspace-folder /home/ofeks/src/ruff --workspace-mount-consistency cached --gpu-availability detect --id-label devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff --id-label devcontainer.config_file=/home/ofeks/src/ruff/.devcontainer/devcontainer.json --log-level debug --log-format json --config /home/ofeks/src/ruff/.devcontainer/devcontainer.json --default-user-env-probe loginInteractiveShell --mount type=volume,source=vscode,target=/vscode,external=true --skip-post-create --update-remote-user-uid-default on --mount-workspace-git-root --include-configuration --include-merged-configuration
[2025-03-08T20:54:17.948Z] @devcontainers/cli 0.74.0. Node.js v20.18.2. linux 5.15.167.4-microsoft-standard-WSL2 x64.
[2025-03-08T20:54:17.948Z] Start: Run: docker buildx version
[2025-03-08T20:54:17.990Z] Stop (42 ms): Run: docker buildx version
[2025-03-08T20:54:17.990Z] github.com/docker/buildx v0.20.0 8e30c46
[2025-03-08T20:54:17.990Z] 
[2025-03-08T20:54:17.990Z] Start: Run: docker -v
[2025-03-08T20:54:18.002Z] Stop (12 ms): Run: docker -v
[2025-03-08T20:54:18.002Z] Start: Resolving Remote
[2025-03-08T20:54:18.005Z] Start: Run: git rev-parse --show-cdup
[2025-03-08T20:54:18.007Z] Stop (2 ms): Run: git rev-parse --show-cdup
[2025-03-08T20:54:18.009Z] Start: Run: docker ps -q -a --filter label=devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff --filter label=devcontainer.config_file=/home/ofeks/src/ruff/.devcontainer/devcontainer.json
[2025-03-08T20:54:18.023Z] Stop (14 ms): Run: docker ps -q -a --filter label=devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff --filter label=devcontainer.config_file=/home/ofeks/src/ruff/.devcontainer/devcontainer.json
[2025-03-08T20:54:18.023Z] Start: Run: docker inspect --type image mcr.microsoft.com/devcontainers/rust:0-1-bullseye
[2025-03-08T20:54:18.036Z] Stop (13 ms): Run: docker inspect --type image mcr.microsoft.com/devcontainers/rust:0-1-bullseye
[2025-03-08T20:54:19.254Z] Resolving Feature dependencies for 'ghcr.io/devcontainers/features/python'...
[2025-03-08T20:54:19.254Z] * Processing feature: ghcr.io/devcontainers/features/python
[2025-03-08T20:54:19.418Z] Start: Run: docker-credential-dev-containers-f552cfcd-cc6f-4076-a95f-f48fe481eb28 get
[2025-03-08T20:54:19.494Z] Stop (76 ms): Run: docker-credential-dev-containers-f552cfcd-cc6f-4076-a95f-f48fe481eb28 get
[2025-03-08T20:54:19.496Z] Stop (78 ms): Run: docker-credential-dev-containers-f552cfcd-cc6f-4076-a95f-f48fe481eb28 get
[2025-03-08T20:54:19.861Z] * Processing feature: ghcr.io/devcontainers/features/common-utils
[2025-03-08T20:54:20.075Z] * Processing feature: ghcr.io/devcontainers/features/oryx
[2025-03-08T20:54:20.297Z] Soft-dependency 'ghcr.io/devcontainers/features/oryx' is not required.  Removing from installation order...
[2025-03-08T20:54:20.297Z] Soft-dependency 'ghcr.io/devcontainers/features/common-utils' is not required.  Removing from installation order...
[2025-03-08T20:54:20.298Z] * Fetching feature: python_0_oci
[2025-03-08T20:54:20.749Z] Files to omit: ''
[2025-03-08T20:54:20.757Z] * Fetched feature: python_0_oci version 1.7.0
[2025-03-08T20:54:20.763Z] Start: Run: docker buildx build --load --build-context dev_containers_feature_content_source=/tmp/devcontainercli-ofeks/container-features/0.74.0-1741467259250 --build-arg _DEV_CONTAINERS_BASE_IMAGE=mcr.microsoft.com/devcontainers/rust:0-1-bullseye --build-arg _DEV_CONTAINERS_IMAGE_USER=root --build-arg _DEV_CONTAINERS_FEATURE_CONTENT_SOURCE=dev_container_feature_content_temp --target dev_containers_target_stage -f /tmp/devcontainercli-ofeks/container-features/0.74.0-1741467259250/Dockerfile.extended -t vsc-ruff-4d05958e2404e3862fd28f52af4b3a5ce738d28a60dd07705b7cad3b3edc54aa-features /tmp/devcontainercli-ofeks/empty-folder
[2025-03-08T20:54:20.845Z] [+] Building 0.0s (0/1)                                          docker:default
[2025-03-08T20:54:21.008Z] [+] Building 0.2s (1/2)                                          docker:default
 => [internal] load build definition from Dockerfile.extended              0.0s
 => => transferring dockerfile: 3.19kB                                     0.0s
 => resolve image config for docker-image://docker.io/docker/dockerfile:1  0.2s
[2025-03-08T20:54:21.158Z] [+] Building 0.3s (1/2)                                          docker:default
 => [internal] load build definition from Dockerfile.extended              0.0s
 => => transferring dockerfile: 3.19kB                                     0.0s
 => resolve image config for docker-image://docker.io/docker/dockerfile:1  0.3s
[2025-03-08T20:54:21.307Z] [+] Building 0.5s (1/2)                                          docker:default
 => [internal] load build definition from Dockerfile.extended              0.0s
 => => transferring dockerfile: 3.19kB                                     0.0s
 => resolve image config for docker-image://docker.io/docker/dockerfile:1  0.5s
[2025-03-08T20:54:21.318Z] [+] Building 0.5s (2/2)                                          docker:default
 => [internal] load build definition from Dockerfile.extended              0.0s
 => => transferring dockerfile: 3.19kB                                     0.0s
 => ERROR resolve image config for docker-image://docker.io/docker/docker  0.5s
[2025-03-08T20:54:21.354Z] [+] Building 0.5s (2/2) FINISHED                                 docker:default
 => [internal] load build definition from Dockerfile.extended              0.0s
 => => transferring dockerfile: 3.19kB                                     0.0s
 => ERROR resolve image config for docker-image://docker.io/docker/docker  0.5s
------
 > resolve image config for docker-image://docker.io/docker/dockerfile:1.4:
------
[2025-03-08T20:54:21.357Z] Dockerfile.extended:1
--------------------
   1 | >>> # syntax=docker/dockerfile:1.4
   2 |     ARG _DEV_CONTAINERS_BASE_IMAGE=placeholder
   3 |     
--------------------
ERROR: failed to solve: failed to resolve source metadata for docker.io/docker/dockerfile:1.4: error getting credentials - err: exec: "docker-credential-dev-containers-f552cfcd-cc6f-4076-a95f-f48fe481eb28": executable file not found in $PATH, out: ``
[2025-03-08T20:54:21.365Z] Stop (602 ms): Run: docker buildx build --load --build-context dev_containers_feature_content_source=/tmp/devcontainercli-ofeks/container-features/0.74.0-1741467259250 --build-arg _DEV_CONTAINERS_BASE_IMAGE=mcr.microsoft.com/devcontainers/rust:0-1-bullseye --build-arg _DEV_CONTAINERS_IMAGE_USER=root --build-arg _DEV_CONTAINERS_FEATURE_CONTENT_SOURCE=dev_container_feature_content_temp --target dev_containers_target_stage -f /tmp/devcontainercli-ofeks/container-features/0.74.0-1741467259250/Dockerfile.extended -t vsc-ruff-4d05958e2404e3862fd28f52af4b3a5ce738d28a60dd07705b7cad3b3edc54aa-features /tmp/devcontainercli-ofeks/empty-folder
[2025-03-08T20:54:21.277Z] Error: Command failed: docker buildx build --load --build-context dev_containers_feature_content_source=/tmp/devcontainercli-ofeks/container-features/0.74.0-1741467259250 --build-arg _DEV_CONTAINERS_BASE_IMAGE=mcr.microsoft.com/devcontainers/rust:0-1-bullseye --build-arg _DEV_CONTAINERS_IMAGE_USER=root --build-arg _DEV_CONTAINERS_FEATURE_CONTENT_SOURCE=dev_container_feature_content_temp --target dev_containers_target_stage -f /tmp/devcontainercli-ofeks/container-features/0.74.0-1741467259250/Dockerfile.extended -t vsc-ruff-4d05958e2404e3862fd28f52af4b3a5ce738d28a60dd07705b7cad3b3edc54aa-features /tmp/devcontainercli-ofeks/empty-folder
[2025-03-08T20:54:21.277Z]     at ytA (/home/ofeks/.vscode-remote-containers/dist/dev-containers-cli-0.401.0/dist/spec-node/devContainersSpecCLI.js:468:1260)
[2025-03-08T20:54:21.277Z]     at bH (/home/ofeks/.vscode-remote-containers/dist/dev-containers-cli-0.401.0/dist/spec-node/devContainersSpecCLI.js:468:1002)
[2025-03-08T20:54:21.277Z]     at async TtA (/home/ofeks/.vscode-remote-containers/dist/dev-containers-cli-0.401.0/dist/spec-node/devContainersSpecCLI.js:485:3848)
[2025-03-08T20:54:21.277Z]     at async iB (/home/ofeks/.vscode-remote-containers/dist/dev-containers-cli-0.401.0/dist/spec-node/devContainersSpecCLI.js:485:4963)
[2025-03-08T20:54:21.277Z]     at async wrA (/home/ofeks/.vscode-remote-containers/dist/dev-containers-cli-0.401.0/dist/spec-node/devContainersSpecCLI.js:666:203)
[2025-03-08T20:54:21.277Z]     at async DrA (/home/ofeks/.vscode-remote-containers/dist/dev-containers-cli-0.401.0/dist/spec-node/devContainersSpecCLI.js:665:14830)
[2025-03-08T20:54:21.277Z]     at async /home/ofeks/.vscode-remote-containers/dist/dev-containers-cli-0.401.0/dist/spec-node/devContainersSpecCLI.js:485:1190
[2025-03-08T20:54:21.286Z] Stop (3672 ms): Run in Host: /home/ofeks/.vscode-server/bin/6609ac3d66f4eade5cf376d1cb76f13985724bcb/node /home/ofeks/.vscode-remote-containers/dist/dev-containers-cli-0.401.0/dist/spec-node/devContainersSpecCLI.js up --container-session-data-folder /tmp/devcontainers-3d142100-c338-4c6a-afea-59fec3398ecd1741467255670 --workspace-folder /home/ofeks/src/ruff --workspace-mount-consistency cached --gpu-availability detect --id-label devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff --id-label devcontainer.config_file=/home/ofeks/src/ruff/.devcontainer/devcontainer.json --log-level debug --log-format json --config /home/ofeks/src/ruff/.devcontainer/devcontainer.json --default-user-env-probe loginInteractiveShell --mount type=volume,source=vscode,target=/vscode,external=true --skip-post-create --update-remote-user-uid-default on --mount-workspace-git-root --include-configuration --include-merged-configuration
[2025-03-08T20:54:21.286Z] Exit code 1
[2025-03-08T20:54:21.289Z] Command failed: /home/ofeks/.vscode-server/bin/6609ac3d66f4eade5cf376d1cb76f13985724bcb/node /home/ofeks/.vscode-remote-containers/dist/dev-containers-cli-0.401.0/dist/spec-node/devContainersSpecCLI.js up --container-session-data-folder /tmp/devcontainers-3d142100-c338-4c6a-afea-59fec3398ecd1741467255670 --workspace-folder /home/ofeks/src/ruff --workspace-mount-consistency cached --gpu-availability detect --id-label devcontainer.local_folder=\\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff --id-label devcontainer.config_file=/home/ofeks/src/ruff/.devcontainer/devcontainer.json --log-level debug --log-format json --config /home/ofeks/src/ruff/.devcontainer/devcontainer.json --default-user-env-probe loginInteractiveShell --mount type=volume,source=vscode,target=/vscode,external=true --skip-post-create --update-remote-user-uid-default on --mount-workspace-git-root --include-configuration --include-merged-configuration
[2025-03-08T20:54:21.289Z] Exit code 1
[2025-03-08T20:54:50.309Z] Start: Run: wsl -d Ubuntu-24.04 -e wslpath -u \\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff
[2025-03-08T20:54:50.514Z] Stop (205 ms): Run: wsl -d Ubuntu-24.04 -e wslpath -u \\wsl.localhost\Ubuntu-24.04\home\ofeks\src\ruff
```


### Version

Current github source

---

_Label `question` added by @dhruvmanila on 2025-03-10 05:25_

---

_Comment by @dhruvmanila on 2025-03-10 05:28_

(I think I prematurely transferred this issue to the Ruff VS Code extension, moved it back to Ruff.)

Is this related to opening the Ruff source code in VS Code devcontainer? Can you provide the steps to reproduce this error?

---

_Comment by @OfekShilon on 2025-03-10 08:33_

@dhruvmanila  Yes, this is exactly about being unable to open the Ruff source code in the VSCode devcontainer. Here's a capture of a simple repro: open the project folder, then 'Reopen in dev container'.

https://github.com/user-attachments/assets/d83e5234-132e-40d2-8cfe-13c652072c79

---

_Comment by @MichaReiser on 2025-03-10 09:27_

Hmm, not sure. I don't think anyone on the team uses devcontainer. We added it when we had a contributor that was excited about it. I'm able to open the ruff repo using Github's codespaces project without any errors (there's an extension that seems outdated). 

Have you tried googling for the error message? It seems very generic to me

https://forums.docker.com/t/error-failed-to-solve-error-getting-credentials-err-exit-status-1-out/136124

---

_Comment by @OfekShilon on 2025-03-10 11:57_

I see. Not sure I'd be able to look into it any time soon but please leave this open, so others can at least see it's a known issue.

---

_Label `question` removed by @dhruvmanila on 2025-03-18 11:06_

---

_Label `help wanted` added by @dhruvmanila on 2025-03-18 11:06_

---
