---
title: Yubikey Fido2 Setup
description: |
    How to setup docker for windows and kickstart on windows 10
---

# Setup YubiKey (FIDO2) for use in Docker Container

## Installation

```bash
sudo apt update
sudo apt install yubikey-agent
```

## Start den Yubikey Agent beim Booten

```bash
mkdir -p ~/.config/systemd/user
cat > ~/.config/systemd/user/yubikey-agent.service <<'EOF'
[Unit]
Description=YubiKey SSH Agent

[Service]
ExecStart=/usr/bin/yubikey-agent -l /run/user/%U/yubikey-agent.sock
Restart=on-failure

[Install]
WantedBy=default.target
EOF

systemctl --user daemon-reload
systemctl --user enable --now yubikey-agent
```


## Konfiguration Yubikey Agent

```bash
# einmalig in der aktuellen Shell:
export SSH_AUTH_SOCK=/run/user/$(id -u)/yubikey-agent.sock

# dauerhaft (bash/zsh rc):
echo 'export SSH_AUTH_SOCK=/run/user/$(id -u)/yubikey-agent.sock' >> ~/.bashrc
# oder ~/.zshrc entsprechend
```


## Import vorhandener SSH Keys

```bash
ssh-add -L            # zeigt Keys, die der Agent anbietet
```

Wenn Dein key hier nicht angezeigt wird:


```bash
ssh-add ~/.ssh/id_ed25519_sk    # oder sk-ecdsa Stub
```

