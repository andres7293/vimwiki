# SSH

ssh (SSH client) is a program for logging into a remote machine and for executing commands on a remote machine.

## Generate ssh key

```bash
ssh-keygen
```

Generate ssh key with default parameters.
The public and private key are stored in ~/.ssh/

## Copy ssh key to remote server

```bash
ssh-copy-id ~./ssh/id_rsa.pub <hostname>@<ip>
```

Copy public key to authorized key file in remote server. This allows us to login without user and password every time.

## Add ssh key to openSSH authentication agent

```bash
ssh-add
```

This command searches ssh private key to ~/.ssh/id_rsa folder.
Adding ssh-key to openSSH agent allow us write passphrase once.

## Execute X11 apps through ssh

```bash
#Client machine
ssh -X <host>@<ip>
```

```bash
#Edit in server machine
vim /etc/ssh/sshd_config
#Uncomment 
X11Forwarding yes
```

## Force tty

```bash
ssh -t <host>@<ip> vim
```

ssh force tty allocation, so we can use cli apps without problem

## Tunnel/proxy ssh

Read man ssh

```bash
ssh -D <local port> <host>@<ip>
```

Adding proxy to firefox --> ip = localhost; port =<local port>
This way firefox is sending all http/https connections encrypted through ssh. Is the remote machine which forwards all http connections

Adding this proxy at system level, proxies all connection through ssh tunnel


## keychain

```bash
eval `keychain -q --eval --agents ssh id_rsa`
```

Add previous to ~/.bashrc or ~/.zshrc
This command execute ssh-add the first time terminal is opened. So you enter passphrase once
