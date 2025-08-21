---
title: SSH-Agent / Yubikey Fido2 Setup
description: |
    How to setup docker for windows and kickstart on windows 10
---

# Setup YubiKey (FIDO2) for use in Docker Container

## Installation

```bash
sudo apt update
sudo apt install ssh-agent
```

## Autostart per .bashrc und import der Schlüssel

```bash
# ~/.bashrc or ~/.zshrc
# Start one instance of ssh-agent and add all keys
SOCK="$HOME/.ssh/agent.sock"
if [ ! -S "$SOCK" ]; then
  eval $(ssh-agent -a "$SOCK" -s) >/dev/null
  for key in ~/.ssh/id_*; do
    [ -f "$key" ] && [[ "$key" != *.pub ]] && ssh-add "$key"
  done
fi
export SSH_AUTH_SOCK="$SOCK"
``


## SSH Keys anzeigen:

```bash
ssh-add -L            # zeigt Keys, die der Agent anbietet
```

Wenn Dein key hier nicht angezeigt wird:


```bash
ssh-add ~/.ssh/id_ed25519_sk    # oder sk-ecdsa Stub
```


## Test im Container

```
kickstart
```

Sollte ganz oben die Ausgabe machen: Mounting SSH Agent Socket...

Im Container sollte /ssh-agent gesetzt sein und ein `ssh-add -l` sollte den schlüssel zeigen
