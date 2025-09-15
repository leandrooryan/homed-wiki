---
title: 'APT Репозиторий'
---

# APT Репозиторий

APT репозиторий позволяет устанавливать сервисы _HOMEd_ на оборудование, работающее под упрвлением [Debian Linux](https://debian.org) и другими дистрибутивами основанными на Debian Linux и использующими пакетный менеджер APT.

## Поддерживаемые архитектуры

- `aarch64`
- `arm64`
- `armhf`
- `amd64`

!!! warning ""

    Архитектура `amd64` поддерживается только для __Debian 11__ ("bullsye"), в других версиях Debian Linux установленные сервисы работать не будут. Для этих версий рекомендуется использовать __Docker__.

## Добавление GPG-ключа

```sh
sudo wget -O /etc/apt/trusted.gpg.d/debian-homed.asc \
  https://apt.homed.dev/apt.key
```

## Добавление репозитория

```sh
echo "deb https://apt.homed.dev/ debian main" | \
  sudo tee /etc/apt/sources.list.d/homed.list
```
