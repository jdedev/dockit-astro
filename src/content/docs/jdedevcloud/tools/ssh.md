---
title : SSH
---

# SSH

## Generate ssh key / identity

```shell
ssh-keygen -t ed25519 -C "jdedev@gmail.com" -f id_ed25519_serge`
```

## SSH-Agent (optional)

### List all identities

```shell
ssh-add.exe -l
```

### Delete all identities

```shell
ssh-add.exe -D
```

### Add a identity

```shell
ssh-add.exe   .\id_ed25519_serge
```

## Setup ssh config

### Choose ssh identity file for a host

```shell
vi ~/.ssh/config
```

```shell
Host github.com    HostName github.com    User git    IdentityFile ~/.ssh/id_ed25519_serge    IdentitiesOnly yes 
```

### Verify identity

```shell
ssh -vT   git@github.com      ...Hi jdedev! You've successfully authenticated, but GitHub does not provide   shell access....
```

### delete known_hosts entry line

#### delete line # 6
```shell
sed -i '6d' ~/.ssh/known_hosts
```

## SSH Key Management

### Generate SSH Key

```shell
ssh-keygen -t ed25519 -C "jdedev@gmail.com" -f id_ed25519_serge
```

### SSH Agent

- **List all identities**
```shell
ssh-add -l
```

- **Delete all identities**
```shell
ssh-add -D
```

- **Add an identity**
```shell
ssh-add ~/.ssh/id_ed25519_serge
```

### SSH Config

Edit `~/.ssh/config ` to specify identity for a host:

```shell
Host github.com
HostName github.com
User gitIdentityFile ~/.ssh/id_ed25519_serge
IdentitiesOnly yes
```

### Verify SSH Identity

```shell
ssh -vT git@github.com
# ...
# Hi jdedev! You've successfully authenticated, but GitHub does not provide shell access.
# ...
```

### Remove a known_hosts Entry

Delete line #6 from known_hosts:

```shell
sed -i '6d' ~/.ssh/known_hosts
```
