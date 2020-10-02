# 1. Visual Studio Code HOWTO

<!-- TOC -->autoauto- [1. Visual Studio Code HOWTO](#1-visual-studio-code-howto)auto    - [1.1. Concepts](#11-concepts)auto        - [1.1.1. Partial string/command match](#111-partial-stringcommand-match)auto        - [1.1.2. Extension](#112-extension)auto    - [1.2. Common Shortcuts](#12-common-shortcuts)auto        - [1.2.1. Editing](#121-editing)auto        - [1.2.2. General](#122-general)auto        - [Extension related Shortcuts](#extension-related-shortcuts)autoauto<!-- /TOC -->

---

Great doc: https://code.visualstudio.com/docs/editor/codebasics#_multiple-selections-multicursor

**Good Extensions**
- https://itnext.io/12-vs-code-extensions-you-should-consider-using-4747e6281ee
- https://1stwebdesigner.com/top-free-extensions-for-vs-code/
- https://medium.freecodecamp.org/visual-studio-code-extensions-ff7f29b71341

[Run VS Code on Server and access via browser](https://dev.to/babak/how-to-run-vs-code-on-the-server-3c7h)

## 1.1. Concepts
[Based on Gitub's Electron Platform](https://github.com/electron/electron): based on Node.js and Chromium; use HTML, CSS and Javascript to build crosss-platform desk apps (e.g., slack)

Like Sublime Text

### 1.1.1. Partial string/command match
E.g., `ext ins` will match **Ext**ensions:**Ins**tall Extensions

### 1.1.2. Extension

Type `ext install python` in command palette

## 1.2. Common Shortcuts

[Shortcut Reference Page](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

### 1.2.1. Editing

Purpose  |  Shortcut
---------|----------
Block selection & multiple caret | `CTRL+ALT+UP` <br> `CTRL+ALT+DOWN`

### 1.2.2. General

Purpose  |  Shortcut
---------|----------
Go to File/Quick Open | `CTRL+P`
Show Tool Palette | `CTRL+SHIFT+P`

### Extension related Shortcuts

Purpose  |  Shortcut
---------|----------
`CTRL+M` `T` | Create/Update Table of Contents
`CTRL+k v` | Open **Makedown Preview Enhanced** preview
`CTRL+SHIFT+S` | Synch **Makedown Preview Enhanced** preview

## Remote Developement

### VSCode Remote SSH a Host 

- Install [VS Code’s Remote - SSH Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
- `Ctrl+Shift+P` -> `Remote-SSH: Open Configuration File ...`: `C:\Users\gao.qiang\.ssh\config` in my case. Then add the host info to it. The following is mine (add SSH key info if you want to avoid entering password):
```sh
Host CP4GPU
  HostName 105.64.210.10
  User gao.qiang
  IdentityFile C:\Users\gao.qiang\.ssh\id_rsa

Host BC_HEAD
  HostName 105.64.210.4
  User gao.qiang
  IdentityFile C:\Users\gao.qiang\.ssh\id_rsa
```
- `Ctrl+Shift+P` -> `Remote-SSH: Connect to a Host ...`:  select the host you want to connect to.

And that is it! It will automatically set up the remote site if this is the first time. Then you can open folder on the remote machine. Everything will be performed in the context of the remote machine.

### VSCode Remote SSH a Docker Container on a Remote Host

There is a `Remote - Docker` extension,  but I've found that it is very hard to make it work with remote container on Windows. `Remote-SSH`works better for me although it requires some **coordination** among users.
- Run docker with a port mapping, e.g., `docker run -p <mapped_ssh_port>:22 container1` where `22` is the default SSH port and `<mapped_ssh_port>` is the port you will use to SSH to the container, e.g., 52022.  

- Enable SSH in the docker. I use the following for Ubuntu (see [here](https://dev.to/s1ntaxe770r/how-to-setup-ssh-within-a-docker-container-i5i))
```sh
apt update && apt install  openssh-server sudo -y
service ssh start
# if you want to login as non-root, create a user 'test' for that
useradd -rm -d /home/test -s /bin/bash -g root -G sudo -u 1000 test
# whatever password you like
echo 'test:test' | chpasswd
```
- You can now use `ssh -p <mapped_ssh_port> <host_ip>` to SSH to the container. Note that use the **host’s IP addres**s, not the container’s IP address that is not externally accessible (you can find that by `docker inspect -f "{{ .NetworkSettings.IPAddress }}" *Container_Name*`).
- Enable root remote login if you want to login as `root` to the container (see [here](https://medium.com/@leicao.me/how-to-ssh-into-a-docker-container-remotely-as-root-or-a-non-root-user-b2105c797273))
```sh
sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
echo 'export NOTVISIBLE="in users profile"' >> ~/.bashrc
echo "export VISIBLE=now" >> /etc/profile
```

- Add the following to `C:\Users\<user_name>\.ssh\config`
```sh
  Host CP4GPU-2
  HostName 105.64.210.10
  User root
  Port 52022
  IdentityFile C:\Users\gao.qiang\.ssh\id_rsa
```

- Use VS Code’s `Remote-SSH` to connect to the container.
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               
